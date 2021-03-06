# keep-alive源码分析

## 一、keep-alive介绍与应用

### 1.1 keep-alive是什么

> keep-alive是一个抽象组件：它自身不会渲染一个DOM元素，也不会出现在父组件链中；使用keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。

#### 一个场景

用户在某个列表页面选择筛选条件过滤出一份数据列表，由列表页面进入数据详情页面，再返回该列表页面，我们希望：列表页面可以保留用户的筛选（或选中）状态。
 keep-alive就是用来解决这种场景。当然keep-alive不仅仅是能够保存页面/组件的状态这么简单，它还可以避免组件反复创建和渲染，有效提升系统性能。总的来说，**keep-alive用于保存组件的渲染状态。**

#### keep-alive用法

- 在动态组件中的应用

```xml
<keep-alive :include="whiteList" :exclude="blackList" :max="amount">
     <component :is="currentComponent"></component>
</keep-alive>
```

- 在vue-router中的应用

```xml
<keep-alive :include="whiteList" :exclude="blackList" :max="amount">
    <router-view></router-view>
</keep-alive>
```

**include**定义缓存白名单，keep-alive会缓存命中的组件；**exclude**定义缓存黑名单，被命中的组件将不会被缓存；**max**定义缓存组件上限，超出上限使用LRU的策略置换缓存数据。

> [内存管理](https://links.jianshu.com/go?to=https%3A%2F%2Fbaike.baidu.com%2Fitem%2F%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86%2F5633616)的一种页面置换算法，对于在内存中但又不用的[数据块](https://links.jianshu.com/go?to=https%3A%2F%2Fbaike.baidu.com%2Fitem%2F%E6%95%B0%E6%8D%AE%E5%9D%97%2F107672)（内存块）叫做LRU，操作系统会根据哪些数据属于LRU而将其移出内存而腾出空间来加载另外的数据。



## 二、源码剖析

keep-alive.js内部还定义了一些工具函数，我们按住不动，先看它对外暴露的对象

```kotlin
// src/core/components/keep-alive.js
export default {
  name: 'keep-alive',
  abstract: true, // 判断当前组件虚拟dom是否渲染成真实dom的关键
  props: {
      include: patternTypes, // 缓存白名单
      exclude: patternTypes, // 缓存黑名单
      max: [String, Number] // 缓存的组件
  },
  created() {
     this.cache = Object.create(null) // 缓存虚拟dom
     this.keys = [] // 缓存的虚拟dom的键集合
  },
  destroyed() {
    for (const key in this.cache) {
       // 删除所有的缓存
       pruneCacheEntry(this.cache, key, this.keys)
    }
  },
 mounted() {
   // 实时监听黑白名单的变动
   this.$watch('include', val => {
       pruneCache(this, name => matched(val, name))
   })
   this.$watch('exclude', val => {
       pruneCache(this, name => !matches(val, name))
   })
 },

 render() {
    // 先省略...
 }
}
```

可以看出，与我们定义组件的过程一样，先是设置组件名为keep-alive，其次定义了一个abstract属性，值为true。这个属性在vue的官方教程并未提及，却至关重要，后面的渲染过程会用到。props属性定义了keep-alive组件支持的全部参数。

keep-alive在它生命周期内定义了三个钩子函数：

- created
   初始化两个对象分别缓存VNode(虚拟DOM)和VNode对应的键集合
- destroyed
   删除this.cache中缓存的VNode实例。我们留意到，这不是简单地将this.cache置为null，而是遍历调用pruneCacheEntry函数删除。

```tsx
// src/core/components/keep-alive.js
function pruneCacheEntry (
  cache: VNodeCache,
  key: string,
  keys: Array<string>,
  current?: VNode
) {
 const cached = cache[key]
 if (cached && (!current || cached.tag !== current.tag)) {
    cached.componentInstance.$destroyed() // 执行组件的destroy钩子函数
 }
 cache[key] = null
 remove(keys, key)
}
```

删除缓存的VNode还要对应组件实例的destory钩子函数

- mounted
   在mounted这个钩子中对include和exclude参数进行监听，然后实时地更新（删除）this.cache对象数据。pruneCache函数的核心也是去调用pruneCacheEntry

```tsx
function pruneCache (keepAliveInstance: any, filter: Function) {
  const { cache, keys, _vnode } = keepAliveInstance
  for (const key in cache) {
    const cachedNode: ?VNode = cache[key]
    if (cachedNode) {
      const name: ?string = getComponentName(cachedNode.componentOptions)
      if (name && !filter(name)) {
        pruneCacheEntry(cache, key, keys, _vnode)
      }
    }
  }
}
```

- render

```kotlin
render () {
  const slot = this.$slots.defalut
  const vnode: VNode = getFirstComponentChild(slot) // 找到第一个子组件对象
  const componentOptions : ?VNodeComponentOptions = vnode && vnode.componentOptions
  if (componentOptions) { // 存在组件参数
    // check pattern
    const name: ?string = getComponentName(componentOptions) // 组件名
    const { include, exclude } = this
    if (// 条件匹配
      // not included
      （include && (!name || !matches(include, name))）||
      // excluded
        (exclude && name && matches(exclude, name))
    ) {
        return vnode
    }
    
    const { cache, keys } = this
    // 定义组件的缓存key
    const key: ?string = vnode.key === null ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '') : vnode.key
     if (cache[key]) { // 已经缓存过该组件
        vnode.componentInstance = cache[key].componentInstance
        remove(keys, key)
        keys.push(key) // 调整key排序
     } else {
        cache[key] = vnode //缓存组件对象
        keys.push(key)
        if (this.max && keys.length > parseInt(this.max)) {
          //超过缓存数限制，将第一个删除
          pruneCacheEntry(cahce, keys[0], keys, this._vnode)
        }
     }
     
      vnode.data.keepAlive = true //渲染和执行被包裹组件的钩子函数需要用到
 
 }
 return vnode || (slot && slot[0])
}
```

- 第一步：获取keep-alive包裹着的第一个子组件对象及其组件名；
- 第二步：根据设定的黑白名单（如果有）进行条件匹配，决定是否缓存。不匹配，直接返回组件实例（VNode），否则执行第三步；
- 第三步：根据组件ID和tag生成缓存Key，并在缓存对象中查找是否已缓存过该组件实例。如果存在，直接取出缓存值并更新该key在this.keys中的位置（更新key的位置是实现LRU置换策略的关键），否则执行第四步；
- 第四步：在this.cache对象中存储该组件实例并保存key值，之后检查缓存的实例数量是否超过max设置值，超过则根据LRU置换策略删除最近最久未使用的实例（即是下标为0的那个key）;
- 第五步：最后并且很重要，将该组件实例的keepAlive属性值设置为true。



## 三、重头戏：渲染

### 3.1 Vue的渲染过程

借助一张图看下Vue渲染的整个过程：

![img](https:////upload-images.jianshu.io/upload_images/4191769-13841e040b205889.png?imageMogr2/auto-orient/strip|imageView2/2/w/724/format/webp)

vue渲染过程.png


 Vue的渲染是从图中render阶段开始的，但keep-alive的渲染是在patch阶段，这是构建组件树（虚拟DOM树），并将VNode转换成真正DOM节点的过程。
**简单描述从render到patch的过程**
 我们从最简单的new Vue开始：

```jsx
import App from './App.vue'

new Vue({
    render: h => h(App)
}).$mount('#app')
```

- Vue在渲染的时候先调用原型上的_render函数将组件对象转化成一个VNode实例；而_render是通过调用createElement和createEmptyVNode两个函数进行转化；

- createElement的转化过程会根据不同的情形选择new VNode或者调用createComponent函数做VNode实例化；

- 完成VNode实例化后，这时候Vue调用原型上的_update函数把VNode渲染成真实DOM，这个过程又是通过调用

  patch函数完成的（这就是patch阶段了）

  用一张图表达：

  ![img](https:////upload-images.jianshu.io/upload_images/4191769-88a821c34ada7669.png?imageMogr2/auto-orient/strip|imageView2/2/w/878/format/webp)

  渲染过程.png

### 3.2 keep-alive组件的渲染

我们用过keep-alive都知道，它不会生成真正的DOM节点，这是怎么做到的？

```php
// src/core/instance/lifecycle.js
export function initLifecycle (vm: Component) {
    const options= vm.$options
    // 找到第一个非abstract父组件实例
    let parent = options.parent
    if (parent && !options.abstract) {
        while (parent.$options.abstract && parent.$parent) {
              parent = parent.$parent
        }
        parent.$children.push(vm)
    }
    vm.$parent = parent
    // ...
}
```

Vue在初始化生命周期的时候，为组件实例建立父子关系会根据abstract属性决定是否忽略某个组件。在keep-alive中，设置了abstract:true，那Vue就会跳过该组件实例。

**最后构建的组件树中就不会包含keep-alive组件，那么由组件树渲染成的DOM树自然也不会有keep-alive相关的节点了。**

**keep-alive包裹的组件是如何使用缓存的？**
 在patch阶段，会执行createComponent函数：

```csharp
// src/core/vdom/patch.js
function createComponent (vnode, insertedVnodeQueue, parentElm, refElm) {
    let i = vnode.data
    if (isDef(i)) {
        const isReactivated = isDef(vnode.componentInstance) && i.keepAlive
        if (isDef(i = i.hook) && isDef(i = i.init)) {
            i(vnode, false)
        }
        if (isDef(vnode.componentInstance)) {
            initComponent(vnode, insertedVnodeQueue)
            insert(parentElem, vnode.elem, refElem) // 将缓存的DOM(vnode.elem) 插入父元素中
            if (isTrue(isReactivated)) {
                reactivateComponent(vnode, insertedVnodeQueue, parentEle, refElm)
            }
            return true
        }
    }
}
```

- 在首次加载被包裹组建时，由keep-alive.js中的render函数可知，vnode.componentInstance的值是undfined，keepAlive的值是true，因为keep-alive组件作为父组件，它的render函数会先于被包裹组件执行；那么只执行到i(vnode,false)，后面的逻辑不执行；
- 再次访问被包裹组件时，vnode.componentInstance的值就是已经缓存的组件实例，那么会执行insert(parentElm, vnode.elm, refElm)逻辑，这样就直接把上一次的DOM插入到父元素中。



## 四、不可忽视：钩子函数

### 4.1 只执行一次的钩子

一般的组件，每一次加载都会有完整的生命周期，即生命周期里面对于的钩子函数都会被触发，为什么被keep-alive包裹的组件却不是呢？
 被缓存的组件实例会为其设置keepAlive= true，而在初始化组件钩子函数中：

```tsx
// src/core/vdom/create-component.js
const componentVNodeHooks = {
    init (vnode: VNodeWithData, hydrating: boolean): ?boolean{
        if (
         vnode.componentInstance &&       
         !vnode.componentInstance._isDestroyed &&
         vnode.data.keepAlive
        ) {
          // keep-alive components, treat as a patch
          const mountedNode:any = vnode
          componentVNodeHooks.prepatch(mountedNode, mountedNode)
        } else {
          const child = vnode.componentInstance = createComponentInstanceForVnode (vnode, activeInstance)
           
        }
    }
}
```

可以看出，当vnode.componentInstance和keepAlive同时为true时，不再进入$mount过程，那mounted之前的所有钩子函数（beforeCreate、created、mounted）都不再执行。

### 4.2 可重复的activated

在patch的阶段，最后会执行invokeInsertHook函数，而这个函数就是去调用组件实例（VNode）自身的insert钩子：

```cpp
// src/core/vdom/patch.js
function invokeInsertHook (vnode, queue, initial) {
      if (isTrue(initial) && isDef(vnode.parent)) {
          vnode.parent.data,pendingInsert = queue
      } else {
         for(let i =0; i<queue.length; ++i) {
                queue[i].data.hook.insert(queue[i]) // 调用VNode自身的insert钩子函数
         }
      }
}
```

再看insert钩子：

```csharp
const componentVNodeHooks = {
      // init()
     insert (vnode: MountedComponentVNode) {
           const { context, componentInstance } = vnode
           if (!componentInstance._isMounted) {
                 componentInstance._isMounted = true
                 callHook(componentInstance, 'mounted')
           }
           if (vnode.data.keepAlive) {
                 if (context._isMounted) {
                     queueActivatedComponent(componentInstance)
                 } else {
                      activateChildComponent(componentInstance, true/* direct */)
                 }
          }
         // ...
     }
}
```

在这个钩子里面，调用了activateChildComponent函数递归地去执行所有子组件的activated钩子函数：

```jsx
// src/core/instance/lifecycle.js
export function activateChildComponent (vm: Component, direct?: boolean) {
  if (direct) {
    vm._directInactive = false
    if (isInInactiveTree(vm)) {
      return
    }
  } else if (vm._directInactive) {
    return
  }
  if (vm._inactive || vm._inactive === null) {
    vm._inactive = false
    for (let i = 0; i < vm.$children.length; i++) {
      activateChildComponent(vm.$children[i])
    }
    callHook(vm, 'activated')
  }
}
```

相反地，deactivated钩子函数也是一样的原理，在组件实例（VNode）的destroy钩子函数中调用deactivateChildComponent函数

