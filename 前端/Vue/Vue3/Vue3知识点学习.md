# Vue3知识点学习

## 一、创建一个Vue实例应用

### 1.1 使用Vue3新API创建一个Vue实例

一、在Vue3中，如果要创建一个Vue应用实例，那么可以采用Vue3新提供的**全局API方法 `createApp()`。**

`createApp()` 方法会返回一个Vue应用实例，因此可以链式继续调用其它的方法，从而实现全局使用。

```js
Vue.createApp({...选项})
```

这里创建一个todoList应用为例子，展示 `createApp()` 方法的使用。

```html
<div id="app">
  添加今天要做的任务
  <input
         class="input"
         type="text"
         v-model="inputVal"
         @keyup.enter="confirm"
         />
  {{inputVal}}
  <ul>
    <li v-for="(item,index) in list">
      <span :key="index">{{item.text}}</span>
    </li>
  </ul>
</div>
```

```js
const todoList = {
  data() {
    return {
      list: [
        {
          text: '今天学习了吗',
          done: false,
        },
      ],
      inputVal: '',
    }
  },
  methods: {
    confirm() {
      const { inputVal, list } = this
      list.push({
        text: inputVal,
        done: false,
      })
      this.inputVal = ''
    },
  },
}
Vue.createApp(todoList).mount('#app')
```

二、该方法的第二个参数可以是自身实例组件的 `prop` 属性，因此可以在根组件处将该 `prop` 传递到应用的其它组件中。

继续以todoList应用为例子：

```js
const todoList = {
  props: {
    username: {
      type: String,
    },
  },
  data() {
    return {
      list: [
        {
          text: '今天学习了吗',
          done: false,
        },
      ],
      inputVal: '',
    }
  },
  methods: {
    confirm() {
      const { inputVal, list } = this
      list.push({
        text: inputVal,
        done: false,
      })
      this.inputVal = ''
    },
  },
}
Vue.createApp(todoList, { username: 'threey' }).mount('#app')
```



### 1.2 将Vue实例挂载在模板页面上

在Vue3中，如果想把创建的 `Vue应用实例` 挂载到模板页面上进行展示，那么只能通过 `mount()` 方法实现。

```js
Vue.createApp({...选项}).mount('#app')
```



### 1.3 Vue3创建实例/挂载跟Vue2时的区别

#### 1.3.1 创建实例的区别

在Vue2时，如果要创建Vue应用实例则是使用 `Vue构造函数` 创建。

```js
new Vue({...选项})
```

Vue3采用 `createApp()` 创建应用实例。

```js
Vue.createApp({...选项})
```

#### 1.3.2 挂载时的区别

在Vue2时，将 `Vue应用实例` 挂载到模板页面上，有两种方式：1.指定 `el选项`。2.通过 `mount()` 方法。

```js
// 1.指定el选项
new Vue({
  el:'...',
  ...选项
})

// 2.通过mount()方法
new Vue({...选项}).mount('...')
```

而Vue3时，只能通过 `mount()` 方法将 `Vue应用实例` 挂载到模板页面上。

```js
Vue.createApp({...选项}).mount('#app')
```



## 二、