# Vue.js

## 一、什么是Vue

引文来自官方：https://cn.vuejs.org/v2/guide/reactivity.html

参考：https://cn.vuejs.org/v2/guide/index.html

### 1.1 Vue2.x的概述

`Vue (读音 /vjuː/，类似于 view) 是一套用于构建用户界面的渐进式框架`。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动

### 1.2 理解Vue的核心理念

MVVM===>M/V/VM

- M：model数据层
- V：view视图层
- VM：ViewModel 视图模型
- 核心：M<=> ==VM== <=>V

![image-20210525211751311](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117002119.png)

MVVM 提出来的思想是通过 `数据双向绑定`让数据自动的双向同步

- V(修改视图)==>M
- M(修改数据)==>V

------

传统的做法：

![image-20210519164723956](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117002148.png)

采用MVVM模式:

![image-20210519170117393](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212603.png)

>好处：数据和视图分离，便于维护和升级

### 1.3 学习Vue要转换思想

- 采用的是`数据驱动视图`的思想，**数据是核心**
- 以后如果想要操作DOM，立马想到的是拿到数据而不是拿到元素对象。
- **数据驱动视图**：不要再想着怎么操作DOM，而是想着如何操作数据。

### 1.4 小结

​		Vue最独特的特性之一，是其非侵入性的响应式系统。`数据模型仅仅是最普通的JavaScript对象`。而当你`修改数据时`，`视图会进行更新`。这使得状态管理非常简单直接。==好处就是：数据和视图分离，便于维护和升级。==

​		

## 二、Vue2.x的安装

### 2.1 Vue Devtools安装

在使用 Vue 时，我们推荐在你的浏览器上安装 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)。它允许你在一个更友好的界面中审查和调试 Vue 应用。

### 2.2 直接用<script>引入

- 直接下载Vue文件并用`<script>`标签引入，`Vue`会被注册为一个全局变量。

![image-20210519180327358](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212609.png)

- 使用`CDN`方式引入：这样可以使用最新版本

  ```html
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  ```

### 2.3 NPM安装(推荐)

在用 Vue ==构建大型应用时推荐使用 NPM 安装==。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)。

```shell
# 最新稳定版
$ npm install vue
```



## 三、Vue实例

### 3.1 创建一个Vue实例

步骤：

1. 引入`Vue文件`
2. 创建一个`Vue的实例对象VM`
3. 定义`Vue实例`从什么节点中进行**挂载**和**渲染**。

```vue
<div id="app">
    <h1>{{title}}</h1>
    <p>{{name}}</p>
</div>


<!-- 1.引入Vue文件 -->
<script src="../js/vue.js"></script>
<!-- 2.创建一个Vue的实例 -->
<script>
    let vm = new Vue({
        // 指定Vue管理的边界
        el: '#app',
        // data:数据可以直接在Vue的管理边界内直接使用
        data: {
            title: "threey第一次学习Vue",
            name: 'threey'
        }
    })
</script>
<!-- 3.定义Vue实例从什么节点中进行渲染和挂载 -->
```



## 四、插值表达式

官网参考：https://cn.vuejs.org/v2/guide/syntax.html

Vue模板中插值常见的方法主要有：`文本`、`原始HTML`、`属性`、`JavaScript表达式`、`指令`和`修饰符`

### 4.1 Mustache语法

数据绑定最常见的形式就是使用==“Mustache”语法 (双大括号)==的文本插值：

```html
<h2>{{title}}</h2>
<p>我是{{name}},今年{{age}},家住在{{adress}},学号是:{{id}}</p>
```

### 4.2 v-once指令

v-once指令可以使插值处的内容不会发生改变。

```html
<h1>{{title}}</h1>
<h2>{{msg}}</h2>
<!-- v-once指令可以使插值处的内容不会发生改变  -->
<h2 v-once>{{msg}}</h2>
```

结果图：

![image-20210524160316388](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212618.png)

![image-20210524160522202](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212621.png)

### 4.3 v-html指令

一对大括号会将数据解析为普通文本，而不是HTML代码。为了能输出真正的HTML代码，需要使用`v-html`指令。

```vue
<div id="box">
  <h3>{{url}}</h3>
  <h3 v-html="url"></h3>
  //有无v-html指令的情况
</div>
```

结果：

![image-20210524161020883](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212626.png)

### 4.4 v-pre指令的使用

v-pre指令可以显示原来的双大括号的语法

```vue
<div id="box">
  <h2>{{title}}</h2>
  <h3>{{message}}</h3>
  <!-- 使用v-pre指令 -->
  <!-- v-pre指令可以显示原来的双大括号的语法 -->
  <h3 v-pre>{{message}}</h3>
</div>
```



### 4.5 支持JavaScript表达式：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会生效**。

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流程控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```



## 五、动态绑定属性

### v-bind指令

`v-bind`绑定的属性如果是多单词的，不能使用驼峰命名法，而是**使用'-'连接之间的单词**。

#### #v-bind基本使用

双大括号语法不能用在HTML的属性上，只能使用`v-bind指令`。

`v-bind`用于绑定一个或多个属性值，或者向另一个组件传递`props值`，这个我们学到组件的时候在介绍

```vue
<div class="box" id="box">
  <img :src="urlLink" :alt="urlAlt" />
  <a :href="src.link">{{src.name}}</a>
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      urlLink:
      "https://hbimg.huabanimg.com/d98ed32871cbdbb296500988443ebce53cfa10191d0b5-fALB66_fw658/format/webp",
      urlAlt: "Rose",
      src: {
        link: "http://www.baidu.com",
        name: "百度一下",
      },
    },
  });
</script>
```

   v-bind绑定HTML属性后，vue实例中的属性在这里会变成‘变量’的方式。

#### #v-bind动态绑定class(对象语法)

> v-bind绑定的HTML元素属性的属性值不仅可以是变量还可以是数组、对象等等。

其中==:class里的值是一个对象是最常用的==，也就是对象语法的形式最常用

```vue
<div id="box">
  <!-- v-bind绑定的HTML元素属性的属性值不仅可以是变量还可以是数组、对象等等
其中:class里的值是一个对象是最常用的，也就是对象语法的形式最常用 -->
  <h2 :class="{active:isboolean}">{{message}}</h2>

  <!-- 动态绑定的class能和静态的class合并共存 -->
  <h2 class="w" :class="{active:isboolean}">{{message}}</h2>
  <button @click="change">变颜色</button>
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      message: "今天星期五",
      age: 22,
      name: "threey",
      id: 2181600,
      active: "active",
      isboolean: false,
    },
  });
</script>
```

> 如果v-bind绑定的class语句过长，可以把它放在`methods`或者`computed`中

```vue
<div id="box">
  <h2 :class="getClass()">{{message}}</h2>
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      message: "今天星期五",
      age: 22,
      active: "active",
      isboolean: false,
    },
    methods: {
      getClass: function () {
        return { active: this.isboolean };
      },
    },
  });
</script>
```

> 动态绑定的class能和静态的class合并共存

```vue
<h2 class="w" :class="{active:isboolean}">{{message}}</h2>
```

结果图：

![image-20210524162601955](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212633.png)



#### #v-bind动态绑定class(数组语法)

v-bind动态绑定class(数组语法)，这个方法了解一下就可以，因为用的少。

```vue
<style>
  .active {
    color: #fba;
  }
</style>
</head>
<body>
  <div id="box">
    <h1>{{title}}</h1>
    <!-- 数组语法的形式很少用 -->
    <h2 v-bind:class="[active,w]">{{msg}}</h2>
  </div>
  <script src="../js/vue.js"></script>
  <script>
    const vm = new Vue({
      el: "#box",
      data: {
        title: "v-bind动态绑定class(数组语法--=了解)",
        msg: "threey333",
        active: "active",
        isboolean: true,
        w: "w",
      },
    });
  </script>
```

#### #v-bind指令也支持JavaScript表达式

```vue
<p :id="'name-'+num4">{{name+num4}}</p>
```

#### #v-bind动态绑定style(对象语法)

动态绑定style时，key为css属性名，value为css属性值

```vue
<h2 :style="{fontSize:finalSize+'px',color:'red'}">{{name}}</h2>
```

当绑定style语句过长时，可以把语句放在方法中当返回值返回

```javascript
<h2 :style="getStyle()">{{name}}</h2>
```

#### #v-bind动态绑定style(数组语法)

v-bind动态绑定style(数组语法)---了解用的少,基本如下这种形式：

```vue
<h2 :style="[result1,result2]">{{name+id}}</h2>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      name: "threey",
      id: 2181600,
      result1: { fontSize: "50px" },
      result2: { color: "red" },
    },
  });
</script>
```



## 六、计算属性和监听器

### 6.1 计算属性的基本使用

计算属性关键词: `computed`。

**计算属性在处理一些复杂逻辑时是很有用的。**

```vue
<div id="box">
  <h2>{{totalPrice}}</h2>
  <!-- 结果53，正确 -->
  <h2>{{reverseMessage}}</h2>
  <!-- 结果：gnayoaix -->
  <!-- 计算属性里的方法本质也把它当作属性看，所以不加() -->
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      price1: 25,
      price2: 28,
      message: "xiaoyang",
    },
    // computed是计算属性，里面的方法用简介表示法(ES6)
    computed: {
      // 计算属性的getter
      // 计算属性中的方法名字不要起动名词。
      totalPrice() {
        return this.price1 + this.price2;
      },
      reverseMessage() {
        return this.message.split("").reverse().join("");
      },
    },
  });
</script>
```



### 6.2 计算属性的setter和getter

计算属性默认只有getter，不过在你需要的时候也可以提供一个setter

```vue
<div id="box">
  <h2>{{totalPrice}}</h2>
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      num1: 10,
      num2: 20,
    },
    computed: {
      // 原本是这样的，计算属性里面有属性，里面有getter和setter
      fullName: {
        // 赋值器
        set: function (newValue) {
          const names = newValue.split(" ");
          console.log(names);
          this.name1 = names[0];
          this.name2 = names[1];
        },
        //取值器
        get: function () {
          return this.name1 + " " + this.name2;
        },
      },
      // 后面为了书写简单，直接简化了写法，所以就有如下的这种形式
      totalPrice: function () {
        return this.num1 + this.num2;
      },
    },
  });
</script>
```

### 6.3 计算属性和methods的对比

- `计算属性(computed)`：只要**依赖的数据没发生改变**，就可以直接返回缓存中的数据，而不需要每次都重复执行数据操作
- `方法(methods)`：只要**页面更新用户界面(即data数据发送变化)，就会发生重新渲染**，`methods`调用对应的方法，执行该函数，而不管是不是它所依赖的。

验证示例：

```vue
<div id="box">
  <!--2. 通过定义methods   -->
  <h2>通过定义methods方法：{{getFullName()}}</h2>
  <h2>通过定义methods方法：{{getFullName()}}</h2>
  <h2>通过定义methods方法：{{getFullName()}}</h2>
  <h2>通过定义methods方法：{{getFullName()}}</h2>
  <h2>通过定义methods方法：{{getFullName()}}</h2>

  <!--3. 通过computed -->
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
</div>
<script src="../js/vue.js"></script>
<script>
  const vm = new Vue({
    el: "#box",
    data: {
      name1: "threey",
      name2: "Ethon",
    },
    // computed是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值,也就是说当依赖的值发生改变，它才会执行。
    computed: {
      fullName: function () {
        console.log("fullName");
        return this.name1 + " " + this.name2;
      },
    },
    // methods方法没有缓存，每当触发重新渲染时，调用方法总会再次执行方法
    methods: {
      getFullName() {
        console.log("fullName");
        return this.name1 + " " + this.name2;
      },
    },
  });
</script>
```

图片验证：

![image-20210524164848904](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212641.png)

>computed和methods从中对比的思考

​		我们为什么需要缓存？假设我们有一个性能开销比较大的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A。如果没有缓存，我们将不可避免的多次执行 A 的 getter！如果你不希望有缓存，请用方法来替代。



### 6.4 计算属性的特点

- 计算属性一定要有返回值，返回值才是计算属性最终的结果。
- 计算属性只会随着**`相关的数据`**发生变化而变化，相关的数据一发生变化，计算属性就会重新计算。



### 6.5 watch监听器

当需要在==**数据变化时执行异步**==或==**开销较大的操作**==时，这个方式是最有用的。

#### # watch监听器的基本使用

`watch`：Vue的监听器

用法：

```js
watch:{
  【要监听data中的数据】:function(new,old){
    .....
  }
}
其中，new代表新值，old代表旧值.
```

当然也可以使用ES6对象属性的简洁表示法：

```js
【要监听data中的数据】(){
  .....
}
```

还可以把==监听的数据写成对象的形式==，其中包含`handler方法`和`immediate属性`。

```js
watch:{
  【要监听data中的数据】：{
    handler(newVal,oldVal){
      .....
    },
      immediate:true
  }
}
说明：immediate为true则立即执行handler方法（不管数据有没有发生变化）,为false则在数据发生变化后才执行handler方法
```

案例示例：

```html
<div id="app">
  <h1>{{num2}}</h1>
  <button @click="num2++">+</button>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    num1: 10,
    num2: 20,
  },
  watch: {
    num2: function (newnum1, oldnum2) {
      console.log(newnum1, oldnum2);
    },
  },
});
```

结果：

![image-20210605180614500](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212646.png)



#### #监听器的小案例

监听器小案例，加深对监听器的用法

> 需求 : 监听文本框的字符个数 (3-6之间), 如果超过这个范围,给一个格式不正确的提示

```html
<div id="app">
  <h1>{{title}}</h1>
  <h2>
    <input type="text" v-model="contents" />
    <span v-show="isShow" style="color: red">格式不正确</span>
  </h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "watch监听器的小案例",
    contents: "",
    isShow: false,
  },
  watch: {
    contents(newVal) {
      if (newVal.length >= 3 && newVal.length <= 6) {
        this.isShow = false;
      } else {
        this.isShow = true;
      }
    },
  },
});
```

结果：

![image-20210605180932737](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212650.png)

当输入的字符个数超过6个或者小于3个的时候，提示框就会出来

![image-20210605181031047](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212652.png)



#### #watch的深度监听

当watch需要监听一个==复杂对象的改变时==，普通的watch方法无法监听到对象内部属性

比如：

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "watch的深度监听",
    list: {
      name: "threey",
    },
  },
  watch: {
    list(newVal, oldVal) {
      console.log(newVal);
    },
  },
});
```

此时`watch监听器`是监听不到`list`对象内的数据变化，因为==此时的watch监听的是list对象的内存地址==，但是`list对象`在这个案例中地址是不变的,所以是监听不到`list对象`内的数据变化。



**想要深入监听list对象内的数据变化，有两种方法：**

1. **使用`deep`属性进行深度监听**
2. **使用`字符串的形式`监听对象属性，此时无需设置`deep:true`**

方法一：

```js
watch: {
  list: {
    //把监听的数据写成对象的形式
    handler: function (newVal, oldVal) {
      console.log(newVal);
    },
      deep: true,
  },
}
```

方法二：

```js
watch: {
  "list.name"(newVal) {
    console.log(newVal);
  },
    //当修改list.name的值，在控制台能看到监听方法触发了。
},
```



#### #watch和computed的区别

对于`计算属性(computed)`：

- 计算属性一定有返回值
- 计算属性是根据已知值得到新值，并且新值还会随着已知值的变化而变化
- 特点：==**依赖的已知值改变会影响到计算属性新值的改变**==

示例：

```html
<div id="app">
  <h1>{{fullname}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    name1: "threey",
    name2: "ethon",
  },
  computed: {
    fullname() {
      console.log("data的值发生了改变");
      return this.name1 + "想成为" + this.name2;
    },
  },
});
```

图片显示：

![image-20210605183551628](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212657.png)

修改后的结果：

![image-20210605183735271](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212659.png)

------

对于`watch监听器`:

- 会监听数据的变化
- 特点：==**监听的数据会影响data中的其他数据**==

示例：

```html
<div id="app">
  <p v-show="isshow">监听的值发生了改变</p>
  {{num2}}<button @click="num2++">+</button>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    isshow: true,
    num1: 100,
    num2: 200,
  },
  watch: {
    num2(newVal) {
      if (newVal > 202) {
        this.isshow = false;
      } else {
        this.isshow = true;
      }
    },
  },
});
```

结果演示：当新值大于202时会因此“监听的值发生了改变”这几个字

![image-20210605184428546](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212701.png)

![image-20210605184520670](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212704.png)



## 七、事件处理

### 7.1 事件监听

可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

示例：

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

结果：

![image-20210524213531576](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212706.png)

### 7.2 事件处理方法

`v-on` 还可以接收一个==需要调用的方法名称。==

示例：

```html
<div id="box">
  <h1>v-on的基本使用</h1>
  <h1>{{count}}</h1>
  <span><button @click="add()">+</button></span>
  <span><button @click="sub()">-</button></span><br />
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    count: 1,
  },
  methods: {
    add() {
      this.count++;
    },
    sub() {
      this.count--;
    },
  },
});
```

结果：点击+号按钮可以实现cout的值增加1，点击-号按钮可以是count的值减少1。

### 7.3 事件监听的参数问题

>1.如果methods中的js方法带有参数，但事件调用方法你没有带参数，结果是`undefined`

示例：

```html
<div id="box">
  <button @click="btn1Click()">按钮2</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    title: "v-on的参数问题",
  },
  methods: {
    btn1Click(name) {
      console.log(name);
    },
  });
```

结果：

![image-20210524215519118](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212710.png)

==跟js的函数调用情况一样，记得住原生的情况就知道什么原因。==



>2.对于需要一个参数的方法，如果省略小括号得到是`event事件对象`

**原因是Vue会默认将浏览器生产的event事件对象作为参数传入到方法中**

示例：

```html
<div id="box">
  <button @click="btn1Click">按钮4</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    title: "v-on的参数问题",
  },
  methods: {
    btn1Click(name) {
      console.log(name);
    },
  },
});
```

结果是：

![image-20210524220104111](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212712.png)

==注意：如果方法没有形参，那么事件调用方法省略小括号是不会得到事件对象的==



>3.如果methods中的方法带有多个形参，此时你还想得到`event事件对象`的情况

存在多个形参，只需把其中一个作为`$event`实参传入就可以得到`event事件对象`

示例：

```html
<div id="box">
  <button @click="btn1Click("threey",20,$event)">按钮</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    title: "v-on的参数问题",
  },
  methods: {
    btn1Click(name,a,event) {
      console.log(name,a,event);
    },
  },
});
//结果是：threey 20 MouseEvent {isTrusted: true, screenX: 1692, screenY: 218, clientX: 417, clientY: 101, …}
```

结果：

![image-20210524220836434](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212715.png)

### 7.4 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。

尽管我们可以在方法中轻松实现这点，但更好的方式是：**方法只有纯粹的数据逻辑**，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是`由点开头的指令后缀`来表示的。

- `.stop`：阻止冒泡
- `.prevent`：阻止默认行为
- `.capture`：捕获
- `.self`：只点击才触发
- `.once`：只触发一次
- `.passive`：过

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

> 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

部分示例：

`.stop`阻止事件冒泡：

```html
<div id="box">
  <div @click="divClick()">
    aaaaaa
    <button @click.stop="btnClick()">点击</button>
  </div>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {},
  methods: {
    divClick() {
      console.log("divClick");
    },
    btnClick() {
      console.log("btnClick");
    },
  },
});

//结果只显示btnClick,说明阻止了事件冒泡
```

`.once`点击事件将只会触发一次：

```html
<div id="box">
  <button @click.once="btn1Click()">点我一下</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {},
  methods: {
    btn1Click() {
      console.log("点我一下");
    },
  },
});
```

`prevent修饰符的使用`：

```html
<form action="baidu">
  <input type="submit" value="提交" @click.prevent="preventClick()" />
</form>
```

```js
const vm = new Vue({
  el: "#box",
  data: {},
  methods: {
    preventClick() {
      console.log("submit");
    },
  },
});
```



### 7.5 按键修饰符

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

Vue 提供了绝大多数常用的按键码的别名：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

示例：监听某个键盘的键帽:

```html
<input type="text" @keyup.enter="keyup()" />
```

```js
const vm = new Vue({
  el: "#box",
  data: {},
  methods: {
    keyup() {
      console.log("Enther");
    },
  },
});
```



你还可以通过全局`config.keyCodes`对象自定义按键修饰符别名：

```js
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```



## 八、条件渲染

### 8.1 v-if的使用

`v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回`true`值的时候被渲染。

```html
<h2 v-if="isshow">
  <div>abc</div>
  <div>abc</div>
  <div>abc</div>
  <div>abc</div>
</h2>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    isshow: true,
  },
});
```

也可以和`v-else`搭配使用：

```html
<div id="box">
  <h1 v-if="isshow">我可以看见你</h1>
  <h1 v-else>我看不见你</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    isshow: true,
  },
});
```



#### 1. 在 `<template>` 元素上使用 v-if 条件渲染分组

因为 `v-if` 是一个指令，所以必须将它添加到一个元素上。

但是如果想切换多个元素呢？此时可以把一个 `<template>` 元素当做不可见的包裹元素，并在上面使用 `v-if`。

最终的渲染结果将不包含 `<template>` 元素。

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    ok: true,
  },
});
```

结果：只显示`<template>元素内的内容，不显示本标签`

![image-20210526155025930](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212723.png)



#### 2. v-else-if

`v-else-if`，顾名思义，充当 `v-if` 的“else-if 块”，可以连续使用：

```html
<div id="box">
  <div v-if="type === 'A'">A</div>
  <div v-else-if="type === 'B'">B</div>
  <div v-else-if="type === 'C'">C</div>
  <div v-else>Not A/B/C</div>
  <h1>{{result}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    type: "A",
  },
```

类似于 `v-else`，`v-else-if` 也必须紧跟在带 `v-if` 或者 `v-else-if` 的元素之后。

==注意：当逻辑条件复杂的时候，上面的写法是不推荐的（对阅读不友好），这时应该把逻辑判断的部分放在计算属性里面==

示例：

```html
<h1>{{result}}</h1>
修改：<input type="text" @keyup.enter="change" />
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    type: "A",
  },
  computed: {
    result() {
      let showMessage = "";
      if (this.type === "A") {
        showMessage = "A";
      } else if (this.type === "B") {
        showMessage = "B";
      } else if (this.type === "C") {
        showMessage = "C";
      } else {
        showMessage = "Not A/B/C";
      }
      return showMessage;
    },
  },
  methods: {
    change(event) {
      this.type = event.target.value;
    },
  },
});
```

结果：

![image-20210526155755609](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212727.png)



#### 3. 用`key`管理可复用的元素

Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

但是这样也不总是符合实际需求，所以 Vue 为你提供了一种方式来表达==“这两个元素是完全独立的，不要复用它们”==。只需添加一个具有唯一值的`key标签属性`即可：

示例：

```html
<div id="box">
  <template v-if="type===true">
    <label for="user">Username</label>
    <input
           type="text"
           id="user"
           placeholder="请输入您的用户名"
           key="data-username-input"
           />
  </template>
  <template v-else>
    <label for="email">Email</label>
    <input
           type="text"
           id="email"
           placeholder="请输入您的邮箱"
           key="data-email-input"
           />
  </template>
  <button @click="change()">切换登录界面</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    type: true,
  },
  methods: {
    change() {
      this.type = !this.type;
    },
  },
});
```

结果：

![image-20210526160419048](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212731.png)

切换后：

![image-20210526160436138](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212733.png)



### 8.2 v-show

根据条件展示元素的选项是 `v-show 指令`。用法大致和`v-if指令`一样：

```html
<h1 v-show="isshow">{{title}}1</h1>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    title: "Hello Vue",
    isshow: true,
  },
});
```

不同的是带有 `v-show` 的元素==始终会被渲染并保留在 DOM 中==。`v-show` 只是简单地切换元素的 CSS属性的display，即(display:none)。



> 注意，v-show 不支持 <template>元素，也不支持 和v-else指令搭配使用

如果用`v-else`和`v-show`搭配使用，会报错

```html
<template v-show="isshow">
  <h1>{{title}}2</h1>
</template>
<template v-else>
  <h1>{{title}}</h1>
</template> 
//报错
```

如果用<template>元素和`v-show`搭配使用，当布尔值为false时，它是不会隐藏的。



### 8.3 v-if和v-show的区别

-  `v-if`：当条件为false时，包含v-if指令的元素，根本就不会存在dom中
- `v-show`：当条件为false时，v-show只是给我们的元素添加一个行内样式:display:none

![image-20210526161706838](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212740.png)



>那什么时候使用v-if，什么时候使用v-show？

一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。

因此：

- **如果需要非常频繁地切换，则使用 v-show 较好；**
- 如果在运行时条件很少改变，则使用 v-if 较好。



## 九、循环遍历

### 9.1 v-for遍历数组

v-for 指令需要以 **item of arrays** 形式的特殊语法。

```html
<div id="box">
  <h1 v-for="item of items" :key="item.message">{{item.message}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    items: [{ message: "Threey" }, { message: "Foo" }],
  },
});
```

结果：

![image-20210526162653792](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212747.png)

v-for还支持一个可选的第二个参数，即当前项的索引：

```html
<h1 v-for="(item,index) of items">{{index}}：{{item.message}}</h1>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    items: [{ message: "Threey" }, { message: "Foo" }],
  },
});
```

结果：

![image-20210526162946416](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212752.png)



### 9.2 v-for遍历对象

 `v-for` 也可以用来遍历一个对象的属性。

```html
<div id="box">
  <h1 v-for="item of object">{{item}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    object: {
      title: "v-for遍历对象",
      name: "threey",
      id: 2181600,
    },
  },
});
```

结果：

![image-20210526170049497](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212756.png)

**v-for遍历对象也可以使用第二个参数，也就是键名**：

```html
<div id="box">
  <h1 v-for="(item,name) of object">{{name}}：{{item}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    object: {
      title: "v-for遍历对象",
      name: "threey",
      id: 2181600,
    },
  },
});
```

结果：

![image-20210526170249553](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212759.png)

**还可以使用第三个参数作为索引：**

```html
<div id="box">
  <h1 v-for="(item,name,index) of object">{{index}}、{{name}}：{{item}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    object: {
      title: "v-for遍历对象",
      name: "threey",
      id: 2181600,
    },
  },
});
```

结果：

![image-20210526170349606](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212802.png)



### 9.3 v-for使用过程添加key

在使用`v-for`时尽可能的提供`key标签属性`，除非遍历输出的内容非常简单，因为key标签属性是Vue识别节点的一个通用机制。

```html
<div id="box">
  <h1 v-for="item of letter" :key="item">{{item}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    letter: ["a", "b", "c", "d", "e"],
  },
});
```

> 注意：不要使用**对象**或**数组之类**的非基本类型值作为 `v-for` 的 key。请用**字符串**或**数值类型**的值



### 9.4 数组的更新检测

数组的方法是响应式的有：

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

示例：

```html
<div id="box">
  <h1 v-for="items of arrayItems">
    {{items.name}}的价格是:{{items.price}}
  </h1>
  <button @click="tailAdd()">点击尾部增加数组</button>
  <button @click="tailSub()">点击尾部删除数组</button>
  <button @click="headAdd()">点击头部增加数组</button>
  <button @click="headSub()">点击头部删除数组</button>
  <button @click="reverse()">翻转数组</button>
  <button @click="splitArray()">split方法插入数组</button>
</div>
```

```js
const vm = new Vue({
  el: "#box",
  data: {
    arrayItems: [
      { id: 01, name: "高等数学", price: 100 },
      { id: 02, name: "大学物理", price: 88 },
      { id: 03, name: "数据结构", price: 90 },
    ],
  },
  methods: {
    //尾部增加数组方法
    tailAdd() {
      this.arrayItems.push({ id: 04, name: "网页设计", price: 79 });
    },
    //尾部删除数组方法
    tailSub() {
      this.arrayItems.pop();
    },
    //头部增加数组方法
    headAdd() {
      this.arrayItems.unshift({
        id: 05,
        name: "计算机组成原理",
        price: 58,
      });
    },
    //头部删除数组方法
    headSub() {
      this.arrayItems.shift();
    },
    //翻转数组方法
    reverse() {
      this.arrayItems.reverse();
    },
    //splice插入数组方法
    splitArray() {
      this.arrayItems.splice(1, 0, {
        id: 06,
        name: "英语四级",
        price: 88,
      });
    },
  },
});
```

==注意事项：由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化。==



### 9.4 显示过滤或排序后的结果

​		有时，我们想要显示一个数组经过 **过滤** 或 **排序** 后的数据，而不实际变更或重置原始数据。在这种情况下，可以创建一个`计算属性`，来返回过滤或排序后的数组。

例如：

```html
<div id="app">
  <h1 v-for="n of evenNumbers">{{n}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    numbers: [1, 2, 3, 4, 5, 6],
  },
  computed: {
    // evenNumbers：偶数
    evenNumbers() {
      return this.numbers.filter(function (number) {
        return number % 2 === 0;
      });
    },
  },
});
```

结果：

![image-20210526173834154](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212807.png)

在计算属性不适用的情况下 (例如，在嵌套 `v-for` 循环中) 你可以使用一个方法：

```html
<div id="app">
  <ul v-for="data of datas">
    <li v-for="item of even(data)">{{item}}</li>
  </ul>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    datas: [
      [1, 2, 3, 4, 5],
      [6, 7, 8, 9, 10],
    ],
  },
  methods: {
    even: function (numbers) {
      return numbers.filter(function (number) {
        return number % 2 === 0;
      });
    },
  },
});
```

结果：

![image-20210526180315604](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212812.png)



### 9.5 在v-for里使用值范围

`v-for` 也可以接受**整数**。在这种情况下，它会把模板重复对应次数。

```html
<div id="app">
  <h1 v-for="n of 8">{{n}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
});
```

结果：

![image-20210526180858980](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212815.png)



### 9.6 在`<template>`上使用v-for

类似于 `v-if`，你也可以利用带有 `v-for` 的 `<template>` 来循环渲染一段包含多个元素的内容。

比如：

```html
<div id="app">
  <ul>
    <template v-for="item of message">
      <li>{{item}}</li>
      <li class="divider"></li>
    </template>
  </ul>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    message: [1, 2, 3, 4, 5, 6],
  },
});
```

结果：

![image-20210526182401582](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212819.png)



### 9.7 v-for与v-if一同使用

如果你的目的是有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 (或 [`) 上。

如：

```html
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```



### 9.8 在组件上使用v-for

在自定义组件上，你可以像在任何普通元素上一样使用 `v-for`。

```html
<cpn v-for="item in items" :key="item.id"></cpn>
```

**当在组件上使用 `v-for` 时，`key` 现在是必须的**



然而，任何数据都不会被自动传递到组件里，因为==组件有自己独立的作用域==。

示例：

```html
<div id="app">
  <!-- prevent事件修饰符可以防止提交事件不再加载页面 -->
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">添加今天要做的任务 </label>
    <input v-model="newTodoText" id="new-todo" placeholder="任务清单" />
    <button>添加</button>
  </form>

  <ul>
    <!-- is="todo-item"跟<todo-item>作用一样。 -->
    <li
        is="todo-item"
        v-for="(item, index) in todos"
        v-bind:key="item.id"
        v-bind:title="item.title"
        v-on:remove="todos.splice(index, 1)"
        ></li>
  </ul>
</div>
```

```js
Vue.component("todo-item", {
  template: ` 
<li>
{{ title }}
<button v-on:click="$emit('remove')">Remove</button>
</li>
`,
  props: ["title"], //props：父组件向子组件传递数据
});

new Vue({
  el: "#app",
  data: {
    newTodoText: "",
    todos: [
      {
        id: 1,
        title: "做菜",
      },
      {
        id: 2,
        title: "打游戏",
      },
      {
        id: 3,
        title: "看电影",
      },
    ],
    nextTodoId: 4,
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText,
      });
      this.newTodoText = "";
    },
  },
});
```

结果：

![image-20210531165416928](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212823.png)



## 十、表单输入绑定

### 10.1 基础用法

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上实现双向数据绑定。

`v-model`会忽略所有表单元素的`value`、`checked`、`selected`等标签属性的初始值，而总是将 Vue 实例的数据作为数据来源。

>`v-model` 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

- text 和 textarea 元素使用 `value` 属性 和 `input` 事件；
- checkbox 和 radio 使用 `checked` 属性 和 `change` 事件；
- select 字段将 `value` 作为 prop 并将 `change` 作为事件

==上面三项就是各表单元素的双向数据绑定基本原理==



**v-model 本质上不过是语法糖**,它的背后本质上是包含两个操作（以表单元素的text属性举例）：

1. v-bind绑定一个value属性
2. v-on指令给当前元素绑定input事件

![image-20210526183524020](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212827.png)



#### #文本text

这里`v-model`会忽略`value`标签属性的初始值

```html
<div id="app">
  <input type="text" v-model="message" />
  <h1>{{message}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    message: "v-model的基本使用",
  },
});
```



#### #多行文本textarea

这里`v-model`会忽略`value`标签属性的初始值

```html
<div id="app">
  <textarea name="textarea" id="" v-model="message1"></textarea>
  <h1>{{message1}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    message1: "我的邮箱:229498183@qq.com",
  },
});
```



#### #单选按钮radio

这里`v-model`会忽略`checked`标签属性的初始值

```html
<div id="app">
  <input type="radio" id="one" value="One" v-model="picked" />
  <label for="one">One</label>
  <br />
  <input type="radio" id="two" value="Two" v-model="picked" />
  <label for="two">Two</label>
  <br />
  <h2>Picked：{{picked}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    picked: "One",
  },
});
```



#### #复选框checkbox

单个复选框，绑定到布尔值：

这里`v-model`会忽略`checked`标签属性的初始值

```html
<div id="app">
  <input type="checkbox" id="checkbox" v-model="checked" />
  <label for="checkbox">{{checked}}</label>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    checked: true,
  },
});
```

多个复选框，绑定到同一个数组：

这里`v-model`会忽略`checked`标签属性的初始值

```html
<div id="app">
  <input type="checkbox" id="Ethon" value="Ethon" v-model="checkedNames" />
  <label for="Ethon">Ehton</label>
  <input type="checkbox" id="Rose" value="Rose" v-model="checkedNames" />
  <label for="Rose">Rose</label>
  <input type="checkbox" id="Miya" value="Miya" v-model="checkedNames" />
  <label for="Miya">Miya</label>
  <h2>checkedName：{{checkedNames}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    checkedNames: [],
  },
});
```

结果：

![image-20210526204055427](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212831.png)



#### #选择框select

单选时:

选择哪个把该选项的`value标签属性`值赋值到`v-model`绑定Vue实例中的一个属性上。

```html
<div id="app">
  <select id="select" v-model="selected">
    <option disabled value="">请选择</option>
    <option value="A">A</option>
    <option value="B">B</option>
    <option vlaue="C">C</option>
  </select>
  <h2>selected：{{selected}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    selected: "",
  },
});
```

![image-20210526204149127](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212833.png)



多选时 (绑定到一个数组)：

选择哪个把该选项的`value标签属性`值赋值到`v-model`绑定Vue实例中的一个属性上。

```html
<div id="app">      
  <select id="selected" multiple style="width: 50px" v-model="selected1">
    <option value="A">A</option>
    <option value="B">B</option>
    <option value="C">C</option>
  </select>
  <h2>selected：{{selected1}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    selected1: [],
  },
});
```

结果：

![image-20210526204215237](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212835.png)



用 `v-for` 渲染的动态选项：

选择哪个把该选项的`value标签属性`值赋值到`v-model`绑定Vue实例中的一个属性上。

```html
<div id="app">      
  <select v-model="selected3">
    <option v-for="item of selected2" v-bind:value="item.value">
      {{item.text}}
    </option>
  </select>
  <h2>Selected：{{selected3}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    selected3: "a",
    selected2: [
      { text: "One", value: "a" },
      { text: "Two", value: "b" },
      { text: "Threey", value: "c" },
    ],
  },
});
```

结果：

![image-20210526204733287](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212838.png)



### 10.2 值绑定

对于`单选按钮(radio)`，`复选框(checked)`及`选择框(select)的选项，`v-model绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

```html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

但是有时我们可能想把值==**绑定到 Vue 实例的一个动态属性**==上，这时可以用 `v-bind` 实现，并且这个属性的值可以不是字符串。



#### #复选框

```html
<div id="app">
  <input
         type="checkbox"
         v-model="boolean"
         true-value="yes"
         false-value="no"
         id="one"
         />
  <label for="one">One</label>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    boolean: "",
  },
});
```

结果：

![image-20210526213151248](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212842.png)

选择后：

![image-20210526213226379](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212844.png)

>注意:

​		这里的 `true-value` 和 `false-value`标签属性并不会影响输入控件的 `value`标签属性，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(即“yes”或“no”)，请换用单选按钮。



#### #单选按钮

```html
<div id="app">
  <input type="radio" v-model="pick" id="one" v-bind:value="a" />
  <label for="one">one</label>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    pick: "",
    a: "选择后value标签属性会被赋值到v-model绑定的Vue实例的一个属性上",
  },
});
```

选择后的结果：**vm.pick===vm.a**，说明选择哪个把该选项的`value标签属性`值赋值到`v-model`绑定Vue实例中的一个属性上。

![image-20210526214741375](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212847.png)

![image-20210526214810063](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212921.png)



#### #选择框的选项

```html
<div id="app">
  <select v-model="selected">
    <!-- 内联对象字面量 -->
    <option v-bind:value="{ number: 123 }">123</option>
  </select>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    selected: "",
  },
```

选择前：

![image-20210526215429906](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212926.png)

选择后：

![image-20210526215441776](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212928.png)

![image-20210526215505463](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212930.png)



#### #值绑定总结

值绑定就是==动态的给value赋值==而已。

- 我们前面的value中的值，都是在定义Input的时候直接给定死的。
- 但是在真实开发中，这些input的值可能是从网络获取或定义在data中的
- 所以我们可以通过`v-bind:value`动态的给`value`绑定值。

**说白了就是`v-bind`在input中的应用。**



### 10.3 修饰符

1. input事件在用户输入的时候会实时地、连续地触发
2. blur事件在输入框失去焦点时触发
3. change事件在用户输入的值与上一次输入的值不同，并且输入框失去焦点时触发。

#### # .lazy

在默认情况下， `v-model`在 input 事件中同步输入框的值与数据，但你可以添加一个`修饰符 lazy` ，从而转变为在 change 事件中同步：

```html
<div id="app">
  <!--  在 "change"事件中更新 而不是 "input" 事件中更新 -->
  <input type="text" v-model.lazy="message" />
  <h1>{{message}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    message: "请输入内容",
  },
});
```



#### # .number

如果想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 `v-model` 来处理输入值：

```html
<div id="app">
  <!-- 2.number修饰符 -->
  <input type="text" v-model.number="Number" />
  <h1>{{Number}}-{{typeof Number}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    Number: 0,
  },
});
```

没加`.number`修饰符:

![image-20210526224007879](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212938.png)

加`.number`修饰符后：

![image-20210526223915093](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212940.png)



#### # .trim

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 `v-model` 上过滤输入：

```html
<div id="app">
  <!-- 3.trim修饰符 -->
  <input type="text" v-model.trim="content" />
  <h2>{{content}}</h2>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    content: "trim修饰符",
  },
});
```



## 十一、组件基础

组件（Component）是 Vue.js 最强大的功能之一。

组件可以扩展 HTML 元素，封装可重用的代码。

组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个`组件树`：

![image-20210527232110214](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212944.png)

### 11.1 基本使用

组件是==可复用的 Vue 实例==，且带有一个名字：在这个例子中是 `<button-counter>`。

我们可以在一个通过 `new Vue` 创建的` Vue根实例`中，把这个组件作为自定义元素来使用：

```html
<div id="app">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

以**全局注册组件**为例：

```vue
Vue.component("button-counter", {
template: `
<div>
  <h1>自定义组件1</h1>
</div>
`,
});
```

结果：

![image-20210527231145248](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212948.png)



> 注意：

因为==组件是可复用的 Vue 实例==，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

#### # vue组件里name属性的作用

- 当使用组件递归调用时，被递归调用的组件必须定义name属性，因为在组件里面调用自己时，不是使用的在components里注册的组件，而是使用根据name属性查找组件。

- 配合keep-alive对组件缓存做限制。

  ```html
  # 代码示例：
  <keep-alive :include="['page']">
    <router-view></router-view>
  </keep-alive>
  ....
  const User={
  	name:'user',
  	template:'<div><input type="range" /></div>'
  },
  const Page={
  	name:'page',
  	template:'<div><input type="text" /></div>'
  }
  ```

- 在dev-tools中使用：在开发中我们经常需要对代码进行调试，在dev-tools中组件是以组件name进行显示的，（如图一）这样更有语义化，方便我们快速定位到我们需要审查的位置，结构更清晰明了。

  >**对比：**
  >
  >Vue组件没添加`name`属性时，在开发工具中的显示：
  >
  >![image-20210728220530495](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212953.png)
  >
  >Vue组件添加`name`属性后，在开发工具中的显示：
  >
  >![image-20210728220738893](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119212957.png)



### 11.2 组件的复用

可以将组件进行任意次数的复用：

```html
<div id="app">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

#### #组件中的data问题

**注意：**

>Vue组件的data属性必须是一个函数，这个函数返回一个对象。

为什么组件中的data必须是一个函数？

原因：

1. 如果不是函数会报错
2. 因为函数也是一个对象，同时有自己的作用域（这样就保证了组件每次使用data时，指向的内存地址是不一样的），以至于Vue让每个组件对象的data数据都不一样(返回的对象地址会不一样)。
3. 保证组件无论复用多少次，组件中的data数据都是相互隔离，互不影响的。

```html
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <cpn></cpn>
</div>

//组件模板
<template id="cpn">
  <div>
    <h1>标题:组件中的数据必须是一个函数</h1>
    <button @click="count++">You clicked me {{ count }} times.</button>
  </div>
</template>
```

```js
// 全局注册一个组件
Vue.component("cpn", {
  template: "#cpn",
  data() {
    return {
      count: 0,
    };
  },
  // data: {
  //   message: "threey",
  // },//报错
});

const vm = new Vue({
  el: "#app",
});
```

图示:

![image-20210528133932386](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213000.png)



### 11.3 全局组件和局部组件

#### #全局组件

使用`Vue.component()`创建的组件为全局组件，全局组件==无序特意指定挂载到哪个实例上==，可在需要时直接在实例中使用，但是组件必须编写在将要使用的`根实例`之前才能生效。

全局组件:任意的`Vue实例`中可以使用全局组件。

```html
<div id="app">
  <cpn></cpn>
</div>
```

```vue
Vue.component("cpn", {
template: `
<div>
  <h1>全局组件</h1>
  <p>全局组件的段落</p>
</div>
`,
});
```

结果：

![image-20210527232526814](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213004.png)



#### #局部组件

局部组件:在`Vue根实例`中注册局部组件，这样组件只能在这个实例中使用。

```html
<div id="app">
  <cpn-C></cpn-C>
</div>
```

```vue
const cpnC = {
template: `
<div>
  <h1>局部组件</h1>
  <p>局部组件的段落</p>
  <p>1</p>
</div>
`,
};
const vm = new Vue({
el: "#app",
data: {},
components: {
// 对象属性的简介表示法
cpnC,
},
});
```

结果：

![image-20210527232928960](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213008.png)



### 11.4 父组件和子组件

最简单形成父子组件的就是在`Vue根实例`中注册局部组件：

示例1：

```js
//Vue实例就是一个root组件
const vm = new Vue({
  el: "#app",
  components: {
    cpn2: {
      template: `
<div>
<h1>我是标题2</h1>
<p>这是第二个组件</p>

</div>
`,
    },
  },
});
```

图示：

![image-20210528131203667](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213012.png)

示例2：

```html
<div id="app">
  <cpn2></cpn2>
</div>
```

```js
//注册第一个子组件
Vue.component("cpn1", {
  template: `
<div>
<h1>我是标题1</h1>
<p>这是第一个组件</p>
</div>
`,
});

// 注册第二个父组件
// Vue实例就是一个root组件
const vm = new Vue({
  el: "#app",
  components: {
    cpn2: {
      template: `
<div>
<h1>我是标题2</h1>
<p>这是第二个组件</p>
<cpn1></cpn1>
</div>
`,
    },
  },
});
```

图示：

![image-20210528131707704](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213015.png)



### 11.6 组件模板的分离写法

#### #使用script标签

以下组件模板分离写法都是以注册局部组件为例子

示例：

```html
<script type="text/x-template" id="myCpn">
  <div>
    <h1>组件模板分离写法---使用script标签</h1>
    <p>我是组件的内容，我今天学vue是第4天了</p>
  </div>
</script>
```

```js
const vm = new Vue({
  el: "#app",
  data: {},
  components: {
    cpn0: {
      //模板属性
      template: "#myCpn", //这里有坑啊，模板挂载的时候记得加上#，匹配id。
    },
  },
});
```



#### #使用template模板标签

```html
<template id="cpn1">
  <div>
    <h1>组件模板分离写法---使用template模板标签</h1>
    <p>我是组件的内容，我叫Threey</p>
  </div>  
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {},
  components: {
    cpn1: {
      template: "#cpn1",
    },
  },
});
```



### 11.7 动态组件

#### #动态组件的使用

动态组件：通过使用Vue的特殊内置组件`<component>`可以**动态地挂载不同的组件**，主要利用了`is标签属性`同时结合`v-bind`来动态绑定。

示例：

```html
<div id="app">
  <input type="text" v-model.lazy="cpnName" />
  <component :is="cpnName"></component>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    cpnName: "one",
  },
  //注册局部组件
  components: {
    one: { template: `<div><h1>我是组件一</h1></div>` },
    two: { template: `<div><h1>我是组件二</h1></div>` },
    three: { template: `<div><h1>我是组件三</h1></div>` },
  },
});
```

结果显示：

>- 原因解释：
>
>![image-20210608171559913](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213021.png)
>
>- 这里我添加了表单元素，通过双向数据绑定改变`data`中`cpnName`的值，从而实现不同组件的动态切换：
>
>![image-20210608172052437](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213027.png)
>
>![image-20210608172237583](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213030.png)
>
>



#### #动态组件的特点总结

- 动态组件就是几个组件放在一个挂载点下，然后根据父组件的某个变量来决定显示哪个，或者不显示。
- 在挂载点中使用`<component>`标签，然后使用`is=‘组件名’`，同时可以结合`v-bind`来动态绑定，它会自动去找匹配的组件名。



## 十二、组件通信

### 12.1 父组件向子组件通信

父组件向子组件通信可以理解成：

- 父组件向子组件传递数据。
- 父组件调用子组件的方法。



#### #通过props向子组件传递数据

利用`props`属性可以实现父组件向子组件传递数据

> **`props`可以理解成==标签属性的集合（propertys的简写）==，里面存放了各标签属性的值，以至于可以实现父组件给子组件传递数据。**

==props可以直接接收一个静态值，也可以接收动态值。==

示例：props接收动态值。

```html
<div id="app">
  <cpn :C-Message="message" :C-Movie="lolHero" :c-title="title"></cpn>
</div>

<!--组件模板-->
<template id="cpn">
  <div>
    <h1>{{CMessage}}</h1>
    <ul v-for="item of CMovie">
      <li>{{item}}</li>
    </ul>
    <p>{{cTitle}}</p>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "通过使用props实现父组件向子组件传递数据",
    message: "父组件向子组件传递数据",
    lolHero: ["提莫", "寒冰", "盖伦", "杰斯"],
  },
  components: {
    cpn: {
      template: '#cpn',
      props: ["CMessage", "CMovie", "cTitle"],
      //props写在子组件中，子组件使用props的属性时，对应属性的值填父组件的变量，再通过v-bind动态绑定props,即可通过props中的属性获取父组件的变量值。
    },
  },
});
```

图示： props写在子组件中，子组件使用props的属性时，对应属性的值填父组件的变量，再通过v-bind动态绑定props,即可通过props中的属性获取父组件的变量值。

![image-20210528135239018](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213038.png)



#### #数据验证

props选项不仅可以使用一个数组，当需要对props进行**类型等验证**时，就需要对象写法

type可以是下列原生构造函数中的一个：

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

![image-20210528141126188](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213043.png)

另外，type还可以是一个自定义的构造函数。

```js
function Person(firstName,lastName){
  this.firstName=firstName
  this.lastName=lastName
}
//在props接收时，这样设置
props:{
  author:Person
}
```



#### #$refs和ref

首先理解`$refs`是什么？它只不过是==Vue实例中众多属性中的一个属性==(可以自己打印一下Vue实例)。

**利用Vue实例的$refs属性可以实现父组件调用子组件的属性和方法。**

`vm.$refs`：

- 类型：`Object`
- 只读
- 详细：一个对象，持有已注册过`ref`的所有子组件

==注意：在没有ref被用来给元素或子组件注册引用信息时，$refs默认指向的是一个空对象==

如图：

![image-20210528224649500](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213048.png)

`$refs` 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅**作为一个用于直接操作子组件的“逃生舱**”——你应该避免在模板或计算属性中访问 `$refs`。



那`ref`是什么呢？从官网的回答我可以知道，它只不过是`标签属性`。

`ref`:

`ref` 被用来给DOM元素或子组件注册引用信息。**引用信息会根据父组件的` $refs 对象`进行注册**。

- 如果在普通的DOM元素上使用，引用信息就是元素; 
- 如果用在子组件上，引用信息就是组件实例.

示例一：**ref作用于组件**

```html
<div id="app">
  <cpn1 ref="cpn1"></cpn1>
  <button @click="btnRef()">点击一下</button>
</div>

<!-- 注册子组件 -->
<template id="cpn1">
  <div>
    <h1>ref和$refs的使用</h1>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  components: {
    cpn1: {
      template: "#cpn1",
      data() {
        return {
          message: "threey",
        };
      },
    },
  },
  methods: {
    btnRef() {
      console.log(this.$refs.cpn1.message);
      //结果是threey
    },
  },
});
```

示例二：**ref作用于Html标签**

```html
<div id="app">
  <h1 ref="h1">{{title}}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "ref作用于Html标签",
  },
  mounted() {
    console.dir(this.$refs.h1.textContent);
    //ref作用于Html标签
  },
});
```

图示：

![image-20210528232435309](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213054.png)

==注意：只要想要在Vue中直接操作DOM元素，就必须用ref属性进行注册==



### 12.2 子组件向父组件通信

子组件向父组件通信可以理解为：

- 子组件向父组件传值
- 子组件调用父组件的方法

与父组件向子组件通信不同的是，子组件调用父组件方法的同时，就可向父组件传值，使用`$emit`方法。



#### #$emit

​		$emit方法的主要作用是触发当前组件实例上的事件，所以子组件调用父组件方法就可以理解成==子组件触发了绑定在父组件上的自定义事件==。

示例：

```html
<div id="app">
  <h1></h1>
  <child @child-data="getChildData"></child>
</div>
```

```js
//注册全局子组件
Vue.component("child", {
  template: `<div>
<h1>子组件传递父组件数据</h1>
<h2></h2>
</div>
`,
  data() {
    return {
      list: ["threey", "rose"],
    };
  },
  mounted() {
    this.$emit("child-data", this.list);
  },
});

const vm = new Vue({
  el: "#app",
  methods: {
    getChildData(childData) {
      console.log(childData);
    },
  },
});
```

图示：

![image-20210606220541655](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213100.png)



==**总结：**==

>1. 子组件先用`$emit`发送给父组件一个`自定义事件`![image-20210606221310141](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213109.png)
>2. 父组件在`methods`中准备一个方法（方法名可以自拟）![image-20210606221521958](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213115.png)
>3. 在父组件的作用域中通过触发`自定义事件`然后触发父组件的方法，从而把子组件的数据传递给父组件
>
>![image-20210606221703559](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213122.png)



#### #$parent ---这个实际开发用的少

​		因为组件是可复用的Vue示例，其中子组件实例中有这么个`$parent`属性，通过`this.$parent`获取父组件的实例，从而调用父组件的方法。

示例：

```html
<div id="app">
  <cpn></cpn>
</div>

<template id="cpn">
  <div>
    <h2>我是cpn组件</h2>
    <ccpn></ccpn>
  </div>
</template>

<template id="ccpn">
  <div>
    <h2>我是子组件</h2>
    <button @click="btnClick">按钮</button>
  </div>
</template>
```

```js
const app = new Vue({
  el: "#app",
  data: {
    message: "你好啊",
  },
  components: {
    cpn: {
      template: "#cpn",
      data() {
        return {
          name: "我是cpn组件的name",
        };
      },
      components: {
        ccpn: {
          template: "#ccpn",
          methods: {
            btnClick() {
              // 1.访问父组件$parent
              console.log(this.$parent);
              console.log(this.$parent.name);

              // 2.访问根组件$root
              // console.log(this.$root);
              // console.log(this.$root.message);
            },
          },
        },
      },
    },
  },
});
```

结果：

![image-20210529000549149](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213128.png)

> 注意：Vue并不推荐以这种方式来实现子组件调用父组件

`$parent`这个的主要目的是作为**访问组件的应急方法**，推荐使用==$emit方法实现子组件向父组件的通信。==





### 12.3 父子组件之间的通信总结

示意图：

![image-20210529001340614](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213133.png)



### 12.4 兄弟组件的通信

兄弟组件的通信可以通过 使用`中央事件总线`实现通信。

`中央事件总线`其实就是一个`空的Vue实例`(即没有任何的属性和方法)。

实现过程:

- 发送数据===>触发自定义事件
- 接收数据===>注册自定义事件

**执行顺序：注册事件的代码先执行，触发事件的代码后执行。不然会报错**

图示：

![image-20210608141358797](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213136.png)

==注意：==

> 兄弟之间的通信利用`中央事件总线`在小型项目中会非常适合，但对于一些大型项目，要实现复杂的组件通信和状态管理，就需要使用`Vuex`。

案例演示：

```html
<div id="app">
  <msg></msg>
  <message></message>
</div>
```

```js
//定义中央事件总线
const EventBus = new Vue();
console.log(EventBus);
//注册全局组件1
Vue.component("msg", {
  template: `<div>
<h1>哥哥组件：弟弟帮哥哥拿点东西过来</h1>
<button @click="send()">发送信息</button>
</div>`,
  methods: {
    send() {
      EventBus.$emit("task", "知道了");
    },
  },
});
//注册全局组件2
Vue.component("message", {
  template: `<div>
<h1>弟弟组件：{{message}}</h1>  
</div>`,
  data() {
    return {
      message: "",
    };
  },
  created() {
    EventBus.$on("task", res => {
      this.message = res;
    });
  },
});

const vm = new Vue({
  el: "#app",
  data: {},
});
```

图示：

![image-20210608141829385](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213141.png)

点击后：

![image-20210608142005148](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213145.png)



## 十三、插槽

### 12.1 什么是插槽

​		在使用`Vue`的过程中，有时需要在**组件**中预先设置一部分内容，但是这部分内容并不确定，而是**依赖于父组件的设置**，这种情况俗称为==“占坑”==。

> 个人理解：

**插槽实质是对子组件的扩展，通过`<slot>`插槽向组件内部指定位置传递内容**。

或者这么说：

**`slot`的出现，使得父组件向子组件加入内容变得轻而易举。**

打个比方：

**有句话叫一个萝卜一个坑。父组件想要在子组件中种萝卜，需要在子组件中挖个坑，`<slot>`就是一个【萝卜坑】。父组件想要给子组件添加的内容就是【萝卜】。**



### 12.2 插槽的基本使用

示例：

```html
<div id="app">
  <!-- 
1.插槽的基本使用<slot></slot>
2.插槽的默认值<slot>默认值<slot>
3、如果有多个值，同时放入到组件进行替换，一起作为替换元素
-->
  <cpn>
    <span>{{message}}</span>
    <div>学习的第5天</div>
  </cpn>
  <cpn></cpn>
</div>
<!-- 注册子组件 -->
<template id="cpn">
  <div>
    <h1>我是子组件</h1>
    <slot>我是萝卜坑</slot>
    <button>按钮</button>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    message: "我要把萝卜填到萝卜坑里",
  },
  components: {
    cpn: {
      template: "#cpn",
    },
  },
});
```

结果示意图：

![image-20210529201601860](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213151.png)

### 12.3 slot和props的区别

`slot` 与 `props` 的区别：通过props属性，父组件可以向子组件传递数据(属性、方法)，可是父组件不能通过属性传递带标签的内容、甚至是组件。

就像如下这样：

![image-20210529202732941](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213210.png)

面对上面这种情况，插槽能完美解决。前面也说了**插槽实质是对子组件的扩展，通过`<slot>`插槽向组件内部指定位置传递内容**。

加入插槽后：

![image-20210529203745562](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213215.png)



### 12.4 匿名插槽

**匿名插槽（单个插槽、默认插槽）**

- 匿名插槽就是==没有设置name属性的插槽==。一个组件中只能有一个该类的插槽，作为找不到匹配内容片段时的**备用插槽**。
- 匿名插槽只能作为没有`slot属性`元素的插槽。

示例：

```html
<div id="app">
  <h1>我是父组件</h1>
  <cpn>
    <template>
      <h3>threey</h3>
      <h3>2181600</h3>
    </template>
  </cpn>
</div>

<!-- 子组件模板 -->
<template id="cpn">
  <div>
    <h1>我是子组件</h1>
    <slot></slot>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  components: {
    cpn: {
      template: "#cpn",
    },
  },
});
```

结果图示：

![image-20210529211606091](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213219.png)



### 12.5 具名插槽

有时需要多个插槽，需要标识出每个插槽替换哪部分内容，给每个插槽指定`标签属性name`名字，这就是`具名插槽`

**用最简单的话来说就是有名字的插槽，名字通过属性name来定义。**

示例：

```html
<div id="app">
  <cpn>
    <template v-slot:left>
      <div>开始吧</div>
      <div>{{title}}</div>
    </template>
  </cpn>
  <cpn><button slot="center">按钮</button></cpn>
</div>

<!-- 注册子组件 -->
<template id="cpn">
  <div>
    <h1>slot具名插槽的使用</h1>
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "具名插槽的使用",
  },
  components: {
    cpn: {
      template: "#cpn",
    },
  },
});
```

结果如下图：

![image-20210529212835367](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213224.png)

**注意：**

> 在向`具名插槽`提供内容时，可以在一个`<template>`模板标签元素上使用`v-slot指令`,并以`v-slot`的参数的形式提供插槽名称。

如下：

![image-20210529213840688](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213227.png)

==注意：==

**任何没有被包裹在带有` v-slot `的 template 中的内容都会被视为默认插槽的内容。**

![image-20210529214537528](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213231.png)



**不带name名字的插槽会带有隐含的名字`default`。如果你希望更明确一些，可以在一个 `<template>`中包裹默认插槽的内容：**

![image-20210529215019906](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213235.png)



### 12.6 动态插槽名

指令的动态参数，也就是用方括号括起来的JavaScript表达式作为一个`v-slot指令`的参数,因此**我们可以把需要导入的`插值name`通过写在组件的data属性中来动态设置插槽名。**

示例：

```html
<div id="app">
  <cpn>
    <template v-slot:[slotname]>
      <p>我是threey，今年22岁</p>
    </template>
  </cpn>
</div>

<!-- 子组件模板 -->
<template id="cpn">
  <div>
    <h1>动态插槽名</h1>
    <slot name="one">子组件插槽坑位</slot>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    slotname: "one",
  },
  components: {
    cpn: {
      template: "#cpn",
    },
  },
});
```

结果：

![image-20210529224906199](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213241.png)

>注意：在指定动态参数时，slotname要保持全部小写！！！



### 12.7 理解Vue中的编译作用域

==记住：==**父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

示例：

```html
<div id="app">
  <!-- Vue根实例的作用域 -->
  <cpn v-show="isshow"></cpn>
  <br />
  <!-- 因为在Vue根实例的作用域，所以能使用Vue根实例的属性 -->
  <cpn :cname="name" :cisshow="isshow"></cpn>
</div>

<!-- 注册子组件 -->
<template id="cpn">
  <div>
    <h1 v-show="cisshow">{{id}}</h1>
    <slot><span>插槽</span></slot>
    <h2>{{cname}}</h2>
  </div>
</template>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    title: "理解Vue中编译作用域",
    name: "Threey",
    isshow: true,
  },
  components: {
    cpn: {
      template: "#cpn",
      data() {
        return {
          isshow: true,
          id: 2816000,
        };
      },
      props: {
        cname: {
          type: String,
        },
        cisshow: {
          type: Boolean,
        },
      },
    },
  },
});
```

图示：

![image-20210529220447245](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213247.png)



### 12.8 作用域插槽

虽然Vue官方称它为作用域插槽，实际上我们可以把它理解成**"带数据的插槽"**

`作用域插槽`：**目的是想在父组件的作用域调用子组件的数据**。

#### 12.8.1 理解作用域插槽

一、用一个例子方便理解 `作用域插槽`：

- 第一步：我们先自定义一个子组件 `MyList`，同时在该子组件中添加插槽 `slot`。
-  第二步：在该插槽中添加默认的数据。

```vue
<template>
  <div class="mylist-wrapper">
    <h1>作用域插槽的使用</h1>
    <slot>
      <template>
        <p>姓名: {{user1.name}}</p>
        <p>年龄: {{user1.age}}</p>
        <div class="hobby">
          <span>爱好：</span>
          <ul>
            <li
              v-for="item in user1.hobby"
              :key="item.name"
            >{{item}}</li>
          </ul>
        </div>
      </template>
    </slot>
  </div>
</template>

<script>
export default {
  name: 'MyList',
  data () {
    return {
      user1: {
        name: 'threey',
        age: 20,
        hobby: ['run', 'study', 'sleep']
      },
      use2: {
        name: 'tom',
        age: 18,
        hobby: ['eat', 'watch TV', 'play basketball']
      }
    }
  }
}
</script>
```

- 第三步，在父组件中引入查看，结果显示如下。

```vue
<template>
<div id="app">
  <!-- 自定义MyList组件 -->
  <my-list></my-list>
  </div>
</template>
```

![image-20220330000728528](https://cdn.jsdelivr.net/gh/threey333/Picture/Vue.js/20220330000728.png)



二、此时我们有个需求，这个需求就是我们能不能直接**在父组件中直接拿子组件的中数据对插槽数据进行更改？**回答是肯定能。解决的思路：利用作用域插槽处理。

步骤：

- 第一步：先在子组件 `MyList` 中向 `slot插槽` 组件进行数据绑定，这样 `slot插槽组件` 上就有了该子组件 `MyList` 中的某个字段属性的数据。如下图所示： 

```vue
<slot
  :user1="user1"
  :user2="user2"
>
  <template>
<p>姓名: {{user1.name}}</p>
<p>年龄: {{user1.age}}</p>
<div class="hobby">
  <span>爱好：</span>
  <ul>
    <li
        v-for="item in user1.hobby"
        :key="item.name"
        >{{item}}</li>
    </ul>
    </div>
  </template>
</slot>
```

>==**注意**：==**绑定在 `<slot>插槽` 组件上的标签属性被称为`插槽prop`。**

- 第二步：在父组件作用域中的子组件 `myList` 中通过 `template标签` 包裹，然后利用 `v-slot:[插槽名称]=”自己定义的数据名称“`。当然也可以不自定义数据名称，通过解构也可以获取绑定在 `slot插槽上的数据`。

```vue
<my-list>
  <template v-slot:default="slotData">
{{slotData}}
  </template>
</my-list>
```

结果图示：

![image-20220330003628056](https://cdn.jsdelivr.net/gh/threey333/Picture/Vue.js/20220330003628.png)

从图中可以看到，我们已经获取到了绑定在 `slot插槽` 上的所有数据。

- 第三步，更换插槽显示的数据。

```vue
<template>
  <div id="app">
    <!-- MyList组件 -->
    <my-list>
      <template v-slot:default="slotData">
        <p>姓名: {{slotData.user2.name}}</p>
        <p>年龄: {{slotData.user2.age}}</p>
        <div class="hobby">
          <span>爱好：</span>
          <ul>
            <li
              v-for="item in slotData.user2.hobby"
              :key="item.name"
            >{{item}}</li>
          </ul>
        </div>
      </template>
    </my-list>
  </div>
</template>
```

结果图示：

![image-20220330004002610](https://cdn.jsdelivr.net/gh/threey333/Picture/Vue.js/20220330004002.png)



#### 12.8.2 多个作用域插槽

示例：

```html
<div id="app">
  <cpn>
    <template v-slot:default="ids">
      <p v-for="item of ids.heroName">{{item}}</p>
    </template>

    <template v-slot:one="one">
      <br />
      <h1>插槽1</h1>
      <p>{{one.heroName[1]}}</p>
    </template>

    <template v-slot:two="two">
      <br />
      <h1>插槽2</h1>
      <p>{{two.heroName[2]}}</p>
    </template>
  </cpn>
</div>

<!-- 注册子组件模板 -->
<template id="cpn">
  <div>
    <h1>多个作用域插槽</h1>
    <!-- 这里有个坑，v-bind绑定的标签属性如果有多个单词，不能用驼峰命名，要用‘-’连接 -->
    <p><slot :hero-Name="heroName">插槽坑位0</slot></p>

    <slot name="one" :hero-Name="heroName">插槽坑位1</slot>

    <slot name="two" :hero-Name="heroName">插槽坑位2</slot>
  </div>
</template>
```

```js
const mv = new Vue({
  el: "#app",
  components: {
    cpn: {
      template: "#cpn",
      data() {
        return {
          message: "haha",
          heroName: ["提莫", "蛮王", "赵信", "寒冰"],
        };
      },
    },
  },
});
```

结果：

![image-20210529223354374](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213303.png)



## 十四、Vue.js组件的生命周期

### 14.1 什么是组件的生命周期

在Vue中，每个组件都有自己的生命周期，所谓的生命周期，指的是==组件自身的一些方法（或者叫做钩子函数）==，这些方法在`特殊的时间点`或`遇到特殊的框架事件`时会被自动触发。

![](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213310.png)

Vue生命周期中所有钩子函数：**前八个是常见的！**

>- beforeCreate     ——创建前
>- created               ——已创建
>- beforeMount     ——挂载前
>- mounted             ——已挂载
>- beforeUpdate     ——更新前
>- updated               ——已更新
>- beforeDestory    ——卸载前
>- destroyed            ——已卸载
>
>
>
>- activated              ——活性
>- deactivated          ——停用



### 14.2 组件生命周期的三大阶段

- 挂载阶段 - 进入页面阶段
- 更新阶段 - 数据发生改变,重新熏染
- 卸载阶段 - 页面关闭

### 14.3 挂载阶段

挂载阶段又可以分为：

- 第一小阶段：数据初始化，比如data:{ msg："数据初始化阶段"}。
- 第二小阶段：找模板进行编译（只是把内容准备好，并没有渲染在节点上）：比如：<h1>{{msg}}</h1>
- 第三小阶段：执行 `render方法` 进行DOM渲染（将Vue实例挂载DOM上），比如：<h1>数据初始化阶段</h1>

![image-20210605211429943](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213317.png)



#### #挂载阶段--第一小阶段beforeCreate和created

> 1.beforeCreate方法：

这个方法在数据初始化之前调用，以至于==**它无法获取Vue组件中data里定义的数据**==，官方也不推荐在这里操作data数据。

> 2.created方法（非常重要）：

这个方法在数据初始化之后调用，这样它==**可以获取Vue组件中data里定义的数据**==。

在这个方法中可以：

- 发送ajax请求。
- 操作data里的数据。
- 获取本地存储数据。

示例：

```html
<div id="app">
  <h1>{{ msg }}</h1>
</div>
```

```js
const vm = new Vue({
  el: "#app",
  data: {
    msg: "测试信息",
  },
  methods: {
    fn() {
      console.log('你好')
    },
  },
  beforeCreate() {
    console.warn("beforeCreate:", this.msg, this.fn);
  },
  created() {
    console.warn("created:", this.msg, this.fn);
    // $.ajax(..)
    // this.msg =  123
    // localStorage.set/get...
  },
});
```

图示：

![image-20210605214703575](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213322.png)



#### #挂载阶段--第二小阶段(找模板)

- 在`created`方法执行时，会判断当前实例是否设置了`el`(即挂载点)。如果设置了，就继续向下编译执行，如果没有，则停止编译，也就意味着停止了生命周期，直到该`Vue组件`上调用`vm.$mount(el)`，生命周期才恢复。
- 当寻找到挂载点后，还会继续寻找是否有`模板	`，如果指定了`template选项`，则会将其作为模板进行编译成`render函数`（即template中的HTML内容会渲染到el这个节点内部）。如果没有指定`template选项`，则会将el的`outerHTML`作为模板进行编译。
- 这个小阶段还没有渲染到界面上，仅存在内存中。

![image-20210606141513592](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213326.png)

示例：

![image-20210606141217313](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213330.png)



#### #挂载阶段--第三阶段beforeMount和mounted

这个小阶段也是==DOM渲染阶段==，即Vue组件渲染HTML内容。

> 1.beforeMount方法

这个方法是在挂载渲染之前执行的。在这个方法中得到时DOM渲染之前的内容。如下所示：

![image-20210606142131863](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213335.png)

>2.mounted方法

这个阶段就开始真正地执行`render方法`进行渲染，之前设置的el会被`render函数`执行的结果所替换，也就是说**将结果真正渲染到当前Vue实例的el节点上，这时会调用`mounted方法`。**如下所示：

![image-20210606142614035](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213340.png)

在这个方法中可以进行：`操作DOM`、`发送ajax请求`

这阶段的图示：

![image-20210606143126308](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213345.png)



### 14.4 更新阶段

当Vue实例data中的数据发生了改变，就会触发对应组件的重新渲染，这时就会触发更新阶段的两个钩子函数：`beforeUpdate`和`updated`

#### #beforeUpdate和updated

>1.beforeUpdate

这个方法在普通的Vue实例data数据发生改变，是==获取不了更新后DOM渲染的新数据。==

```js
beforeUpdate() {
  console.warn("beforeUpdate", document.querySelector("h1").innerText);
},
  updated() {
    console.warn("updated", document.querySelector("h1").innerText);
  },
```

结果：beforeUpdate获取不了更新之后的DOM新数据。

![image-20210606144858166](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213350.png)

**但在双向数据绑定中还是可以获取更新后的数据。**

>2.updated

这个方法是在DOM页面更新后调用，所以`updated`方法可以获取更新之后的DOM

![image-20210606144858166](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213354.png)

这个阶段的图示：

![image-20210606145418222](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213357.png)



### 14.5 卸载阶段

既然组件有创建，也必然会有消亡。

**如果频繁调用创建的代码，但是一直没有清除就会造成内容飙升，而且一直不会释放，这时就有可能会导致"内存泄漏"问题，这也是销毁的意义。**

#### #beforeDestroy和destroyed

> 1.beforeDestroy

beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。

>2.destroyed

`destroyed`钩子函数在Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解除绑定，**所有的事件监听器都会移除，所有的当前实例的子组件也会被销毁。**



使用`vm.$destroy()`可以模拟关闭页面后的场景。如下图所示：

![image-20210606151114905](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213402.png)

这样就能看到在关闭页面后，`beforeDestroy`和`destroyed`这两个方法的执行情况。

对于开发人员自己写的一些`定时器`、`创建的DOM`，需要开发人员自己写在`beforeDestroy`钩子函数里面，然后手动执行清理。

示例：在数据初始化后，我添加了一个定时器，我们在`beforeDestroy`钩子函数中手动清除定时器，不然定时器不会被销毁。

```js
created() {
  console.warn("created", this.title);
  this.timer = setInterval(() => {
    console.log("threey最厉害的!");
  }, 1000);
},
  beforeDestroy() {
    console.warn("beforeDestory");
    clearInterval(this.timer);
  },
```

图示：

不手动清除定时器的结果:

![image-20210606152633192](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213405.png)

在`beforeDestroy`钩子函数中手动清除定时器任务的结果：

![image-20210606152507323](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213409.png)



卸载阶段图示：

![image-20210606151658837](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211119213415.png)



### 14.6 两个特殊的生命周期方法

`activated`和`deactivated`这两个方法并不是Vue组件的生命周期方法，它们的触发要结合`vue-router`和`keep-alive`来使用。

- `activated`表示当vue-router的页面被打开时，会触发这个钩子函数。
- `deactivated`表示当vue-router的页面被关闭时，会触发这个钩子函数。

