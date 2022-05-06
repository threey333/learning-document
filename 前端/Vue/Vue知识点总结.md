# Vue知识点总结

## 一、Vue实例的一些常用属性

### 1.1 $data属性

通过 `this.$data` 属性可以获取到Vue实例代理的`data对象`属性数据信息。

```js
export default {
  name: 'App',
  props: {
    result: {
      type: String,
      default: 'oh mygad'
    }
  },
  data () {
    return {
      title: 'hello world'
    }
  },
  created () {
    console.log(this.$data) // => {title:'hello world'}
    console.log(this.title) // => hello world
  },
}
```



### 1.2 $props

Vue实例代理了 `Props对象属性` 的数据信息，也就是说，我们可以通过 `this.$options` 属性可以拿到**当前组件的Props对象数据。**

```js
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
  created () {
    console.log(this.$props) // => {result:You look so good}
  },
}
```



### 1.3 $options

通过 `this.$options` 属性可以**拿到Vue实例的初始化选项信息。**

```js
export default {
  name: 'App',
  props: {
    result: {
      type: String,
      default: 'oh mygad'
    }
  },
  data () {
    return {
      title: 'hello world'
    }
  },
  created () {
    console.log(this.$options.data) // =>{...}
    console.log(this.$options.name) // => App
    console.log((this.$options.props) //=>{result:{dafault:'oh mygad'}}
  },
}
```

而根据文档，该属性一般是用在**当组件中包含有自定义property时，可以通过$options属性获取。**

```js
new Vue({
  customOption: 'foo',
  created: function () {
    console.log(this.$options.customOption) // => 'foo'
  }
})
```

同时，通过该属性还可以获取 `render函数` 对模板进行重新编译。

```js
new Vue({
  created: function () {
    this.$options.render = (h) => {
      return h('h1', {}, 'hello world!!!!')
    }
  }
})
```



### 1.4 $root

通过 `this.$root` 可以拿到Vue实例的根节点元素。

==注意：==在任何一个Vue组件中都可以通过 `this.$root` 拿到Vue实例的根节点。

```js
new Vue({
  mounted () {
    console.log(this.$root) //= >拿到的是Vue的根节点元素。
  },
})
```



## 二、Vue实例中的render函数

`render函数` 作用是将指定的模板进行编译。它的第一个参数是一个**创建元素节点的createElement方法。**

```js
new Vue({
  render: h => {
    return h(App)  //=> h指的就是createElement方法，该方法里的第一个参数是指定的元素节点（例如App组件可以认为是一个节点）
  }
}).$mount('#app')
```

触发时机：**在beforeMount函数之后，Mounted函数之前。**



## 三、Vue.extend方法

>API：Vue.extend(options)

该方法的返回值是一个组件构造器。

**主要使用场景：**1、动态渲染组件。2、实现类似于window.alert()这种提示功能。

### 3.1 基础用法

用一个例子说明，方便理解 `Vue.extend()` 方法。

```html

```

