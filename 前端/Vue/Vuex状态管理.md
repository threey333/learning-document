# Vuex状态管理

## 一、什么组件的状态

### 1.1 组件的状态

​		一个完整的Vue项目是由各个组件所组成的，每个组件在用户界面上的显示是由**组件内部的 `属性` 和 `逻辑`** 所决定的，==**我们把这种属性和逻辑叫做组件的状态。**==

​		**组件之间的相互通信可以用来改变组件的状态。**

>注意：
>
>- 如果项目结构简单，父子组件之间的数据传递可以使用`props`或者`$emit`等方式，但是对于大型项目应用来说，由于组件众多，`组件的状态`零散地分布在各个组件之间的交互操作中，这样不太好管理。
>- 为了解决这个问题，我们可以在大型项目中引入`Vuex`来进行状态管理。
>- `Vuex`是一个很好的Vue状态管理模式。



## 二、什么是 “状态管理模式”

### 2.1 状态管理模式示例

代码示例：

```js
new Vue({
  el: '#app',
  // state
  data() {
    return {
      count: 0,
    }
  },
  // view
  template:
  '<div>{{count}}<button @click="increment">按钮</button></div>',

  // actions
  methods: {
    increment() {
      this.count++
    },
  },
})
```

在上面的代码中，完成了一个计数的逻辑，当`increment`方法被不断调用时，`count`的值就会不断增加并显示在页面上，我们称其为==**"状态自管理"**==

图示：

![image-20210722011113894](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004833.png)

图中各部分的含义：

- `state`：驱动应用的数据源。
- `view`：以声明方式将`state`映射到视图上。
- `action`：响应在view上**用户交互操作**导致的`state`状态变化，也就是数据发生变化。

>注意：
>
>- 从图中可以发现，这个过程是一个单向的过程，从`view`上触发`action`改变`state`，`state`的改变最终回到了`view`上，这种叫做“**单向数据流**”。



### 2.2 单向数据流会遇到的问题

如上图所示的“**单向数据流**”也会遇到问题，比如：当我们的应用遇到多个组件共享`state状态`时，假如有另外3个Vue计数器实例都依赖于这个`state`，并在`state`改变时做到同步的UI改变，这时这种**单向数据流**的简洁性会很容易受到破坏，会出现下述问题：

- 一、多个组件依赖于同一状态。
- 二、来自不同组件的行为需要变更同一状态。

>​		对于问题一，如果使用 之前传参的方法来解决，对于多层嵌套的组件将会非常烦琐，并且对于兄弟组件之间的状态传递无能为力。
>
>​		对于问题二，可采用==父子组件直接引用==或者==通过事件来变更==和==同步状态的多份复制==的方法来解决。
>
>​		**以上的这些方案虽然可以在一定程度上解决这些问题，但是却非常脆弱，通常会导致很多无法维护的代码出现。**



### 2.3 Vuex的设计思想

​		从`单向数据流`遇到的问题中思考，为什么我们不能把组件的==**共享状态**==抽取出来，以一个全局单例模式管理呢？在这种模式下，**所有的组件通过树的方式构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取`状态`或者`触发事件`。**

​		通过定义状态管理中的各种概念，并通过强制规则来维持视图和状态间的独立性，让代码变得更结构化且易于维护。这就是`Vuex`的设计思想。

图示理解：

![image-20210722020617828](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004840.png)



## 三、Vuex概述

### 3.1 Vuex的组成

#### # Vuex的组成部分

![image-20210724221857618](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004844.png)

**在Vuex中用的最多的有：`Store`、`mapActions`、`mapGetters`、`mapMutations`、`mapState`**。



#### # Vuex的Store

每一个Vuex应用都有一个巨大的"视图"，这个视图的核心叫作 `Store(仓库)`。

**`Store` 基本上就是一个数据的容器，它包含着应用中大部分的状态(我的理解就是数据)，所有组件之间的状态改变都需要告诉 `Store`，然后再由 `Store` 负责分发到各个组件。**

抽象一点来说，`Store ` 就像是一个全局的对象，可简单地理解成window对象下的一个对象，组件之间的`通信`和`状态改变`都可以通过全局对象来调用，但是 `Store  ` 和全局对象还是有一些本质区别，并且也更加复杂。

>Vuex的`Store`中有默认的**5种基本的对象**：
>
>- `state`：存储状态，是一个对象，其中的每一个key就是一个状态。
>- `getter`：表示在数据获取之前的再次编译和处理，可以理解为state的计算属性。
>- `mutation`：修改状态，并且是同步的。
>- `action`：修改状态，可以是异步的。
>- `module`：`Store`分割后的模块，为了开发大型项目，可以让每一个模块拥有自己的`state`、`getter`、`mutation`、`action`,使得结构更加清晰，方便管理。但不是必须使用的。

![image-20211215230013023](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211215230021.png)

上面这些对象之间的工作流程如图所示：

![image-20210722031921449](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004848.png)

上面的流程图中虽然没有标出Store,但是从中可以看出，**`Vuex` 是一个抽象的概念，而`Store`是一个具体的对象，在代码中真正使用的是`Store`。**这个对象（指的是Store）要比一般的全局对象复杂的多。

>`Vuex` 和`单纯的全局对象`有以下两点不同：
>
>1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
>2. ==你不能直接改变 store 中的状态。==改变 store 中的状态的唯一途径就是显式地 **提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。



### 3.2 安装Vuex

方法一：使用<script>标签的方式导入Vuex，**前提是必须先导入Vue.js**。

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script src="https://unpkg.com/vuex@3.6.2/dist/vuex.js"></script>
```

方法二：使用npm安装Vuex。

```bash
npm install vuex --save
```

方法三：使用Yarn安装Vuex。

```bash
yarn add vuex
```

**在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex：**

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex) // ==>这样在Vue实例对象才能使用该插件
```



>**==注意：==**
>
>Vuex 依赖 [Promise (opens new window)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)。如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，例如 [es6-promise (opens new window)](https://github.com/stefanpenner/es6-promise)。
>
>你可以通过 CDN 将其引入：
>
>```html
><script src="https://cdn.jsdelivr.net/npm/es6-promise@4/dist/es6-promise.auto.js"></script>
>```
>
>然后 `window.Promise` 会自动可用。
>
>如果你喜欢使用诸如 npm 或 Yarn 等包管理器，可以按照下列方式执行安装：
>
>```bash
>npm install es6-promise --save # npm
>yarn add es6-promise # Yarn
>```
>
>或者更进一步，将下列代码添加到你使用 Vuex 之前的一个地方：
>
>```js
>import 'es6-promise/auto'
>```
>
>



### 3.3 一个简单的Store案例

代码示例：

```html
<script src="./Vue资源文件/vue.js"></script>
<script src="./Vue资源文件/vuex.js"></script>
<script>
  const store = new Vuex.Store({
    state: {
      count: 0,
    },
    mutations: {
      increment:function (state) {
        state.count++
      },
    },
  })
  store.commit('increment')
  console.log(store.state.count)
</script>
```

**==注意：commit方法的参数就是store中定义得到mutation的key值。==**

![image-20210722042645041](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004855.png)

代码演示图示：

![image-20210722040130457](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004859.png)

为了在 Vue 组件中能访问到 `this.$store` property，我们需要为 Vue 实例提供创建好的 store。做法如下：

```js
new Vue({
  el: '#app',
  store: store,
  //ES6 则是执行属性简洁表示法store.
})
```



### 3.4 与Vue组件简单配合使用案例

代码：在 `App.vue` 组件中：

```html
<template>
  <div id="app">
    <button @click="add">点我+3</button>
    <h1>{{count}}</h1>
  </div>
</template>

<script>
export default {
  name: 'App',
  props: {
    result: {
      type: String,
      default: 'You look so good'
    }
  },
  data () {
    return {
      title: 'hello world'
    }
  },
  computed: {
    ...mapState({ count: state => state.count }),
    fullName () {
      return this.$store.getters.fullName
    }
  },
  methods: {
    add () {
      this.increment({ num: 3 })
    },
    ...mapMutations({
      increment: 'increment'  // =>这里使用别名，别名的键值对应的是mutations中的方法
    })
  }
}
```

在 `mutations.js` 中：

```js
export default {
  increment (state, { num }) {
    state.count += num
  }
}

//=>结果是每次点击按钮都会+3
```



### 3.5 总结

再次强调，我们通过`提交 mutation `的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。此外，这样也让我们有机会去实现一些能记录每次状态改变，保存状态快照的调试工具。有了它，我们甚至可以实现如时间穿梭般的调试体验。

**由于 store 中的状态是响应式的，在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可。触发变化也仅仅是在组件的 methods 中提交 mutation。**

```vue
在Vue组件中
...
computed:{
  info(){
    return this.$store.state.info
  }
}，
methods:{
  changeInfo(){
    this.$store.commit('changeInfo')
  }
}
```



## 四、State

### 4.1 State的用法

`state`的主要作用是保存状态，通俗来讲，状态就是由 **"键-值对"**组成的对象。

> **在Vue组件中，读取状态最简单的方法就是在计算属性中返回某个状态。**

代码示例：

```js
<div id="app">
  <counter />
  </div>
<script src="./Vue资源文件/vue.js"></script>
<script src="./Vue资源文件/vuex.js"></script>
<script>
  // 定义store
  const store = new Vuex.Store({
    // 状态是由键值对所组成的对象
    state: {
      count: 3,
    },
    mutations: {
      increment: function (s) {
        s.count++
      },
    },
  })

// 注册子组件
const counter = {
  template: `<div><h1>{{count}}</h1></div>`,
  computed: {
    count: function () {
      return this.$store.state.count //通过this.$store.state可以获取state
    },
  },
}

const vm = new Vue({
  el: '#app',
  store,
  components: {
    counter,
  },
})
</script>
```

结果图示：

![image-20210723003044357](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004908.png)



### 4.2 State的总结

总结：

一、**==在Vue的根实例中注册store选项，该store实例就会注入到根组件下的所有子组件中。==**

二、通过这样，所有的子组件能通过 `this.$store`访问到 `store`选项，然后更新store和counter组件的实现。

![image-20210723004107264](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004927.png)

更新：

![image-20210723004139954](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004946.png)

![image-20210723004159252](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004950.png)

三、将Store中的`state`赋值给计算属性，这样能构成一条响应式的链路，一旦store中的`state`发生变化，就会触发计算属性也发生变化，这样就能达到==动态更新==。

```js
computed: {
  count: function () {
    return this.$store.state.count //通过this.$store.state可以获取state
  },
}
```

**简单来说就是：今后开发的时候，为了方便使用store中的状态数据，可以把state状态数据赋值给某个组件中的计算属性。**



### 4.3 mapState辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为**计算属性**会有些重复和冗余。为了解决这个问题，我们可以使用 **`mapState`** 辅助函数帮助我们生成计算属性，这样能减轻我们的工作量。

==**注意：mapState辅助函数的参数必须数组或对象**==

>**提示：**
>
>- `mapState`辅助函数是Vuex全局对象下的一个方法。
>
>  ```js
>  Vuex.mapState
>  ```
>
>  ![image-20210723013310086](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004957.png)
>
>  ![image-20210723013229865](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005003.png)
>
>  

代码示例：

```html
<div id="app">
  <template>
    <h1>{{countPlusLocalCount}}</h1>
  </template>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
})
const vm = new Vue({
  el: '#app',
  store,
  data() {
    return {
      localCount: 4,
    }
  },
  computed: Vuex.mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,
    // countAlias即计算别名，这里传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',
    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalCount: function (state) { 
      return state.count + this.localCount    //两个相加为7
    },
  }),
})
```

结果图示：

![image-20210723015453576](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005008.png)



#### # mapState辅助函数的参数是对象形式

当 `mapState` 辅助函数的参数是对象形式时，有三种写法：

1. 使用箭头函数写法（优点：简洁。缺点：不能获取当前组件的this）
2. 起别名写法
3. 使用普通函数写法 （优点：可以获取当前组件this，然后可以获取当前组件的状态数据）

```js
// 第一种：使用箭头函数写法
computed: {
  ...mapState({
    count: state => state.count
  }), // 使用箭头函数的形式
},

  // 第二种：起别名写法
computed: {
  ...mapState({
    // count: state => state.count
    count: 'count'
  }), 
}


// 第三种：使用普通函数的写法
computed: {
  ...mapState({
    count: function (state) {
      return state.count + this.localCount
    }
  }),
}
```

==注意：==使用箭头函数写法和普通函数写法的时候，key的名称可以自己起。



#### # mapState辅助函数的参数是数组形式

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个**字符串数组**.

代码示例：

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
})
const vm = new Vue({
  el: '#app',
  store,
  data() {
    return {
      localCount: 4,
    }
  },
  computed: Vuex.mapState([
    // 映射 this.count 为 store.state.count
    'count'
  ]),
})
```



### 4.4 mapState辅助函数中的别名

有时候，我们映射的时候，想给计算属性起个新的名字,不想用原有的属性名，那就可以像下面这样做，用对象的形式取别名：

```html
<div id="app">{{newName}} {{newage}}</div>
```

```js
const store = new Vuex.Store({
  state: {
    name: '生煎火龙果',
    age: 30,
  },
})

const vm = new Vue({
  store,
  el: '#app',
  computed: Vuex.mapState({
    // 如果想给计算属性起一个新的名字，可以使用别名
    newName: 'name',
    newage: 'age',
  }),
})
```

结果图示：

![image-20210723162256862](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005027.png)





### 4.5 mapState函数利用对象扩展运算符将其与computed属性一起使用

#### # 用法原理说明

**`mapState` 函数返回的是一个对象。**

利用**对象扩展运算符**将`mapState`返回的最终对象传给`computed`计算属性。这样就能将`mapState`函数与`computed`属性一起使用。

```js
computed: {
  localComputed () { /* ... */ },
    // 使用对象展开运算符将此对象混入到外部对象中
    ...mapState({
      // ...
    })
}
```

> **原理：**
>
> **`computed`属性对应的就是一个对象，在对象里面使用对象扩展运算符将 `mapState` 函数返回的对象里面所有可遍历的属性，拷贝到当前对象之中。**

原理简单示例如下：

```js
let obj1 = {
  a: 3,
  b: 5
};
console.log({ ...obj1 });   
结果  //{a: 3, b: 5}
```



#### # 具体用法示例

**具体用法示例代码：**

```html
<div id="app">
  <h1>{{title}}</h1>
  <h2>{{totalTitle}}</h2>
  <h3>{{sumcount}}</h3>
</div>
```

```js
// 对象实例化store
const store = new Vuex.Store({
  state: {
    count1: 10,
    count2: 20,
    title: 'mapState辅助函数配合computed属性使用',
  },
})

const vm = new Vue({
  store,
  el: '#app',
  data() {
    return {
      name: 'yyy',
    }
  },
  computed: {
    title() {
      return this.$store.state.title
    },
    // mapState函数返回的是一个对象
    ...Vuex.mapState({
      totalTitle: function (state) {
        return state.title + this.name
      },
      sumcount: state => state.count1 + state.count2,
    }),
  },
})
```

在脚手架项目开发中，`mapState` 辅助函数需要先导入进来再使用。

```js
import { mapState } from 'vuex'  // => 从vuex解构出mapState辅助函数，然后导入到当前组件中使用。
export default {
  name: 'App',
  components: {
    tabBar
  },
  data() {
    return {
      name: 'yyy',
    }
  },
  computed: {
    title() {
      return this.$store.state.title
    },
    // mapState函数返回的是一个对象
    ...mapState({
      totalTitle: function (state) {
        return state.title + this.name
      },
      sumcount: state => state.count1 + state.count2,
    }),
  },
},
```

结果示例：

![image-20210723023852553](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005033.png)



## 五、Getters

### 5.1 getters的使用

- 如果需要对`Store`中`state状态数据`进行**二次加工**或者**添加一些业务逻辑**，那么这些操作只能在**各自组件的computed属性中进行。**

- 但是如果各组件都需要进行这个相同操作，那么就需要重复多次写，这样会显得很麻烦，这时Vuex中的`Getters对象`就能很好解决这个问题。
- Vuex 允许我们在 store 中定义 `“getters”`（可以认为是 store 的计算属性）。**就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。**

getters的使用：

```html
<div id="app">
  <h1>{{totalcount}}</h1>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
  getters: {
    getFormatCount: function (state) {
      // 对状态数据进行二次加工
      let str = '物料总价:' + state.count * 10 + '元'
      return str
    },
  },
})

const vm = new Vue({
  el: '#app',
  computed: {
    totalcount: function () {
      return this.$store.getters.getFormatCount
    },
  },
  store,
})
```

图示：

![image-20210723173432190](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005038.png)



### 5.2 Getters 可以接受其他 getter 作为第二个参数

`Getters` 中的方法除了有`state`参数之外，也可以接收第二个参数 `getter`，这样就能调用其它 `getter` 的方法，达到复用的效果。

getters方法第二个参数代码示例如下：

```js
getters:{
  otherCount:function(){...},
  getFormatCount:function(state,getters){
      return state.count+getters.otherCount
   }
}
```

用法代码：

```js
const store = new Vuex.Store({
  state: {
    name1: '油炸皮卡丘',
    name2: '蟹汉堡王',
  },
  getters: {
    // 对state状态数据进行二次加工
    sumname: function (state) {
      return `我要吃${state.name1}`
    },
    totalname: function (state, getters) {
      return getters.sumname + '和' + state.name2
    },
  },
})
new Vue({
  el: '#app',
  store,
  computed: {
    totalsum() {
      return this.$store.getters.totalname
    },
  },
})
```

结果图示：

![image-20210723203657740](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005044.png)



### 5.3 getters计算属性返回一个函数

**在Vue组件中通过让getter返回一个函数，来实现给getter传参。**

代码演示：

在 `src/store/index.js` 中：

```js
import Vue from 'vue'
import Vuex from 'vuex'
import state from './state.js'
import getters from './getters.js'
import mutations from './mutations.js'

Vue.use(Vuex)

export default new Vuex.Store({
  state,
  getters,
})
```

在组件文件中：

```vue
<template>
  <div id="app">
    <h1>{{ total }}</h1>
    <h1>{{ total }}</h1>
    <h1>{{ total }}</h1>
    <h1>{{ total }}</h1>
    <h1>{{ total }}</h1>
  </div>
</template>

export default {
  name: 'App',
  computed: {
    total () {
      return this.$store.getters.getFormatCount(30) // => 传递参数30
    }
  },
}
```

在 `getters.js` 中：

```js
export default {
  getFormatCount (state) {
    console.log('执行了1')
    return function (unit) {
      console.log('执行了2')
      return `物价总价:${state.count * unit}元`
    }
  }
}
```

 结果图示：

![image-20211216224934151](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20211216224934151.png)

>**提示：**
>
>**在getters方法中通过return返回一个函数function，有时有利于你在store中查询某个符合条件的state状态数组数据。**

代码示例：

```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false },
    ],
  },
  getters: {
    getTodoById: state => id => {
      return state.todos.find(item => item.id === id)
    },
  },
})
new Vue({
  el: '#app',
  store,
  computed: {
    selecttodo() {
      return this.$store.getters.getTodoById(2)
    },
  },
})
```

结果图示：

![image-20210723225108310](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005106.png)

**==注意：getters在通过方法访问时，每次都会取调用方法，而不是读取缓存的结果。==**



### 5.4 mapGetters辅助函数

#### # mapGetters辅助函数的使用

`mapGetters` 辅助函数仅仅是将 `store` 中的 `getters` 映射到局部计算属性：

```html
<div id="app">
  <h1>{{totaltitle}}</h1>
  <h1>{{doneTodos}}</h1>
  <h1>{{author}}</h1>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    todoList: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false },
      { id: 3, text: '...', done: true },
    ],
    name: 'threey',
  },
  getters: {
    doneTodos(state) {
      return state.todoList.filter(item => item.done).length
    },
    author(state) {
      return state.name + '最棒啊！'
    },
  },
})
new Vue({
  el: '#app',
  store,
  data() {
    return {
      title: 'mapGetters',
    }
  },
  computed: {
    totaltitle() {
      return this.title + '辅助函数的使用'
    },
    // 使用对象扩展运算符将 getter 混入 computed 对象中
    ...Vuex.mapGetters(['doneTodos', 'author']),
  },
})
```

结果：

![image-20210723232337843](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005115.png)

#### # getter属性取别名

如果你想将一个 getter 属性另取一个名字，使用**对象形式**：

```js
...mapGetters({
  // => this.otherauthorname 映射为 this.$store.getters.author
   otherauthorname: 'author',
})
```



## 六、Mutations

### 6.1 理解Mutations

**更改 Vuex 的 store 中的状态的唯一方法是`提交mutation`。**

Vuex 中的 `mutations` 非常类似于事件：每个 mutation 都有一个字符串的 **`事件类型 (type)`** 和 一个 **`回调函数 (handler)`**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 `state` 作为第一个参数：

代码示例：

```html
<div id="app">
  <h1>{{changecount}}</h1>
  <button @click="clickCallback">增加</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 1,
  },
  mutations: {
    increment(state) {
      // 变更状态
      state.count++
    },
  },
})

const vm = new Vue({
  el: '#app',
  store,
  computed: {
    changecount() {
      return this.$store.state.count
    },
  },
  methods: {
    clickCallback() {
      // 触发一个类型为increment的mutation时就会调用该函数
      this.$store.commit({
        type: 'increment',
      })
    },
  },
})
```

结果：

![image-20210724001926201](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005123.png)

>注意点：
>
>- 你不能直接调用一个 `mutation handler`。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，则就会调用此函数。”要唤醒一个 mutation handler，你需要以相应的` type` 调用 **store.commit** 方法：
>
>  ```js
>  store.commit('increment')
>  ```
>
>- `type`则相当于`mutation`的事件类型，即 `store`中定义的`mutations`的一个==key值==，如上面代码中 `increment`就是一个key值。



### 6.2 Mutations第二个参数是必须一个对象形式

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**

>载荷就是`commit方法`除了第一个参数外，其它参数都被当作是载荷，这样在store的mutations方法中就可以获取该数据

示例：

```js
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```js
store.commit('increment', 10)
```

在大多数情况下，`commit方法`的载荷应该是一个==对象==，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}

```

```js
store.commit('increment', {
  amount: 10
})
```



### 6.3  对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 `mutation 函数`，因此 handler 保持不变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```



### 6.4 Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp': 123)`, 或者

- 以新对象替换老对象。例如，利用**ES6的对象扩展运算符**我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```



###  6.5 Mutation 必须是同步函数

一条重要的原则就是要记住 **`mutation` 必须是同步函数**。

为什么必须是同步函数，我们来用一个例子说明：

```html
<div id="app">
  <h1>{{totalCount}}</h1>
  <button @click="clickCallback">增加</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 2,
  },
  mutations: {
    someMutation(state) {
      setTimeout(() => {
        state.count++
      }, 1000)
    },
  },
})
const vm = new Vue({
  el: '#app',
  store,
  computed: {
    totalCount() {
      return this.$store.state.count
    },
  },
  methods: {
    clickCallback() {
      this.$store.commit('someMutation')
    },
  },
})
```

> **提示：**
>
> 解析这个例子之前先理清楚**异步函数和回调函数的区别：**
>
> - 异步任务函数一定有回调函数。
> - 回调函数不一定是异步任务函数，也有可能是同步任务函数。

所以上面这个例子中，当回调函数`setTimeout`触发`state.count++`时，可以明显地看到在延时1秒之后，状态改变了，这看起来确实可以达到效果，但是Vue并不推荐这样做，这是由于之前我们提到过的==Chrome浏览器的VueDevTools==。VueDevTools如下图所示：

![image-20210724220350974](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005131.png)

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 `mutation 日志`。每一条 mutation 都被正常记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调打破了这个机制，让调式工作无法完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——==**实质上任何在回调函数中进行的状态改变都是不可追踪的。**==

![image-20210724221316463](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005137.png)



### 6.6 Vuex中的mapMutations辅助函数

`mapMutations辅助函数` 返回的是一个对象。

#### # 数组形式使用mapMutations

当映射的`事件类型（即type）`与mutations中的`key值`相同，则可以**数组的形式**使用。

```html
<div id="app">
  <h1>{{count}}</h1>
  <h1>{{title}}</h1>
  <button @click="increase(10)">点击一下</button>
  <button @click="totaltitle">点我</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
    title: '油炸皮卡丘',
  },
  mutations: {
    increase(state, params) {
      // frequency:次数
      console.log(params)
      state.count = state.count * params
    },
    totaltitle(state) {
      state.title = state.title + ',我喜欢吃哈哈哈'
    },
  },
})
const vm = new Vue({
  el: '#app',
  store,
  computed: {
    count() {
      return this.$store.state.count
    },
    title() {
      return this.$store.state.title
    },
  },
  methods: {
    ...Vuex.mapMutations([
      // 将 `this.increase()` 映射为 `this.$store.commit('increase')`
      'increase',
      // 这里`this.totaltitle()` 映射为 `this.$store.commit('totaltitle')`
      'totaltitle',
    ]),
  },
})
```

验证过程示意图：

![image-20210725020246411](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005151.png)

点击变化后：

![image-20210725020406998](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005155.png)



#### # 对象形式使用mapMutations

使用对象形式将`mapMutations`混入`methods`中，可以对其取**"别名"**。

还是拿上面的例子演示：

```js
...Vuex.mapMutations({
  addCount: 'increase',
  reviesTitle: 'totaltitle',
}),
// 这里是将 
// this.addCount()` 映射为 `this.$store.commit('increase')
// this.reviesTitle()` 映射为 `this.$store.commit('reviesTitle')
```

这样是也可行的。



### 6.7 Mutation总结

在Vuex中，`mutation`都是同步事务，如果要解决异步问题，则需要Vuex中的`action`。



## 七、Actions

### 7.1 了解Actions

`Action` 类似于 `mutation`，不同在于：

- `Action` 提交的是 `mutation`，而==不是直接变更状态==。
- `Action` ==可以包含任意异步操作==。

可以这么理解，**为了解决异步更改state的问题**，需要在 mutation前加一层 `action`,我们直接操作 `action`，然后让 `action`操作 `mutation`。

![image-20210725031111594](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005202.png)

示例代码：

```html
<div id="app">
  <h1>{{title}}</h1>
  <h1>{{count}}</h1>
  <button @click="clickCallback">点击一下</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
  mutations: {
    increament(state, params) {
      state.count = state.count + params.num
    },
  },
  actions: {
    increamentAction(context, params) {
      // 在action里面会去调用mutations
      context.commit('increament', params)
    },
  },
})

const vm = new Vue({
  el: '#app',
  store,
  data: {
    title: 1,
  },
  computed: {
    count() {
      return this.$store.state.count
      // 通过this.$store.state可以获取state
    },
  },
  methods: {
    clickCallback() {
      // 通过this.$store.dispatch调用action
      this.$store.dispatch('increamentAction', {
        num: 4,
      })
    },
  },
})
```

`Action 函数`接受一个与 **store 实例具有相同方法和属性的 context 对象**，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

结果图示：

![image-20210725031030881](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005209.png)

**技巧提示：**

实践中，我们会经常用到 `函数参数中的对象解构` 来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```js
actions: {
  // => 从context这个对象中解构出来个commit
  increment ({ commit }) {
    commit('increment')
  }
}
```



### 7.2 Actions内部执行异步操作

由于 `mutation`必须同步执行这个限制，所以异步函数操作只能放在`actions`内部进行。

代码示例：

```js
...
actions: {
  increamentAction(context, params) {
    setTimeout(() => {
      context.commit('increament', params)
    }, 1000)
  },
},
```

> **结果：**
>
> **把异步操作逻辑放在`actions`中，这样对于`mutation`其实是同步的，Chrome浏览器的Vue DevTools也可以追踪到每一次的`state状态数据`的改变。**



### 7.3 Actions的第二个参数必须是一个对象

`Actions` 的第二个参数必须是一个对象。我们在实际开发中可以从该对象解构出来一些我们想要的数据。 

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})
```



### 7.4 以对象方式进行dispatch分发

当 `dispatch` 分发的时候，第一个参数是 `actions` 方法名，第二个参数是对象。

```js
// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})

//或者 
store.dispatch('incrementAsync',{ amount:10 })
```



### 7.5 Vuex中的mapActions辅助函数

在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 `methods` 映射为 `store.dispatch` 调用（需要先在根节点注入 `store`）：

```js
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    //数组形式
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    
    //对象形式
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}

//使用的时候：
this.add({参数}) 或 this.increment({参数})
```



### 7.6 Actions中的处理函数返回的是Promise对象

在`Actions`中返回一个 `Promise对象`，以便准确地获取异步`Actions`执行完成后的时间点：

```html
<div id="app">
  <h1>{{count}}</h1>
  <button @click="clickCallback">点击一下</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
  mutations: {
    increment(state, params) {
      state.count = state.count + params.num
    },
  },
  actions: {
    incrementAction(context, params) {
      // 在action中返回一个Promise对象的情况
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          context.commit('increment', params)
          resolve('1')
        }, 1000)
      })
    },
  },
})

const vm = new Vue({
  el: '#app',
  store,
  computed: {
    count() {
      return this.$store.state.count //通过this.$store.state可以获取state
    },
  },
  methods: {
    clickCallback() {
      // 通过this。$store.dispatch调用action
      // 而且store.dispatch可以处理被触发的action的处理函数返回的promise,并且store.dispatch 仍旧返回Promise：

      this.$store
        .dispatch('incrementAction', {
        num: 4,
      })
        .then(value => console.log(value))
    },
  },
})
```



### 7.7 Actions内部触发另外一个action

在一个`actions`内部，可以获取当前的 `state` 或者触发另外一个 `actions`。

简单是示例：

```js
...
actions:{
  incrementAction:function(context){
    if(context.state.count>1){
      context.dispatch('actionOther')
    }
  }，
  actionOther:function(){
    console.log('actionOther')
  }
}
```

案例演示：

```html
<div id="app">
  <h1>{{count}}</h1>
  <button @click="clickCallback">点击一下</button>
</div>
```

```js
const store = new Vuex.Store({
  state: {
    count: 3,
  },
  mutations: {
    increment1(state, params) {
      state.count = state.count + params.num
    },
    increment2(state, params) {
      state.count = state.count + params.num + 1
    },
  },
  actions: {
    incrementAction1(context, params) {
      // 在action中返回一个Promise对象的情况
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          context.commit('increment1', params)
          resolve('1')
        }, 1000)
      })
    },
    incrementAction2({ commit, dispatch }, params) {
      return dispatch('incrementAction1', {
        num: 6,
      }).then(() => {
        // 提交increment2
        commit('increment2', params)
        return Promise.resolve('这里是incrementAction2')
      })
    },
  },
})

const vm = new Vue({
  el: '#app',
  store,
  computed: {
    count() {
      return this.$store.state.count //通过this.$store.state可以获取state
    },
  },
  methods: {
    clickCallback() {
      // 通过this。$store.dispatch调用action
      // 而且store.dispatch可以处理被触发的action的处理函数返回的promise,并且store.dispatch 仍旧返回Promise：
      this.$store
        .dispatch('incrementAction2', { num: 30 })
      // 由于dispatch返回的是promise对象，所以可以then
        .then(value => console.log(value))
    },
  },
})
```

结果图示：

![image-20210726004452521](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005220.png)

> **提示：**
>
> **一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。**



## 八、Modules

### 8.1 了解Modules

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 `state`、`mutation`、`action`、`getter`、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```



### 8.2 什么context

`context`即上下文的意思。



### 8.3 模块的局部状态

对于模块内部的 `mutation` 和 `getter`，接收的第一个参数是**模块的局部状态对象**。

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 `action`，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

对于模块内部的 `getter`，根节点状态会作为第三个参数暴露出来：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```



### 8.4 命名空间

#### # 使用namespaced：true成为命名空间模块

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一个 `mutation`、`getters` 或 `action` 作出响应。

但是如果多个`module`有相同名字的`getters`、`mutation`或`action`，就会依次触发，这样可能会出现不是我们想要的结果。

如果希望模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为**`带命名空间的模块`**。当模块被注册后，它的所有 `getter`、`action` 及 `mutation` 都会自动根据模块注册的路径调整命名。例如：

```js
const moduleA = {
  // 通过添加namespaced:true的方式使其成为带命名空间的模块
  namespaced: true,
  state: {
    count: 3,
  },
  mutations: {
    increment: function (state) {
      console.log('moduleA')
      state.count++
    },
  },
  getters: {
    doubleCount: function (state) {
      return state.count * 2
    },
  },
  actions: {
    incrementAction: function ({ commit }) {
      commit('increment')
    },
  },
}

// 通过添加namespaced:true的方式使其成为带命名空间的模块
const moduleB = {
  //
  namespaced: true,
  state: {
    count: 4,
  },
  mutations: {
    increment: function (state) {
      console.log('moduleB')
      state.count++
    },
  },
  getters: {
    doubleCount: function (state) {
      return state.count * 2
    },
  },
  actions: {
    incrementAction: function ({ commit }) {
      commit('increment')
    },
  },
}

const counter = {
  template: `<div>
<h1>{{count}}</h1>
<button @click="clickCallback">增加</button>
</div>`,
  computed: {
    count() {
      // 通过this.$store.state.moduleA可以获取moduleA的state
      return this.$store.state.moduleA.count
    },
  },
  methods: {
    clickCallback: function () {
      // 通过this.$store.dispatch调用 'moduleA/incrementAction'指定的Action
      this.$store.dispatch('moduleA/incrementAction')
    },
  },
}

const store = new Vuex.Store({
  modules: {
    moduleA: moduleA,
    moduleB: moduleB,
  },
})

const vm = new Vue({
  el: '#app',
  store,
  components: {
    counter: counter,
  },
})
```



#### # 命名空间模块属性的调用

如果要调用一个`module`内部的 `actions`时，需要使用如下代码：

```js
this.$store.dispatch('moduleA/incrementAction')
```

![image-20210726201912296](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005233.png)

>- 模块中的 `dispatch方法参数`由==**"空间key+'/'+actions名"**==组成。例如：
>
>  ![image-20210726202354898](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005238.png)
>
>  除了调用指定命名空间的action外。

当然也可以调用指定命名空间的 `mutations`，或者是存取指定命名空间下的 `getters`，代码如下:

```js
this.$store.commit('moduleA/increment')
this.$store.getters['moduleA/doubleCount']
```

示例：

![image-20210726203351864](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005241.png)

![image-20210726203650858](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005246.png)



### 8.5 Module模块之间交互调用

若两个module之间进行交互调用，例如把moduleA的操作 `actions`或`mutations`，通知到moduleB的`actions`或`mutations`中，那么将 `{root:true}`作为第三参数传给`dispatch`或`commit`即可。

代码示例：

```js
.....
const moduleB = {
  namespaced: true,
  state: {
    count: 4,
  },
  mutations: {
    increment: function (state) {
      console.log('moduleB')
    },
  },
  getters: {
    doubleCount: function (state) {
      return state.count * 2
    },
  },
  actions: {
    incrementAction: function ({ commit, dispatch }) {
      // 在moduleB中提交moduleA相关的mutation
      commit('moduleA/increment', null, { root: true })
      // 或者
      dispatch('moduleA/incrementAction', null, { root: true })
    },
  },
}
```

>**说明：**
>
>- ==**第一个参数必须由 "空间key+'/'+actions名（或者mutations名）"组成**==，这样Vuex才能找到对应命名空间下的`actions`或者`mutation`。
>
>- 第二个参数是自定义传递的数据，默认为空。
>
>- 第三个参数是`{root:true}`
>
>  示例：
>
>  ![image-20210726231624395](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005252.png)



### 8.6 rootState和rootGetters参数的使用

举例：如果需要在moduleA内部的`getters`中或者`action`中存取全局的`state`或`getters`，**可以利用 `rootState` 和 `rootGetter` 作为第三个和第四个参数传入getters，同时也会通过 `context`对象的属性传入action。**

示例：

```js
const moduleA = {
  namespaced: true,
  state: {
    count: 3,
  },
  getters: {
    doubleCount: function (state, getters, rootState, rootGetters) {
      console.log(getters) //当前module的getters
      console.log(rootState) //全局的state->rootCount:4
      console.log(rootGetters) //全局的getters->rootDoubleCount:8
      return state.count * 3
    },
  },
  actions: {
    incrementAction: function ({ rootGetters, rootState }) {
      console.log(rootState)
      console.log(rootState.rootCount) //全局的state->rootCount：4
      console.log(rootState.moduleA.count) //模块A中的state->count:3
      console.log(rootGetters) //全局的getters->rootDoubleCount：8
      console.log(rootGetters['moduleA/doubleCount'])
    },
  },
}
const store = new Vuex.Store({
  state: {
    rootCount: 4,
  },
  getters: {
    rootDoubleCount: function (state) {
      return state.rootCount * 2
    },
  },
  modules: {
    moduleA,
  },
})
const vm = new Vue({
  el: '#app',
  store,
  computed: {
    count() {
      return this.$store.state.rootCount
    },
  },
  methods: {
    clickCallback: function () {
      this.$store.dispatch('moduleA/incrementAction')
    },
  },
})
```

其中：`getters部分`

![image-20210726235646024](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005300.png)

`actions部分`

![image-20210726235719441](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005306.png)



### 8.7 带命名空间的模块注册全局action(应用场景较少)

如需要在带命名空间的模块注册全局`actions`（虽然这种应用场景较少遇到），则可添加`root:true`将这个带入到`actions`定义的处理函数中。

示例代码：

```js
...
{
  actions:{
    someOtherAction:function(context){
      context.dispatch('someAction')
    }
  },
  modules:{
    moduleC:{
      namespaced:true,
      actions:{
        someAction:{
          root:true,
          handler:function(namespace,params){...} //  -> 'someAction'
                                            }
        }
      }
    }
  }
```



## 九、Vuex进阶

### 9.1 项目结构

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **`mutation`** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **`action`** 里面。

只要你遵守以上规则，如何组织代码随你便。如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```



### 9.2 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`：

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

在严格模式下，无论何时发生了状态变更且不是由 mutations的函数引起的，将会抛出错误。**这样做是为了保证所有的状态变更都能被调试工具跟踪到。**



### 9.3 开发环境与发布环境

==**不要在发布环境下启用严格模式！！**==严格模式会深度监测状态树来检测不合规的状态变更——请确保在发布环境下关闭严格模式，以避免性能损失。

类似于插件，我们可以让构建工具来处理这种情况：

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```



## 十、简单的Store模式

- `Vue`在大型项目中一般使用`Vuex`作为数据存储。
- 但是在小项目中存储几个共用数据大可不必使用`Vuex`，只需使用**`简单的Store模式`。**

### 10.1 了解简单的Store模式

官方代码示例：

```js
var store = {
  debug: true,
  state: {
    message: 'Hello!'
  },
  setMessageAction (newValue) {
    if (this.debug) console.log('setMessageAction triggered with', newValue)
    this.state.message = newValue
  },
  clearMessageAction () {
    if (this.debug) console.log('clearMessageAction triggered')
    this.state.message = ''
  }
}
```

在`简单的Store模式`中需要注意的有：

> 该`store`中所有`state状态`的变更，都放置在`store`自身的`Actions`中去管理。
>
> ![image-20210727200742115](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005316.png)

优点：

- 这种`集中式状态管理`能更容易地理解`哪种类型的state状态`将会发生变更，以及它们是如何被触发。
- 当错误出现时，我们现在也会有一个 log 记录 bug 之前发生了什么。



每个**实例/组件**仍然可以拥有和管理自己的`状态(也就是数据)`。

```js
var vmA = new Vue({
  data: {
    //自己的组件内的数据
    privateState: {},
    //共享的状态数据
    sharedState: store.state
  }
})

var vmB = new Vue({
  data: {
    //自己的组件内的数据
    privateState: {},
    //共享的状态数据
    sharedState: store.state
  }
})
```



### 10.2 简单的Store模式图

![image-20210727204543422](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005322.png)



### 10.3 简单的Store模式的使用

#### # 案例一

案例一代码：

```html
<div id="app">
  <h1>{{privateState}}</h1>
  <h1>{{sharedState.message}}</h1>
  <button @click="clickBackcall">更改简单store模式中的state</button>
</div>
```

```js
// 用var声明一个全局变量,也就是window顶层的属性
var store = {
  debug: true,
  state: {
    message: 'Hello',
  },
  setMessageAction(newValue) {
    if (this.debug) console.log('这是新的值：' + newValue)
    this.state.message = newValue
  },
  clearMessageAction() {
    if (this.debug) console.log('清除这个值')
    this.state.message = ''
  },
}

const vm = new Vue({
  el: '#app',
  data: {
    // 每个实例/组件仍然可以拥有和管理自己的私有状态数据
    privateState: '这是我的私有状态数据',
    // 共享状态
    // 注意在组件data中不能直接指向store中的state状态对象里的属性，否则不能同步更新！
    sharedState: store.state,
  },
  methods: {
    clickBackcall() {
      // 调用store中更改message的方法
      store.setMessageAction('hello world')
    },
  },
})
```

图示：

![image-20210727205039986](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005329.png)

![image-20210727205114997](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005334.png)



#### # 案例二

利用`http-vue-loader`处理`Vue`文件，从而达到工程中项目的使用。

**HTML部分：**

```html
<div id="my-app">
  <cpn1></cpn1>
</div>
<script src="./Vue资源文件/vue.js"></script>
<script src="./Vue资源文件/httpVueLoader.js"></script>
<script type="module" src="./index.js"></script>
```

**Vue组件文件部分：**

```vue
<template>
<div>
  <h1>
    <div @click="changePersonName">person name: {{ shareState.name }}</div>
  </h1>
  </div>
</template>

<script>
  module.exports = {
    name: 'cpn1',
    data () {
      return {
        shareState: this.$person.state
      }
    },
    methods: {
      changePersonName () {
        const random = Math.floor(Math.random() * 10)
        const name = 'joeal' + random
        this.$person.setName(name)
      }
    }
  }
</script>
```

**入口文件部分：**

由于`http-vue-loader`问题，这里我只能把导入的`person`放在Vue的原型上。

```js
import person from './person.js'
Vue.prototype.$person = person

const vm = new Vue({
  el: '#my-app',
  components: {
    cpn1: httpVueLoader('./cpn1.vue'),
  },
})
```

**简单Store模式的状态数据和Actions：**

```js
// 简单的store模式存放state,使得各组件公用一个数据模块
var person = {
  state: {
    name: '',
    age: 23
  },
  setName (newname) {
    this.state.name = newname
    console.log('set', this.state.name)
  }
}
export default person
```

结果：

![image-20210727210538393](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005343.png)

![image-20210727210621612](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005347.png)



### 10.4 简单的Store模式总结

>- **在简单Store模式中，你不能直接在`Actions`中直接替换`共享状态对象state`。因为组件和Store需要引用同一个`共享状态对象state`,这样`共享状态对象state`变更才能够被观察到。**
>
>- 如果直接在`Actions`中替换了`共享状态对象state`，后果就是不能在Vue开发工具中查看共享状态的变更，这个问题是很致命的。

案例说明：

`共享状态对象state`

![image-20210727212006287](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005352.png)

在 `Actions`中直接替换`共享状态对象`

```js
methods: {
  clickBackcall() {
    // 调用store中更改message的方法
    store.setMessageAction({ message: 'asdas' })
  },
},
```

结果：

![image-20210727212217243](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005357.png)

![image-20210727212331458](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005404.png)

> **最后我们约定，组件不允许直接变更属于 `store 实例`的 `state`，而应执行 `action` 来分发 (dispatch) 事件通知 store 去改变 `state状态数据`。**



## 十一、Vuex适用场合

Vuex 可以帮助我们进行项目状态的管理，在大型项目中，使用Vuex是非常不错的选择。但是，在使用Vuex时，很多的逻辑操作会让我们感觉很”绕“，例如修改一个状态需要 actions->mutations->state,这些步骤不免让人感到烦琐冗余。

**如果应用比较简单，最好不要使用Vuex。一个`简单的 store 模式`就足够用了。**

但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。

