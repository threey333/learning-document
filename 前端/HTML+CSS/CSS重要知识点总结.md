# CSS重要知识点总结

## 一、水平垂直居中方法汇总

### 1.1 利用flex布局

利用flex布局可以很轻松实现物体水平垂直居中。

**css:**

```css
.box{
  display: flex;
  /* 定义主轴方向居中 */
  justify-content: center;
  /* 定义交叉轴方向居中 */
  align-items: center;
}
```

**html:**

```html
<div class="box">
  <div class="smallbox"></div>
</div>
```



### 1.2 利用定位+位移

**css:**

```css
.box {
  position: relative;
  margin: 100px auto;
  width: 600px;
  height: 600px;
  border: 2px solid black;
}
.box .smallbox {
  position: absolute;
  top: 50%;
  left: 50%; 
  /* 利用2D转换调整居中 */
  transform: translate(-50%, -50%); 
  width: 200px;
  height: 200px;
  background-color: aqua;
}
```

**html:**

```html
<div class="box">
  <div class="smallbox"></div>
</div>
```



### 1.3 利用vh视口窗口单位+位移

CSS中存在一个vh（视口高度）,也就相当于DOM中`document.body.clientHeight`或者`document.documentElement.clientHeight`的高度，1vh=1%，即1vh等于视口高度的1%。**使用vh单位就能实现水平垂直居中。**

**css:**

```css
.box{
  margin: 50vh auto;
  transform: translateY(-50%);
}
```

**html:**

```html
<div class="box"></div>
```



### 1.4 当父元素高度确定时可以用margin实现

当父元素高度确定时可以用 ` margin` 实现水平垂直居中。

**css:**

```css
.box {
  width: 600px;
  height: 600px;
  border: 2px solid black;
}
.box .smallbox {
  margin: 50% auto;
  transform: translateY(-50%);
  width: 200px;
  height: 200px;
  background-color: aqua;
}
```

**html:**

```html
<div class="box">
  <div class="smallbox"></div>
</div>
```



### 1.5 让块级元素转换成行内块元素,配合行内元素特点实现

我们对行内元素实现水平垂直居中用的最多的就是 `text-align:center` + `line-height:块级元素自身高度`。

那么我们思考一下，能不能利用这两个特性从而实现物体水平垂直居中呢？回答是可以的。

方法：

>- 先将要居中的元素又块级元素变为行内块元素，这样它即可以设置宽高内边距外边距，还可以享受行内元素的特性。
>- 然后在父级容器添加 `text-align:center` + `line-height:块级元素自身高度`这两个CSS属性。
>- 这样改物体就能实现水平垂直居中。

**html:**

```html
<div class="container">
  <div class="box"></div>
</div>
```

**css:**

```css
.container {
  width: 400px;
  height: 400px;
  background-color: lightpink;
  text-align: center;
  line-height: 400px;
}
.box {
  display: inline-block;
  width: 40px;
  height: 40px;
  background-color: skyblue;
}
```

结果示例：

![image-20210902001837043](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210902001838.png)



## 二、css中的attr函数

css中的 `attr()函数` 返回选择元素的属性值。这个css函数在之前用的很少，这里要记录一下。

**注意：`attr()` 目前支持的仅有伪元素的 `content`属性。** 

**html:**

```html
<a href="www.baidu.com">:百度连接</a>
<p data-title="World">Hello</p>
```

**css:**

```css
a::before {
  content: attr(href);
  color: red;
  font-size: 40px;
}
a {
  text-decoration: none;
}

p::after {
  content: attr(data-title);
  color: lightpink;
  font-size: 36px;
}
```

![image-20210830190411531](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210830190422.png)



## 三、css中的calc函数

### 3.1 calc函数的用法

用法：

- `calc函数` 的参数是一个表达式，表达式的值就是这个函数的结果。
- `calc函数` 中的表达式的运算符前后都要保留一个空格。比如width:calc(100% - 10px),而不是 width:calc(100%-10px)。
- 任何长度值都可以用calc函数进行计算。例如%、vw、vh、px、vmin、vmax、em、rem等单位。
- `calc函数` 支持加减乘除运算符。
- `calc函数` 支持嵌套。例如calc(100% / calc(2em * 5))。

用法示例：

```css
.box {
  width: 600px;
  height: 600px;
  border: 2px solid #ddd;
  position: absolute;
  /* 你会发现使用calc函数会省去margin的调整，从而实现居中。 */
  top: calc(50% - 300px);
  left: calc(50% - 300px);
}

.bigbox {
  width: calc(100% / 3);
  height: 400px;
  background-color: #ddd;
}
```

```html
<div class="box">我喜欢吃烤鱼</div>
<div class="bigbox">hahaha</div>
```



## 四、BFC

`BFC` 全称：`Block Formatting Context`， 名为 "块级格式化上下文"。

### 4.1 理解BFC

**BFC会形成独立的渲染区域，内部的子元素不会对外界产生影响。**



### 4.2 产生BFC的条件

产生条件：

>1. float：left | right。
>
>2. position：absolute | fixed （非relative）。
>
>3. overflow：hidden | scroll | auto (非visible)。
>
>4. display：inline-block | table-cell | table-caption | flex（弹性布局） | grid（栅格布局） 
>
>    即非 none | block | inline。



### 4.3 BFC解决的问题

#### 4.3.1 外边距合并

外边距margin重合的解决方法：让父元素变为BFC，即父元素内部的子元素的渲染就不会对外界产生影响。

**css:**

```css
.container {
  overflow: hidden;
}
.box1 {
  margin-bottom: 20px;
  width: 200px;
  height: 200px;
  background-color: #ddd;
}

.box2 {
  margin-top: 20px;
  width: 200px;
  height: 200px;
  background-color: lightcoral;
}
```

**html:**

```html
<div class="container">
  <div class="box1"></div>
</div>
<div class="container">
  <div class="box2"></div>
</div>
```



#### 4.3.2 外边距塌陷

当元素有嵌套同时其中内部元素设置 `margin` 时会让整体发生改变。 这就是外边距塌陷。

解决思路：也是让父元素变成BFC，这样其子元素的渲染就不会对外界造成影响。

**css:**

```css
header {
  /* display: inline-block; */
  overflow: hidden;
  width: 400px;
  height: 400px;
  background-color: lightblue;
}

.box3 {
  margin-top: 100px;
  width: 200px;
  height: 200px;
  background-color: lightgreen;
}
```

**html:**

```html
<header>
  <div class="box3"></div>
</header>
```



#### 4.4 嵌套元素使用Float脱离文档流，高度塌陷

高度塌陷发生的情况在，父元素高度设置为随内容而自适应增加，但子元素设置浮动后脱离标准流，这样就会造成父元素高度塌陷。

**解决方式：也是让父元素设置为BFC。**

**css:**

```css
.height {
  display: inline-block;
  width: 400px;
  border: 2px solid black;
}

.box4 {
  float: left;
  width: 400px;
  height: 400px;
  background-color: lightyellow;
}
```

**html:**

```html
<div class="height">
  <div class="box4"></div>
</div>
```



## 五、块元素、行内块元素和行内元素之间的转换汇总

### 5.1 回顾块元素、行内块元素和行内元素的性质

**块元素：**h1~h5、p、div、ul、li、ol标签等。

>- 独占一行。
>- 高度、宽度、外边距、内边距都可以控制。
>- 宽度默认是父容器的100%。
>- 块元素内部可以放行内元素和块元素==（除了个别特例：比如P标签和h1~h6标签内部是不能放块级元素）==

**行内元素：**span、a、strong、i等。

>- 相邻行内元素在一行上，一行上可以显示多个。
>- 高度和宽度设置无效。
>- 默认宽度就是它本身内容的宽度。
>- **行内元素只能容纳文本或其他行内元素。**
>
>注意：像 `a` 标签这种行内元素，里面不能再放链接了。
>
>​			还有 `a` 标签元素内部可以放块级元素，但是最好给 `a` 标签转换为块级元素最安全。

**行内块元素：**img、input、td标签等。

>- 和相邻行内元素(行内块)在一行上，但是之间会有空白风险。一行可以显示多个(行内元素特点)
>- 默认宽度就是它本身内容的宽度。(行内元素特点)
>- 高度、宽度，行高，外边距以及内边距都可以控制。(块级元素特点)



### 5.2 转换方式

#### 5.2.1 显示转换

可以使用css中的 `display` 属性进行转换。

示例：

```css
display:block         //转换为块级元素
display:inline        //转换为行内元素
display:inline-block  //转换为行内块元素
```

#### 5.2.2 隐式转换

如果一个元素设置了 `浮动`、`绝对定位`、`固定定位`，那么它们会隐式转换为**行内块元素**。这时盒子能设置宽和高。

==**伪元素添加的内容属于行内元素。**==



## 六、CSS初始化代码

```css
/* em 和 i 斜体的文字不倾斜 */
em,
i {
  font-style: normal
}
/* 去掉li 的小圆点 */
li {
  list-style: none
}

img {
  /* border 0 照顾低版本浏览器 如果 图片外面包含了链接会有边框的问题 */
  border: 0;
  /* 取消图片底侧有空白缝隙的问题 */
  vertical-align: middle
}

button {
  /* 当我们鼠标经过button 按钮的时候，鼠标变成小手 */
  cursor: pointer
}

a {
  color: #666;
  text-decoration: none
}

a:hover {
  color: #c81623
}

button,
input {
  /* "\5B8B\4F53" 就是宋体的意思 这样浏览器兼容性比较好 */
  font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif
}

body {
  /* CSS3 抗锯齿形 让文字显示的更加清晰 */
  -webkit-font-smoothing: antialiased;
  background-color: #fff;
  font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB, "\5B8B\4F53", sans-serif;
  color: #666
}

.hide,
.none {
  display: none
}
/* 清除浮动 */
.clearfix:after {
  visibility: hidden;
  clear: both;
  display: block;
  content: ".";
  height: 0
}

.clearfix {
  *zoom: 1
}
```



## 七、CSS选择(包括css3新增加)

### 7.1 基础选择器

基础选择器有：`标签选择器`、`类选择器`、`id选择器`、`通配符选择器`。

案例演示：

```html
<!-- 基础选择器演示  -->
<div>这是一个标签选择器的盒子</div>
<div class="box">类选择器盒子</div>
<div id="wrapper">id选择器盒子</div>
```

```css
/* 通配符选择器 */
* {
  padding: 0;
  margin: 0;
}
/* 标签选择器 */
div {
  width: 200px;
  height: 200px;
  border: 1px solid #999;
  text-align: center;
  line-height: 200px;
}
/* 类选择器 */
.box {
  margin: 10px 0;
  width: 300px;
  height: 300px;
  border: 1px solid #000;
  line-height: 300px;
}
/* 标签选择器 */
#wrapper {
  width: 400px;
  height: 400px;
  line-height: 400px;
}
```



### 7.2 复合选择器

复合选择器有：`后代选择器`、`子选择器`、`并集选择器`、`伪类选择器`。

#### 7.2.1 后代选择器

后代选择器顾名思义就是选中某个父元素的子元素(或孙子、孙孙子)。

语法：

> 父级 子级{ 样式声明 }

案例演示：

```css
/* 后代选择器 */
article h1 {
  color: red;
}
article section h2 {
  color: blue;
}
```

```html
<article>
  <h1>css复习-选择器</h1>
  <h2>https://coco-music-threey333.vercel.app</h2>
  <h3>我是h3标签</h3>
  <section>
    <h2>threey</h2>
    <article>
      <section>哈哈哈，快毕业啦！！！</section>
    </article>
  </section>
</article>
```



#### 7.2.2 子选择器

使用  `>` 选择符分隔两个元素时,它只会匹配那些作为第一个元素的**直接后代**子元素。

语法：

>元素1 > 元素2 {样式声明 }

案例演示：

```css
/* 子选择器 */
span {
  background-color: white;
}
article > span {
  background-color: DodgerBlue;
}
```

```html
<article>
  <span>复习第一天
    <span> 复习第二天</span>
  </span>
  <span>复习第n天....</span>
</article>
```

结果：

![image-20211026005358914](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211026005419.png)

**从结果中你可以发现，子选择器它只会选中同一层级的第一个相同类型的子元素作为其父元素的子后代，其孙子元素、孙孙子元素都无效。**



#### 7.2.3 并集选择器

语法：

>元素1，元素2 { 样式声明 }

案例演示：

```css
article h1,
article h2 {
  background-color: lawngreen;
}
```

```html
<article>
  <h1>css复习-选择器</h1>
  <h2>https://coco-music-threey333.vercel.app</h2>
  <h3>我也是h3标签</h3>
  <h2>在线浏览地址</h2>
  <section>
    <h2>threey</h2>
    <article>
      <section>哈哈哈，快毕业啦！！！</section>
    </article>
  </section>
</article>
```

结果：article标签下的h1和h2标签背景颜色都变为了草绿色。

![image-20211026010417624](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211026010624.png)



#### 7.2.4 伪类选择器

CSS **伪类** 是添加到选择器的关键字，指定要选择的元素的特殊状态。

语法：

>选择的元素：伪类{ 属性值 }

案例演示：

```css
/* 伪类选择器 */
article h1:hover {
  background-color: lightblue;
  font-size: 18px;
  transition: all 1s;
}
```

```html
<article>
  <h1>css复习-选择器</h1>
  <h2>https://coco-music-threey333.vercel.app</h2>
  <h3>我也是h3标签</h3>
  <h2>在线浏览地址</h2>
  <section>
    <h2>threey</h2>
    <article>
      <section>哈哈哈，快毕业啦！！！</section>
    </article>
  </section>
</article>
```

结果：鼠标移动到该元素上，该元素就会慢慢变小，并且背景演示会编程海蓝色。

<article>
  <h1>css复习-选择器</h1>
</article>
<style>
  article h1 {
    padding:0 10px;
    color: red;
    background-color: lightgreen;
  }
  /* 伪类选择器 */
  article h1:hover {
    background-color: lightblue;
    font-size: 18px;
    transition: all 1s;
  }
</style>



### 7.3 兄弟选择器

语法：

>元素1~元素2 { 样式声明 }    注意：==元素1和元素2要同级。==

兄弟选择器在我理解中是分为 `通用兄弟选择器` 和 `铁兄弟选择器` 。

- 通用兄弟选择器：（以元素1为例）只要是和元素1是兄弟的同层级的元素2，那么元素2都会被 `通用兄弟选择器` 选中。
- 铁兄弟选择器：（以元素2为例）和元素1是兄弟的同层级的元素2并且是第一个关系的，那么该元素2会被 `铁兄弟选择器` 选中。

案例演示：

```css
/* 1.通用兄弟选择器 */
span ~ p {
  background-color: lightcoral;
}

/* 2.铁兄弟选择器 */
span + p {
  color: #fff;
  font-size: 20px;
  font-weight: 700;
}
```

```html
<span>这里是第一个span标签</span>
<p>这个段落中的字体颜色为pink色</p>
<h1>hello world</h1>
<p>这是第二个段落p</p>
<p>这是第三个段落p...</p>
```

结果：

![image-20211026012135920](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211026012142.png)



### 7.4 属性选择器

css `属性选择器` 通过已经存在的属性名或属性值匹配元素。

语法种类：

| 属性选择器用法种类 |                           解释说明                           |
| :----------------: | :----------------------------------------------------------: |
|       [attr]       |             表示含有以 `attr` 命名的属性的元素。             |
|    [attr=value]    |  表示含有以 `attr` 命名的属性，且属性值为 `value` 的元素。   |
|   [attr\|=value]   | 表示含有以 `attr` 命名的属性的元素，属性值为 `value` 或是以 `value-` 为前缀开头。示例：zh-CH，zh-TW. |
|   [attr~=value]    | 表示含有以 `attr` 命令的属性的元素，并且该属性以空格作为分隔的值，其中至少要有一个值是 `value`。示例：[title~="english"] |
|   [attr^=value]    | 表示含有以 `attr` 命名的属性，且属性值是以 `value` 开头的元素。 |
|   [attr$=value]    | 表示含有以 `attr` 命名的属性，且属性值是以 `value` 结尾的元素。 |
|   [attr*=value]    | 表示含有以 `attr` 命名的属性，且属性值至少包含一个 `value` 的元素。 |

案例演示：

```css
/* 存在title属性的<a> 元素 */
a[title] {
  color: purple;
}

/* 存在href属性并且属性值匹配"https://example.org"的<a> 元素 */
a[href="https://example.org"] {
  color: green;
}

/* 存在href属性并且属性值包含"example"的<a> 元素 */
a[href*="example"] {
  font-size: 2em;
}

/* 存在href属性并且属性值结尾是".org"的<a> 元素 */
a[href$=".org"] {
  font-style: italic;
}

/* 存在class属性并且属性值包含以空格分隔的"logo"的<a>元素 */
a[class~="logo"] {
  padding: 2px;
}
```



### 7.3 伪类选择器

伪类选择器这里我大致可以分为三种：

1. `子元素伪类选择器`：即一个父元素中有多个相同条件的子元素，这时可以使用子元素伪类选择器。
2. `类型子元素伪类选择器`：筛选出符合条件类型的元素，适用于一个父元素下有很多不同类型的子元素。
3. `元素状态伪类选择器`：匹配带状态的元素。

#### 7.3.1 子元素伪类选择器

子元素伪类选择器有如下几种使用：

- `first-child`：匹配父元素的第一个子元素。
- `last-child`：匹配父元素的倒数第一个子元素。
- `nth-child(n)`：匹配父元素的第n个子元素。
- `nth-last-child(n)`：匹配父元素的倒数第n个子元素。
- `only-child`：匹配父元素中仅有的一个子元素。

案例演示：

```html
<div>我是第1个</div>
<div>我是第2个</div>
<div>我是第3个</div>
<div>我是第4个</div>
<div>我是第5个</div>
<div>我是最后1个</div>
<div class="only">
  <span>我是唯一的</span>
</div>
```

```css
body div:nth-child(2) {
  text-indent: 10px;
  font-size: 40px;
  color: skyblue;
}

body div:first-child {
  color: pink;
  font-size: 30px;
  text-indent: 15px;
}

body div:last-child {
  color: red;
}

body div:nth-last-child(3) {
  font-size: 26px;
  color: blue;
}

body .only span:only-child {
  font-size: 36px;
  color: green;
}
```

结果图显示：

![image-20220112232452708](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220112232513.png)

#### 7.3.2 类型子元素伪类选择器

类型子元素伪类选择器有如下几种使用：

- `first-of-type`：匹配某父元素下的第一个"x"类型的子元素。
- `last-of-type`：匹配某父元素下的倒数第一个"x"类型的子元素。
- `nth-of-type(n)`：匹配某父元素下的第n个"x"类型的子元素。
- `nth-last-of-type(n)`：匹配某父元素下的倒数第n个"x"类型的子元素。
- `only-of-type`：匹配某父元素下的唯一一个"x"类型的子元素。

**==注意：上面的"x"可以是类名、标签名等等。==**

案例演示：

```html
<div class="div">我是第1个div类型的子元素</div>
<p>我是第1个p类型的子元素</p>
<div class="div">我是第2个div类型的子元素</div>
<p>我是第2个p类型的子元素</p>
<div class="div">我是第3个div类型的子元素</div>
<p>我是倒数第2个p类型的子元素</p>
<div class="div">我是最好一个div类型的子元素</div>
<p>我是最后1个p类型的子元素</p>
<div class="only">
  <span>我是唯一类型的子元素</span>
</div>
```

```css
body {
  font-size: 16px;
}

body .div:first-of-type {
  /* 匹配body父元素下的第一个div类型的子元素 */
  font-size: 20px;
  color: pink;
}

body p:first-of-type {
  /* 匹配body父元素下的第一个p类型的子元素 */
  font-size: 20px;
  color: skyblue;
  text-indent: 2em;
}

body p:last-of-type {
  /* 匹配body父元素下的最后一个p类型的子元素 */
  font-size: 28px;
  color: red;
}

body .div:nth-of-type(2) {
  /* 匹配body父元素下的第2个div类型的子元素 */
  font-size: 36px;
  color: yellowgreen;
  text-indent: 3em;
}

body p:nth-last-of-type(2) {
  /* 匹配body父元素下的倒数第2个p类型的子元素 */
  font-size: 32px;
  color: yellow;
  text-indent: 1.5em;
}

.only span:only-of-type {
  /* 匹配.only父元素下的唯一一个类型的子元素 */
  color: orange;
}
```

结果图显示：

![image-20220112233802308](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220112233805.png)

#### 7.3.3 元素状态伪类选择器

在CSS3之前，我们也用过元素状态伪类选择器，使用最多的就是hover伪类。在CSS3中新增了新的元素状态伪类选择器。

- `disable`：匹配表单元素E并且处于被禁用状态。
- `enabled`：匹配表单元素E并且处于启用状态。
- `checked`：匹配表单元素E并且处于被选中状态。
- `before`：在被选元素E的内容前面插入内容。(常用)
- `after`：在被选元素E的内容后面插入内容。(常用)

==**注意：before、after这两个伪类选择器在项目中是用的最多的伪类。**==

案例演示：

```html
<input type="text" disabled />
<input type="text" /><br />
<input type="radio" checked value="male" name="gander" />男<br />
<input type="radio" value="female" name="gender" />女<br />
```

```css
input[type='text']:disabled {
  /* 选中被禁止状态的表单元素 */
  background-color: chartreuse;
}
input[type='text']:enabled {
  background-color: blue;
}

input[type='radio']:checked {
  /* 当表单元素为radio并且是被选中的时候,宽高都会变为50px */
  width: 50px;
  height: 50px;
}
```

结果图显示：

![image-20220112234457631](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220112234459.png)

