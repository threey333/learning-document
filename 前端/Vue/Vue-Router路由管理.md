# Vue-Router路由管理

## 一、什么单页应用

​		单页应用是一种基于移动web的应用或者网站，这种Web应用大多数由一个完整的HTML页面组成，页面之间的切换通过不断地替换HTML内容或者隐藏和显示所需要的内容来实现。

>单页应用有以下特点：
>
>- 单页应用在页面加载时会将整个应用的资源文件都下载下来（在无“懒加载”的情况）
>- 单页应用的页面内容有前端JavaScript逻辑所生成，在初始化时由一个空的<div>占位。
>- 单页应用的页面切换一般通过修改浏览器的哈希(Hash)来记录和标识。

​		**单页应用在首次打开会加载所有的资源文件，以至于等待的时间要长一些，另外，单页应用的HTML是一个空的div，不利于搜索引擎优化(SEO)!**

​		单页应用要进行页面切换，需要Vue Router来实现。



## 二、Vue Router概述

Vue Router是Vue.js官方的路由管理器，它和Vue.js的核心深度集成，让构建单页面应用变得易如反掌。

它包含的功能有：

- 嵌套的路由、视图表。
- 模块化的、基于组件的路由配置。
- 路由参数、查询、通配符。
- 基于Vue.js过渡系统的视图过渡效果。
- 细粒度的导航控制。
- 带有自动激活的CSS class的链接。
- HTML5历史模式(history)或哈希(Hash)模式，在IE9中自动降级。
- 自定义的滚动条行为。

### 1.1 安装Vue Router

#### #直接下载/CDN

CDN链接地址：https://unpkg.com/vue-router/dist/vue-router.js

通过`<script>标签`的方式导入Vue Router，**前提是必须要先导入Vue.js**

#### #NPM

```shell
npm install vue-router
```

如果在一个模块化工程中使用它，必须要通过 `Vue.use()` 明确地安装路由功能：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

如果使用全局的 `<script> 标签`，则无须如此 (手动安装)。



### 1.2 简单的组件路由基本使用

​		**在Vue项目中使用路由的基本目的就是为了实现页面之间的切换。**在单页面应用中，页面的切换主要是控制一个`容器<div>`，替换或显示和隐藏。

示例：

```html
<div id="app">
  <p>
    <router-link to="/page1">导航page1</router-link>
    <router-link to="/page2">导航page2</router-link>
  </p>
  <!-- 路由占位符 -->
  <router-view></router-view>
</div>

```

上面的标签解释：

- `<router-link>`：用来导航，通过传入 `to` 属性指定链接。同时`<router-link>` 默认会被渲染成一个 `<a>` 标签
- `<router-view>`：路由匹配到的组件将渲染在这里

```js
//创建page1的局部组件
const PageOne = {
  template: `<div>PageOne</div>`,
};
//创建page2的局部组件
const PageTwo = {
  template: `<div>PageTwo</div>`,
};

//配置路由信息
const router = new VueRouter({
  routes: [
    { path: "/page1", component: PageOne },
    { path: "/page2", component: PageTwo },
  ],
});
const vm = new Vue({
  el: "#app",
  router: router, //将router赋值给Vue根实例
});
```

结果：

![image-20210530202443066](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010319.png)



### 1.3 路由导航菜单的高亮处理

- **点击导航 =>** 元素里添加了两个类

```html
<a href="#/page/1" class="router-link-exact-active router-link-active">导航1</a>
<a href="#/page/2" class="">导航2</a>
```

- **修改方式1 : 直接修改类的样式**

```css
.router-link-exact-active,
.router-link-active {
  color: red;
  font-size: 50px;
}
```

- **修改方式2 : 使用存在过的类样式 => 修改默认高亮类名** 

```csss
.red {
color: red;
font-size: 30px;
}
```

```js
const router = new VueRouter({
  routes: [],
  // 修改默认高亮的a标签的类名
  // red 是在CSS中已经定好的类名
  linkActiveClass: 'red'
})
```

#### #精确匹配和模糊匹配

- 精确匹配 : router-link-exact-active 类名 : 只有当 `浏览器地址栏中的哈希值 与 router-link 的 to 属性值,完全匹配对,才会添加该类`
- 模糊匹配: router-link-active 类名 : 只要 `浏览器地址栏中的哈希值` 包含 router-link 的 to 属性值,就会添加该类名
- 解决办法 : 加个 `exact`，已示精确定位。

```js
<router-link to="/" exact>Page1</router-link>
```

- ==**注意 : 精确匹配和模糊匹配，只对添加类名这个机制有效，与路由的匹配规则无关！！！**==

示例：

```html
<div id="app">
  <h1>{{title}}</h1>
  <!-- 为了避免模糊对添加类名的影响，可以在路由导航处加exact属性，以示精确定位 -->
  <router-link to="/" exact>默认页</router-link>
  <router-link to="/page/1">导航1</router-link>
  <router-link to="/page/2">导航2</router-link>
  <router-link to="/page/3">导航3</router-link>
  <router-link to="/page/4">导航4</router-link>

  <!-- 路由匹配到的信息渲染在这里 -->
  <router-view></router-view>
</div>
```

```css
<style>
.router-link-exact-active,
.router-link-active {
  color: red;
  font-size: 50px;
}
</style>
```

```js
const content = {
  template: `<div>
<h1>{{$route.params.id}}</h1>
</div>`,
};
const cpn = {
  template: `<div>
<h1>默认页面</h1>  
</div>`,
};

//配置路由信息
const router = new VueRouter({
  routes: [
    { path: "/", component: cpn },
    { path: "/page/:id", component: content },
  ],
  //red类是提前在css中定义好的
  linkExactActiveClass: "red",
});

const vm = new Vue({
  el: "#app",
  data: {
    title: "路由导航菜单高亮处理",
  },
  router,
});
```

结果：不加`exact`之前的情况

![image-20211117010643227](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010653.png)

加了`exact`之后的情况：

![image-20210608124537904](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010200.png)



## 三、动态路由

### 3.1 动态路由匹配

​		**很多时候我们需要把不同路径的路由==全部映射到同一个组件==。**例如：有一个User组件，对于所以ID各不相同的用户，都要使用这个组件来渲染，则可以在vue-router的路由路径中使用**"动态路径参数"**来实现这个效果。

代码示例：

```html
<div id="app">
  <h2>
    <router-link to="/page/1">导航page1</router-link>
    <router-link to="/page/2">导航page2</router-link>
    <router-link to="/page/3">导航page3</router-link>
  </h2>
  <!-- 路由匹配到的组件将渲染在这里 -->
  <h1><router-view></router-view></h1>
</div>
```

```js
const page = {
  template: `<h1>内容{{ $route.params.id }}</h1>`,
};

//创建路由信息
const router = new VueRouter({
  // 路由路线
  routes: [
    { path: "/", redirect: "page1" },
    { path: "/page/:id", component: page },
  ],
});

const vm = new Vue({
  data: {
    message: "ni hao ",
  },
  router,
}).$mount("#app");
```

**通过`:id`的方式可以指定路由的路径参数**，现在像`/page/1`、`/page/2`、`/page/3`这样的`URL`都会映射到同一个组件中。

图示：

![image-20210602151829235](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010154.png)

==**路径参数**==用冒号 `:` 表示。当一个路由被匹配时，它的 `params` 的值将在每个组件中以`this.$route.params` 的形式暴露出来。

以上面的例子来说，当一个路由被匹配时，==在page组件插值表达式中使用`$route.params.id`可以获取这个id参数。==

如果是在方法中使用，则可以使用`this.$route.params.id`，**注意是`$route`而不是`$router`。**

![image-20210602153408104](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010147.png)



你可以在**同一个路由中**设置有多个 ==**路径参数**==，它们会映射到 `$route.params` 上的相应字段。

例如：

| 模式                          | 匹配路径            | $route.params                 |
| ----------------------------- | ------------------- | ----------------------------- |
| /user/:username               | /user/evan          | {username:'evan'}             |
| /user/:username/post/:post_id | /user/evan/post/123 | {username:'evan',post_id:123} |



### 3.2 高级匹配匹配

如果想实现更高级的正则表达式路径匹配，vue-router也是支持的。

正则表达式路径匹配学习网址：https://github.com/pillarjs/path-to-regexp/tree/v1.7.0

简单的示例:

```js
const router=new VueRouter({
  routes:[
    //正则表达式匹配，id为数字的路径
    {path:'/user/:id(\\d+)',component:User},
    //正则表达式匹配，all后面可以跟任何字符，例如/all/abc或/all/123/a
    {path:'/all/*',components:User}
  ]
})
```

### 3.3  匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：路由定义得越早，优先级就越高。



### 3.4 $route

一个`路由对象 (route object) `表示**当前激活的路由状态信息**，包含了当前 URL 解析得到的信息 。

**一个`哈希值路径`相当于一个`路由对象`。**

示例：

```html
<router-link to="/page/4?age=30#threey">导航4</router-link>
```

```js
{ path: "/page/:id", name: "page", component: revice, props: true },
```

下图为`$route对象`的属性信息：

![image-20210607234938372](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010053.png)

属性解释：

>1.$route.path
>
>- 类型：`String`
>- 字符串，对应当前路由的路径，总是解析为绝对路径，如`"/page/4"`。
>- 范围：`#后面？前面的内容`
>
>2.$route.params
>
>- 类型：`object`
>- 一个 key/value 对象，包含了动态片段和全匹配片段，**如果没有路由参数，就是一个空对象**。
>
>3.$route.query
>
>- 类型：`object`
>- 查询参数
>- 一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/page/4?age=30`，则有 `$route.query.age === 30`，如果没有查询参数，则是个空对象。
>
>4.$route.hash
>
>- 类型：`string`
>- 当前路由的 `hash 值` (带 `#`) ，如果没有 hash 值，则为空字符串。
>
>5.$route.fullPath
>
>- 类型: `string`
>- 全路径
>- 完成解析后的 URL，包含`查询参数`和 `hash 的完整路径`。





### 3.5 响应路由参数的变化

​		使用带有参数的路由时需要注意的是，当用户从 `/users/johnny` 导航到 `/users/jolyne` 时，**相同的组件实例将被重复使用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会被调用。**



​		当使用路由来实现页面切换时，有时需要能够监听到这些切换的事件，例如`/page1`切换到`/page2`时，可以使用`watch监听属性`来获取这个事件。

#### #路由切换对应的是同一个子组件

代码示例1：路由切换对应的是相同组件

```html
<div id="app">
  <h2>
    <router-link to="/page/1">导航page1</router-link>
    <router-link to="/page/2">导航page2</router-link>
    <router-link to="/page/3">导航page3</router-link>
  </h2>
  <!-- 路由匹配到的组件将渲染在这里 -->
  <h1><router-view></router-view></h1>
</div>
```

```js
const page = {
  template: `<h1>内容{{ $route.params.id }}</h1>`,
  watch: {
    //to表示切换之后的路由，from表示切换之前的路由
    $route: {
      handler: function (to, from) {
        //在这里处理响应
        console.log(to, from);
      },
      immediate: true,
    },
  },
};

//创建路由信息
const router = new VueRouter({
  // 路由路线
  routes: [
    { path: "/", redirect: "page1" },
    { path: "/page/:id", component: page },
  ],
});

const vm = new Vue({
  data: {
    message: "ni hao ",
  },
  router,
}).$mount("#app");
```

结果：

![image-20210602161817320](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010047.png)

上面`immediate`属性表示在watch中首次绑定时，是否执行监听方法handler，值为true则表示在watch中声明时，就立即执行`handler方法`，若为false，则和一般使用watch一样，在数据发生变化才执行hadnler。

![image-20210602164024361](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010042.png)

这里watch监听`$route`属性写成了对象形式，其实也可以写成函数形式：

```js
watch: {
  $route(to, from) {
    console.log(to, from);
  },
},
};
```

**函数形式时，immediate默认是false。**



#### #路由切换对应的是不同的组件

示例2：路由切换对应的是不同的组件

```html
<div id="app">
  <p>
    <router-link to="/page1">导航page1</router-link>
    <router-link to="/page2">导航page2</router-link>
  </p>
  <!-- 路由占位符 -->
  <router-view></router-view>
</div>
```

```js
//创建page1的局部组件
const PageOne = {
  template: `<div>PageOne</div>`,
};
//创建page2的局部组件
const PageTwo = {
  template: `<div>PageTwo</div>`,
};

//配置路由信息
const router = new VueRouter({
  routes: [
    { path: "/page1", component: PageOne },
    { path: "/page2", component: PageTwo },
  ],
});
const vm = new Vue({
  el: "#app",
  router: router, //将router赋值给Vue根实例
  watch: {
    $route: function (to, from) {
      console.log(to, from);
    },
  },
});
```

结果示意图：

![image-20210602165007574](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010035.png)

==注意：==

**当路由切换对应的是不同的组件时，监听方法需要写在根组件中。**



#### #watch监听响应路由参数变化的总结：

>- watch监听属性是监听`$route`对象上的属性。如上图所示
>- 当路由切换对应的是同一个子组件时，例如上面的page组件，只是参数id不同，那么监听方法可以写在子组件Page中。
>- 当路由切换对应的是不同的组件时，例如上面的PageOne和PageTwo，那么监听方法需要写在Vue根组件中才可以接收到变化。

​		==**设置在根组件的watch方法，在上面两种情况下都会触发，所以建议统一在根组件中来设置watch监听路由变化**==



## 四、嵌套路由

实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

![image-20210602170235604](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010027.png)

借助 `vue-router`，使用嵌套路由配置，就可以很简单地表达这种关系。

嵌套路由示例：

```html
<div id="app">
  <!-- 路由导航标签 -->
  <h1>
    <router-link to="/page1">导航1</router-link>
    <router-link to="/page2">导航2</router-link>
  </h1>

  <!-- 路由匹配到的信息都放再这里 -->
  <router-view></router-view>
</div>
```

```js
const page1 = {
  template: `<div>pag1内容</div>`,
};
const page2 = {
  template: `<div>page2内容查看:
<hr/>
<h3>
	<router-link to="/page2/tab1">内容1</router-link>
	<router-link to="/page2/tab2">内容2</router-link>
</h3>

<router-view></router-view>
</div>`,
};
const tab1 = {
  template: `<div>tab1子组件内容</div>`,
};
const tab2 = {
  template: `<div>tab2子组件内容</div>`,
};

// 配置路由信息
const router = new VueRouter({
  routes: [
    { path: "/", redirect: "page1" },
    { path: "/page1", component: page1 },
    {
      path: "/page2",
      component: page2,
      // children 数组标识子路由规则
      children: [
        { path: "/page2/tab1", component: tab1 },
        { path: "/page2/tab2", component: tab2 },
      ],
    },
  ],
});

const vm = new Vue({
  el: "#app",
  router,
});
```

结果示意图：

![image-20210602170648882](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010021.png)



## 五、编程式导航

除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

**注意：在Vue实例中，你可以通过 `$router` 访问路由实例。因此你可以调用这个路由实例中的方法。**

### 5.1 router.push()

`push方法`是由`VueRouter`的实例对象提供的。

>```js
>router.push(location, onComplete?, onAbort?)
>```

​		`router.push`方法的**第一个参数：**可以是`字符串路径`，也可以是一个==描述地址的对象==，在这个对象中可以设置传递到下一个路由的参数。

​		`router.push`方法的**第二个和第三个参数：**这两个参数分别接收一个回调函数，它们分别表示当导航成功时触发和导航失败时触发。这两个参数不是必须要传入的。

push方法示例：

```js
//字符串
router.push('home')

//对象
router.push({path:'home'})

//带查询参数，变成 /user?userId=test
router.push({path:'/user',query:{userId:' test '}})

//命名路由
router.push({name:'user',params:{userId:'123'}})
```

上面==带查询参数==说明：path对应路由配置信息中定义的path，query是设置传递的参数。



案例示例：

```html
<div id="app">
  <h1>{{title}}</h1>
  <!-- 路由匹配 -->
  <router-link to="/user/1">导航1</router-link>
  <router-link :to="{name:'user',params:{id:2}}">导航2</router-link>
  <router-link :to="{name:'user',params:{id:3}}">导航3</router-link>
  <router-link :to="{name:'user',params:{id:4}}">导航4</router-link>
  <router-link to="/register">注册</router-link>

  <!-- 匹配到的路由都渲染在这里 -->
  <router-view></router-view>
</div>

```

```js
const cpn1 = {
  template: `<div>
<h3>路由子组件1</h3>
<button @click="tocpn4">跳转到注册页</button>
</div>`,
  methods: {
    tocpn4() {
      this.$router.push("/register");
    },
  },
};
const cpn2 = {
  props: ["id"],
  template: `<div>路由子组件{{id}}</div>`,
};

const register = {
  template: `<div>
<h2>Register组件</h2>
<button @click="back()">后退</button>  
</div>`,
  methods: {
    back() {
      this.$router.go(-1);
    },
  },
};

//配置路由信息
const router = new VueRouter({
  routes: [
    { path: "/user/1", component: cpn1 },
    //命名路由
    { path: "/user/:id", name: "user", component: cpn2, props: true },
    { path: "/register", component: register },
  ],
});
const vm = new Vue({
  data: {
    title: "命名的路由",
  },
  router,
}).$mount("#app");
```

结果：

![image-20210602185628097](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010013.png)



### 5.2 router.replace()

>```js
>router.replace(location, onComplete?, onAbort?)
>```

​		`router.replace`方法可以进行路由切换从而实现导航，与`router.push`很像，唯一的不同是，它不会向history添加新记录。跟它的方法名一样：**替换（replace）掉当前的history记录**。==**也就是当用户单击浏览器返回时，并不会向history添加记录。**==

> push方法和replace方法对比如下：

**push方法：**

![image-20210602213135303](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010007.png)

原来的页面：

![image-20210602213209391](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010003.png)

**replace方法：**

![image-20210608125032185](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005957.png)

原来的页面是：

![image-20210602213209391](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005952.png)

但它点浏览器返回键回到的是最初的页面：这也就说明它**并不会向history添加记录，只是单纯的替换了当前的页面。**

![image-20210602213631773](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005949.png)



### 5.3 router.go(Number)

该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步，类似于 `window.history.go(n)`。

例子：

```js
// 向前移动一条记录，与 router.forward() 相同
router.go(1)

// 返回一条记录，与router.back() 相同
router.go(-1)

// 前进 3 条记录
router.go(3)

// 如果没有那么多记录，静默失败
router.go(-100)
router.go(100)
```



## 六、命名路由

### 6.1 命名路由

​		有时候，通过一个**名称(name)**来标识一个路由显得更方便一些，特别是在链接一个路由或者是执行一些跳转时。在创建Router实例时，并在routes配置中给某个路由设置名称name属性。

代码示例：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给 `router-link`组件的 `to标签属性`传一个对象：

```html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

这跟代码调用 `router.push()` 是一回事：

```js
router.push({ name: 'user', params: { userId: 123 } })
```

**这两种方式都会把路由导航到 `/user/123` 路径。**



### 6.2 name+params和path+query的区别

使用`name+params`和`path+query`的区别总结如下：

- 进行路由配置时，path是必配的，而name则可以选配。

- 使用name进行导航时，传参可以使用 ` params ` ，接收参数需要使用`$route.params`，或者传参使用`query`，接收参数使用`$route.query`。

- 使用`path`进行导航时，传递参数只能使用`query`，接收参数需要使用`$route.query`。

  注意：

  >如果只是单纯使用`path`进行导航而没有使用`query查询参数`，这时`query`默认是空对象。

- **query更加类似于ajax中的get传参，params则类似于post**，简单来说，前者在浏览器地址栏中显示参数，后者则不显示。



## 七、路由组件传参

### 7.1 将 props 传递给路由组件

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦：

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const routes = [{ path: '/user/:id', component: User }]
```

替换成

```js
const User = {
  props: ['id'],   //代替this.$route.params.id
  template: '<div>User {{ id }}</div>'
}
const routes = [{ path: '/user/:id', component: User, props: true }]
```

这允许你在任何地方使用该组件，使得该组件更容易重用和测试。



### 7.2 布尔模式

当`props`被设置为`true`时，`$route.params`的内容将会被设置为组件属性，==在组件中可以使用props接收==。

代码示例：

```html
<div id="app">
  <!-- 路由导航 -->
  <router-link to="/page1/page1组件">导航1</router-link>
  <router-link to="/page2/page2组件">导航2</router-link>

  <!-- 路由匹配到的信息渲染在这里 -->
  <router-view></router-view>
</div>
```

```js
const page1 = {
  //组件使用props接收路由params参数
  props: ["name"],
  template: `
<h3>{{name}}</h3>
`,
};
const page2 = {
  //组件使用props接收路由params参数
  props: ["name"],
  template: `
<h3>{{name}}</h3>
`,
};

//$route与对应路由形成高度耦合，不够灵活，所以可以使用props将组件和路由解耦
const router = new VueRouter({
  routes: [
    { path: "/page1/:name", component: page1, props: true },
    { path: "/page2/:name", component: page2, props: true },
  ],
});

const vm = new Vue({
  el: "#app",
  router,
});
```

结果：

![image-20210603003853868](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005941.png)

![image-20210603003803301](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005937.png)



### 7.3 对象模式

当 `props` 是一个对象时，它将原样设置为组件 props。==**当 props 是静态的时候很有用。**==

代码示例：

```js
const page1 = {
  //组件使用props接收
  props: ["name", "uid", "age"],
  template: `
<h3>{{name}}：学号是：{{uid}},年龄：{{age}}</h3>
`,
};

const router = new VueRouter({
  routes: [
    { path: "/", redirect: "/page1/:name" },
    {
      path: "/page1/:name",
      component: page1,
      props: { name: "threey", uid: 2181600, age: 22 },
    },
  ],
});
```

结果：

![image-20210603004353927](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005932.png)



### 7.4 函数模式

可以创建一个函数返回`props`。这个函数会提供一个route参数，**这样就可以将参数转换为另一种类型，将`静态值`与`基于路由的值`结合。**

代码示例：

子组件cpn1部分

```js
const cpn1 = {
  props: ["id", "uname", "age"],
  template: `<div>
<h1>{{id}},{{uname}},{{age}}</h1>
</div>
`,
};
```

配置路由信息部分

```js
const router = new VueRouter({
  routes: [
    { path: "/", redirect: "/cpn1" },
    {
      path: "/cpn1/:id",
      component: cpn1,
      name: "cpn1",
      props: (route) => (return {
        id: route.params.id,
        uname: "threey",
        age: 20,
      }),
    },
  ],
});
```

结果：

![image-20210603005121379](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005926.png)

![image-20210603005211162](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005922.png)

上面两张图可以发现**基于路由的值会发生动态改变，静态值是不变的**，这样有利用**值的动静态结合**。



### 7.5 命名视图

如果==**同级**==情况下想同时呈现多个视图，可以使用命名视图方式实现

简单来就是给`<router-view>`设置名字`name标签属性`，然后去路由规则中找相关名字的组件。

示例：

> 需求：在“/”url下同时显示三个组件，一般来说留三个出口是没问题的，但是你会发现出口都只显示相同的组件。
>
> 步骤：
>
> - 要想在同级情况下显示多个组件，可以给`router-view`
>
> - 指定名字`name`,那么它就会在路由规则中，找相关名字的组件，然后将该组件渲染在出口处。如图：![image-20210609142847205](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005917.png)
>
>   ![image-20210609143033734](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005906.png)
>
> - 如果router-view没有指定name，那么它会它会在路由规则中找`key为default`的组件，然后将其渲染在出口处。如图：
>
>   ![image-20210609143146517](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005859.png)
>
>   ![image-20210609143239409](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005854.png)

演示的全代码如下：

```html
<div id="app">
  <!-- 路由匹配到的信息渲染在这里，相当于路由出口 -->
  <router-view></router-view>
  <router-view></router-view>
  <router-view name="headbar"></router-view>
  <router-view name="Footer"></router-view>
</div>
```

```js
//路由组件
const HeadBar = {
  template: `<div><h1>头部部分</h1></div>`,
};
const Main = {
  template: `<div><h1>身体部分</h1></div>`,
};
const Footer = {
  template: `<div><h1>底部部分</h1></div>`,
};

//配置路由信息
const router = new VueRouter({
  routes: [
    {
      path: "/",
      components: {
        default: HeadBar,
        headbar: Main,
        Footer,
      },
    },
  ],
});

const vm = new Vue({
  el: "#app",
  data: {},
  router,
});
```

结果：

![image-20210609143540564](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005847.png)



## 八、路由的重定向和别名及元信息

### 8.1 路由重定向

重定向也是通过`routes`配置来完成。

“重定向”的意思是，当用户访问`/a`时，URL将会被替换成`/b`，然后匹配路由为`/b`。

下面例子是从`/a`重定向到`/b`：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```

重定向的目标也可以是一个`命名的路由`：

```js
const router=new VueRouter({
  routes:[
    {path:'/a',redirect:{name:'foo'}}  //从/a重定向到命名路由foo
  ]
})
```

甚至是一个`方法`，**动态返回重定向目标**：

```js
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
```

==注意：==

- **导航不会作用在redirect之前的路由上，只会在redirect之后的目标路由上。**
- **一个路由如果设置了redirect，那么这个路由本身对应的组件视图也不会生效，**也就是说==无需给redirect路由配置component==。



### 8.2 别名

“别名”又是什么呢？

**`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

上面对应的路由配置为：

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。



### 8.3 路由元信息

当我们打印路由组件当前的路由信息`this.$route`时,里面有这么一个属性`meta`，这个属性就是`路由元信息`。

如图：

![image-20210609151039552](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005839.png)

> **路由元信息的作用：我们可以在这里设置一些`自定义信息`，供页面组件或者`导航守卫`和`路由钩子函数`使用。**

示例:

```html
<div id="app">
  <!-- 路由导航入口 -->
  <router-link to="/one">One</router-link>
  <router-link to="/two">Two</router-link>

  <!-- 路由渲染在这里，即出口 -->
  <router-view></router-view>
</div>
```

```js
//路由组件
const One = {
  template: `
<div><h1>子组件：one</h1></div>
`,
  created() {
    document.title = this.$route.meta.title;
    console.log(this.$route.meta.title);
  },
};
const Two = {
  template: `
<div><h1>子组件：two</h1></div>
`,
  created() {
    console.log(this.$route);
    document.title = this.$route.meta.title;
    console.log(this.$route.meta.title);
  },
};

//配置路由规则
const router = new VueRouter({
  routes: [
    { path: "/one", component: One, meta: { title: "threey" } },
    { path: "/two", component: Two, meta: { title: "ethon" } },
  ],
});

const vm = new Vue({
  el: "#app",
  router,
});
```



在组件中可以通过插值表达式使用`$route.meta.title`：

![image-20210609151715814](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005833.png)

结果：

![image-20210609151759083](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005829.png)

还可以在`全局前置路由守卫beforeEach`中获取`meta`信息，然后修改HTML页面的title，代码如下：

```js
router.beforeEach(function(to,from,next){
  window.document.title=to.meta.title;
  next();
})
```



## 九、导航守卫

所谓导航守卫，可以理解成**拦截器**或者**路由发送变化时的钩子函数**。

导航守卫分为多种：

- 全局前置守卫
- 全局解析守卫
- 全局后置钩子
- 组件内守卫
- 路由配置守卫

每当页面发送路由变化时，可以把这种路由引起的路径变化称为`导航`。

### 9.1 全局前置守卫(常用)

全局前置守卫需要直接注册在`router`对象上，可以使用`router.beforeEach`注册一个全局前置守卫。

**当一个路由发送改变时，全局前置守卫的回调方法便会执行。**

正因为这个特性，在改变之前便会进入这个回调方法，所以可以在这个方法中对**路由相关的参数进行修改**等，完成之后必须调用`next()方法`，才可以继续路由的工作。

```js
const router = new VueRouter({
  routes: [
    { path: "/login", component: One },
    { path: "/home", component: Two },
  ],
});
//设置全局前置守卫
router.beforeEach(function (to, from, next) {
	//路由变化逻辑
  if (to.path == "/login") next();
  next();
});
```

全局前置守卫的回调方法有三个参数：

- to：表示即将进入的目标路由对象。
- from：表示当前导航正要离开的路由对象。
- next：只有执行这个方法才能执行下一步，执行效果取决于`next()方法`的调用参数：
  - next()：可以执行下一步。
  - next(false)：中断当前的导航。
  - next('其他路由路径')：跳转到其他路由页面。
  - next(error)：如果传入next的参数是一个`Error`实例对象，则导航会被终止且该错误会被传递给`router.onError()`注册过的回调方法(或回调函数)。

==**确保在任何情况下都要调用next方法，否则守卫方法就不会被完成，而是一直处于等待状态。**==

简单的全局前置守卫示例：

```html
<div id="app">
  <router-link to="/login">登录</router-link>
  <router-view></router-view>
</div>
```

```js
//路由组件
const One = {
  template: `<div>
<h1>登录页</h1>
用户名：<input type="text" v-model="user"><br />
密码：<input type="text"  v-model="passwd">
<button @click="toHome()">登录</button>
</div>`,
  data() {
    return {
      user: "",
      passwd: "",
    };
  },
  methods: {
    toHome() {
      this.$router.push({ path: "/home" });
    },
  },
};
const Two = {
  template: `<div>
<h1>home组件页面</h1>  
</div>`,
};

//配置路由规则
const router = new VueRouter({
  routes: [
    { path: "/login", component: One },
    { path: "/home", component: Two },
  ],
});
//设置全局前置守卫
router.beforeEach(function (to, from, next) {
  if (to.path == "/login") {
    next();
  } else {
    // 停止跳转
    next(false);
    // 跳转到下一步
    // next({ name: 'login' }) 或者 使用路径  next('/login')
  }
});

const vm = new Vue({
  el: "#app",
  router,
});
```

结果图：

![image-20210609162441747](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005820.png)

![image-20210609162605302](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005815.png)



### 9.2 全局解析守卫

全局解析守卫的用法和全局前置守卫一样，都是路由导航被确认之前。

**区别：所有组件内的守卫和异步路由组件被解析之后被调用。**

```js
...
router.beforeResolve(function (to, from, next) {
  console.log('全局解析守卫')
  next()
})
```



### 9.3 全局后置钩子

全局后置钩子不会接受`next函数`也不会改变导航本身。

改钩子是在导航被确认之后调用。

```js
const router = new VueRouter({
  routes: [
    { path: '/user', component: User },
    { path: '/page', component: Page },
  ],
})
router.afterEach(function (to, from) {
  console.log(to)
  console.log('导航后置钩子')
})
```



### 9.4 组件内的守卫

组件内的守卫有三个：

- `beforeRouteEnter`
- `beforeRouteUpdate`
- `beforeRouteLeave`

```js
const user = {
  name: 'user',
  template: `<div><h1>路由规则名称：{{$route.name}}</h1></div>`,
  //组件前置守卫
  beforeRouteEnter: function (to, from, next) {
    // 渲染该组件对于的路由被confirm前调用，所以此时不能获取该组件实例的this，因为组件实例还没有被调用
    console.warn('组件前置守卫-beforeRouterEnter')
    next()
  },
  // 组件更新守卫
  beforeRouteUpdate: function (to, from, next) {
    // 此守卫是在组件复用的时候被调用,例如:动态参数的路径：/user/:id,在/user/1和/user/2之间跳转时能触发调用
    // 可以获取该组件实例this
    console.warn('组件更新守卫-beforeRouterUpdate')
    console.log(this)
    next()
  },
  beforeRouteLeave: function (to, from, next) {
    // 导航离开该组件对于的路由时调用
    // 可以获取该组件实例this
    console.warn('组件离开钩子-beforeRouterUpdate')
    console.log(this)
    next()
  },
}
```



### 9.5 路由配置守卫(常用)

路由配置守卫触发时间要早于`beforeRouteEnter`触发时间。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      components: User,
      beforeEnter: function (to, from, next) {
        console.log('router beforeEnter')
        next()
      },
    },
  ],
})
```



### 9.6 总结所有守卫和钩子函数触发流程

- 导航被触发。
- 在失活的组件中调用离开守卫。
- 调用全局的`beforeEach守卫`。
- 在复用的组件中调用`beforeRouteUpdate守卫`。
- 在路由配置中调用`beforeEnter`。
- 解析异步路由组件。
- 在被激活的组件中调用`beforeRouteEnter守卫`。
- 调用全局的`beforeResolve守卫`。
- 导航被确认。
- 调用全局的`afterEach钩子`。
- 触发DOM更新。
- 用创建好的实例调用`beforeRouteEnter守卫`中传给next的回调函数。



## 十、Vue Router的路由模式

### 10.1 哈希模式

之前我们使用的`Vue Router`相关方法和API都是基于**哈希模式（Vue Router默认采用哈希模式——hash模式）。**这也就是说每次进行导航和路由切换时，在浏览器的`URL`上都可以看到对应的 `哈希变化`。

`哈希模式`的特征：**URL的改变不会导致浏览器刷新或者跳转。**而这个特征正好满足我们单页面应用的需求。

如果不想使用`哈希模式`，可以用路由的 `history模式`，这个模式充分利用了`history.pushState`这个API来完成URL跳转而无须重新加载页面。



### 10.2 哈希模式和history模式的比较

**hash 模式：**

- 哈希值是指在URL中 `“#”符号后面的部分`，例如 http://localhost/index.html#/user，其中 `"/user"`这部分叫作**哈希值**。
- 当该值发送变化并不会使浏览器向服务器发送请求，如果浏览器不发送请求，页面也不会刷新。
- **哈希值**的变化可以采用浏览器原生提供的 `hashchange`事件来监听。
- Vue Router中的哈希模式就是**不断地修改哈希值来监听和记录页面的路径**。

**history 模式：**

- history模式是基于HTML5的 `History Interface接口`中新增的 `pushState()`和`replaceState()`两个API来实现的，通过这两个API可以改变浏览器URL地址且不会发送刷新浏览器的请求。
- 优点： 使URL更加简洁，更接近于真实的URL，即不会产生`#hash值`，例如 http://localhost/index.html/user。

- 缺点：浏览器刷新之后，history就会失效，转而立刻去请求真实的URL地址，对于纯前端来说，会丢失一些数据。



### 10.3 history模式的启用

如果需要启动history模式，则必须使用静态服务来访问，不能直接双击文件访问，也可以通过配置base属性来设置应用的基路径。

```js
# history模式的启用
const router=new VueRouter({
  mode:'history'，  //设置mode项
  routes:[....]
})
```



## 十一、keep-alive

### 11.1 keep-alive 缓存状态

`keep-alive`，标签为`<keep-alive><keep-alive/>`，是Vue内置的一个组件，可以使**被包含的组件保留状态或避免重新渲染。**

>keep-alive是一个抽象组件：
>
>- **它自身不会渲染一个DOM元素，也不会出现在父组件链中；**
>
>- **使用keep-alive包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。**

当`keep-alive`应用在`<route-view>`上时，**导航的切换会保留切换之前的状态**。

完整代码示例：

```html
<div id="app">
  <h1>
    <router-link to="/page">page</router-link>
    <router-link to="/user">user</router-link>
  </h1>
  <h2>
    <!-- 添加keep-alive标签使得出现缓存 -->
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </h2>
</div>
```

```js
// 注册Page组件
// type为range即进度条
const Page = {
  template: `<div><input type="range" /></div>`,
}
// 注册user组件
const User = {
  template: `<div><input type="text" /></div>`,
}

const router = new VueRouter({
  routes: [
    { path: '/page', component: Page },
    { path: '/user', component: User },
  ],
})
const vm = new Vue({
  el: '#app',
  router,
})
```

试验结果：

![image-20210728212948348](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005805.png)

文本输入框和滑动选择器不会随着路由导航的切换而重新渲染，而是会把状态都保存下来。



### 11.2 keep-alive的include/exclude设置项

**`<router-view>`是一个组件**，如果直接被包含在`<keep-alive>`标签里面，那么所有路径匹配到的视图组件都会被缓存。

但是有时我们只需要某个或者某几个路径的路由进行缓存，这时可以通过 `include/exclude`设置。

完整代码示例：

```html
<div id="app">
  <h1>
    <router-link to="/page">page</router-link>
    <router-link to="/user">user</router-link>
  </h1>
  <h2>
    <!-- 添加keep-alive标签使得出现缓存 -->
    <keep-alive :include="['page']">
      <router-view></router-view>
    </keep-alive>
  </h2>
</div>
```

```js
// 注册Page组件
// type为range即进度条
const Page = {
  name: 'page',
  template: `<div><input type="range" /></div>`,
}
// 注册user组件
const User = {
  name: 'user',
  template: `<div><input type="text" /></div>`,
}

const router = new VueRouter({
  routes: [
    { path: '/page', component: Page },
    { path: '/user', component: User },
  ],
})
const vm = new Vue({
  el: '#app',
  router,
})
```

- `include/exclude`可以设置单个字符串或正则表达式，也可以是一个由字符串或正则表达式组成的数组。
- 匹配的内容是 `组件的名称name`，`include`表示需要缓存的组件，`exclude`表示不需要缓存的组件。

**注意：**

>**（防坑）这里需要的是`组件的name属性`，不是在设置路由信息中的命名路由name。**



### 11.3 meta数据设置keep-alive

有时不想通过 `name属性` 来设置组件的缓存(==例如在某些应用场合，无法提前得知组件的名称==)，此时可以利用 `元数据meta`来设置是否需要缓存。

完整代码示例：

```html
<div id="app">
  <!-- 路由导航盒子 -->
  <div>
    <h1>
      <router-link to="/user">user</router-link>
      <router-link to="/page">page</router-link>
    </h1>
  </div>
  <!-- 这里是被缓存的视图组件,比如User组件 -->
  <keep-alive>
    <router-view v-if="$route.meta.keepAlive"></router-view>
  </keep-alive>
  <!-- 这里是不被缓存的视图组件，比如Page组件 -->
  <!-- <router-view v-if="$route.meta.keepAlive"></router-view> -->
</div>
```

```js
// 注册User组件
const User = {
  name: 'user',
  template: `<div>
<h1>这是User组件</h1>
<div><input type="range" /></div>
</div>`,
}
// 注册Page组件
const Page = {
  name: 'page',
  template: `<div>
<h1>这是Page组件</h1>
<div><input type="text" /></div>
</div>`,
}
const router = new VueRouter({
  routes: [
    { path: '/user', component: User, meta: { keepAlive: true } },
    { path: '/page', component: Page, meta: { keepAlive: false } },
  ],
})
const vm = new Vue({
  el: '#app',
  router,
})
```

结果：

![image-20210729202641881](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005757.png)

![image-20210729202847193](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005753.png)

**总结：**

> - 当把`<keep-alive>标签`应用在`<router-view>`上并在路由进行切换时，组件是不会被销毁的。
> - 除了第一次渲染组件，其它时候组件的生命周期钩子函数都不会再触发（例如`created`、`mounted`）。
> - 如果没使用 `keep-alive`进行缓存，那么组件就会随路由的切换而进行销毁、创建，生命周期钩子函数也会重新执行。
>
> 代码示例验证：
>
> ```js
> const User = {
>   name: 'user',
>   template: `<div>
> <h1>这是User组件</h1>
> <div><input type="range" /></div>
> </div>`,
>   created() {
>     console.log('created')
>   },
>   mounted() {
>     console.log('mounted')
>   },
> }
> ```
>
> 结果：
>
> ![image-20210729204753969](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005749.png)



### 11.4 activated和deactivated生命周期方法的使用

在组件生命周期方法中，有两个特殊的方法：`activated` 和 `deactivated`。

**这两个特殊的生命周期方法触发时机要结合`vue-router`和 `keep-alive标签`来使用。**

1. `activated`生命周期函数表示当 `vue-router` 的页面被打开时，会触发这个钩子函数。
2. `deactivated`生命周期函数表示当 `vue-router` 的页面被关闭时，会触发这个钩子函数。

>注意：这里使用这两个钩子函数主要是==监测keep-alive缓存的使用==。

代码示例：

```html
<div id="app">
  <!-- 路由匹配 -->
  <h1>
    <router-link to="/user">User</router-link>
    <router-link to="/page">Page</router-link>
  </h1>
  <!-- 路由出口 ,并且设置缓存-->
  //要想activated和deactivated这两个生命周期方法能触发，要搭配keep-aliva
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</div>
```

```js
// 注册User组件
const User = {
  name: 'user',
  template: `<div><input type="range" /></div>`,
  // activated和deactivated两个生命周期方法。
  activated() {
    console.log('User组件中的-activated')
  },
  deactivated() {
    console.log('User组件中的-deactivaed')
  },
}
// 注册Page组件
const Page = {
  name: 'page',
  template: `<div><input type="text /"></div>`,
  activated() {
    console.log('Page组件中的-activated')
  },
  deactivated() {
    console.log('Page组件中的-deactivaed')
  },
}
```

结果：

![image-20210729230043287](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005742.png)



### 11.5 验证keep-alive是否缓存的方法总结

1. 使用组件的生命周期方法。
2. 使用组件内的守卫方法 `beforeRouteEnter`和 `beforeRouteLeave`。==（注意：路由切换时需要两个不同的组件才可以使用。）==



### 11.6 keep-alive 实现原理浅析

总结：`<keep-alive>组件也是一个Vue组件`，它的实现是通过自定义的`render方法`并且使用了`插槽`。由于是直接使用`VNode（即虚拟DOM）`方式进行内容替换，不是直接存储DOM结构，所以不会执行组件内的生命周期方法，它通过`include`和`exclude`维护组件的`cache对象（缓存对象）`,从而来处理缓存中的具体逻辑。

![img](https:////upload-images.jianshu.io/upload_images/4191769-88a821c34ada7669.png?imageMogr2/auto-orient/strip|imageView2/2/w/878/format/webp)



## 十二、路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

**步骤：**

- 如果是在webpack脚手架开发时，需要先安装`@babel/plugin-syntax-dynamic-import`这个插件，这样才能使 Babel 可以正确地解析语法。

- 安装完之后在`babel.config.js`文件中添加插件：

  ![image-20210720004055953](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005714.png)

- 使用步骤只需要==使用一个特殊的注释语法==来提供 `chunk name`。

  示例：

  ```js
  const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
  const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
  const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
  ```

  Webpack 会将任何一个`异步模块(异步组件)`与`相同的块名称（chunkName）`组合到`相同的异步块(chunk)`中。

- 再利用**==路由懒加载==**，当路由被访问的时候才加载对应组件。这样能提高加载速度，减少不必要的加载时间。

  ![image-20210714014926938](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117005622.png)

  结合这两者，定义一个能够被 Webpack 自动代码分割的异步组件。

  项目案例示例：

  ```js
  const login = () => import(/* webpackChunkName: "login_home" */ '../components/login.vue')
  const home = () => import(/* webpackChunkName: "login_home" */ '../components/home.vue')
  
  const users = () => import(/* webpackChunkName: "users_rights_roles" */ '../components/users/users.vue')
  const rights = () => import(/* webpackChunkName: "users_rights_roles" */ '../components/power/rights.vue')
  const roles = () => import(/* webpackChunkName: "users_rights_roles" */ '../components/power/role/roles.vue')
  
  const categories = () => import(/* webpackChunkName: "categories_params" */ '../components/goods/categories/categories.vue')
  const params = () => import(/* webpackChunkName: "categoreis_params" */ '../components/goods/params/params.vue')
  
  const goods = () => import(/* webpackChunkName: "goods_goodsAdd" */ '../components/goods/list/goods.vue')
  const goodsAdd = () => import(/* webpackChunkName: "goods_goodsAdd" */ '../components/goods/list/goodsAdd/goodsAdd.vue')
  
  const order = () => import(/* webpackChunkName: "order_reports" */ '../components/order/orders.vue')
  const reports = () => import(/* webpackChunkName: "order_reports" */ '../components/report/reports.vue')
  ```

  ​	