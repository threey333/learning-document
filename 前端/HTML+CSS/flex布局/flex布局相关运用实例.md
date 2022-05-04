# flex布局相关运用实例

## 一、利用flex实现骰子各点数图

使用 `flex布局` 可以很轻易的布置骰子各点数图。

示例的公共样式部分已抽离出来。

```css
// 公共样式
body {
  display: flex;
}
body > div {
  margin-left: 10px;
}
.appearance-big-wrapper {
  width: 150px;
  height: 150px;
  background-color: #eee;
}

.item {
  border-radius: 50%;
  width: 50px;
  height: 50px;
  background-color: black;
}
```

好，准备工作已完成，下面就开始布局我们各骰子点数。

![image-20220325201237964](https://cdn.jsdelivr.net/gh/threey333/Picture/flex布局/20220325201238.png)



### 1.1 一点骰子

`HTML` 代码：

```html
<!-- 一点骰子 -->
<div class="first-wrapper appearance-big-wrapper">
  <div class="item"></div>
</div>
```

`css` 代码：

```css
.first-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}
```



### 1.2 二点骰子

`HTML` 代码：

```html
<!-- 二点骰子 -->
<div class="second-wrapper appearance-big-wrapper">
  <div class="item"></div>
  <div class="item"></div>
</div>
```

`css` 代码：

```css
/* 二点骰子 */
.second-wrapper {
  display: flex;
  justify-content: space-between;
}
.second-wrapper .item:nth-of-type(2) {
  align-self: flex-end;
}
```



### 1.3 三点骰子

`HTML` 代码：

```html
<!-- 三点骰子 -->
<div class="third-wrapper appearance-big-wrapper">
  <div class="item"></div>
  <div class="item"></div>
  <div class="item"></div>
</div>
```

`css` 代码：

```css
/* 三点骰子 */
.third-wrapper {
  display: flex;
  justify-content: center;
}
.third-wrapper .item:nth-of-type(2) {
  align-self: center;
}
.third-wrapper .item:nth-of-type(3) {
  align-self: end;
}
```



### 1.4 四点骰子

`HTML` 代码：

```html
<!-- 四点骰子 -->
<div class="four-wrapper appearance-big-wrapper">
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
  </div>
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
  </div>
</div>
```

`css` 代码：

```css
/* 四点骰子 */
.four-wrapper {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}
.four-wrapper .column {
  display: flex;
  justify-content: space-between;
  flex-basis: 100%;
}
```



### 1.5 五点骰子

`HTML` 代码：

```html
<!-- 五点骰子 -->
<div class="fifth-wrapper appearance-big-wrapper">
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
  </div>
  <div class="column">
    <div class="item"></div>
  </div>
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
  </div>
</div>
```

`css` 代码：

```css
/* 五点骰子 */
.fifth-wrapper {
  display: flex;
}
.fifth-wrapper .column {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.fifth-wrapper .column:nth-of-type(2) {
  justify-content: center;
}
```



### 1.6 六点骰子

`HTML` 代码：

```html
<!-- 六点骰子 -->
<div class="sixth-wrapper appearance-big-wrapper">
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
  </div>
  <div class="column">
    <div class="item"></div>
    <div class="item"></div>
    <div class="item"></div>
  </div>
</div>
```

`css` 代码：

```css
/* 六点骰子 */

/* 形式一 */
.sixth-wrapper {
  display: flex;
  justify-content: space-between;
}

/* 形式二 */
.sixth-wrapper {
  display: flex;
  flex-wrap: wrap;
}
.sixth-wrapper .column {
  display: flex;
}
.sixth-wrapper .column:nth-of-type(2) {
  align-items: flex-end;
}
```



## 二、利用flex实现栅栏布局

利于 `flex布局` 也是可以实现栅栏布局的效果。

`HTML` 代码：

```html
<div class="Grid">
  <!-- 第一行 -->
  <div class="first-wrapper">
    <div class="item">1/2</div>
    <div class="item">1/2</div>
  </div>

  <!-- 第二行 -->
  <div class="second-wrapper">
    <div class="item">1/3</div>
    <div class="item">1/3</div>
    <div class="item">1/3</div>
  </div>

  <!-- 第三行 -->
  <div class="third-wrapper">
    <div class="item">1/4</div>
    <div class="item">1/4</div>
    <div class="item">1/4</div>
    <div class="item">1/4</div>
  </div>

  <!-- 第四行 -->
  <div class="fourth-wrapper">
    <div class="item">
      <span>
        Lorem ipsum dolor sit amet, consecteturadipiscing elit. Vestibulum mollis velit nongravida venenatis. Praesent
        consequat lectuspurus, ut scelerisque velit condimentum eu.Maecenas sagittis ante ut turpis varius interdum.Quisque
        tellus ipsum, eleifend nonipsum id, suscipit ultricies neque.
      </span>
    </div>
    <div class="item">
      <span>Full-height, even when my content doesn't fillthe space. 全高度，即使我的内容没有填充空间。 </span>
    </div>
  </div>
</div>
```

 `css` 代码：

```css
.item {
  flex: 1;
  height: 40px;
  text-align: center;
  line-height: 40px;
  background-color: #eee;
}

.item:nth-of-type(n + 2) {
  margin-left: 20px;
}

/* 第一行 */
.Grid .first-wrapper {
  display: flex;
  margin-bottom: 20px;
}

/* 第二行 */
.Grid .second-wrapper {
  display: flex;
  margin-bottom: 20px;
}

/* 第三行 */
.Grid .third-wrapper {
  display: flex;
  margin-bottom: 20px;
}

/* 第四行 */
.Grid .fourth-wrapper {
  display: flex;
  margin-bottom: 20px;
}
.Grid .fourth-wrapper .item {
  height: 30vh;
}
```

结果图示：

![image-20220325204716376](https://cdn.jsdelivr.net/gh/threey333/Picture/flex布局/20220325204716.png)



## 三、利用flex实现百分比布局

`flex` 实现百分比布局，其实是利用了其 `flex-basis` 属性。

- `flex-basis` 属性的默认值为 `auto`，即项目元素本身的大小。
- 当它设置为百分比值，同时 `flex-grow` 属性为 `0`、`flex-shrink` 属性为 `0` 的时候，就可以实现百分比布局。

`html` 代码：

```html
<div class="wrapper">
  <!-- 第一行 -->
  <div class="first-row">
    <div class="item">1/2</div>
    <div class="item">auto</div>
    <div class="item">auto</div>
  </div>
  <!-- 第二行 -->
  <div class="second-row">
    <div class="item">auto</div>
    <div class="item">1/3</div>
  </div>
  <!-- 第三行 -->
  <div class="third-row">
    <div class="item">1/2</div>
    <div class="item">auto</div>
    <div class="item">auto</div>
  </div>
</div>
```

`css` 代码：

```css
body {
  padding-top: 50px;
  text-align: center;
}
.wrapper {
  height: 600px;
}
/* 第一行 */
.wrapper .first-row {
  display: flex;
}
.wrapper .first-row .item {
  height: 100px;
  background-color: #eee;
  line-height: 100px;
}
.wrapper .first-row .item:nth-of-type(1) {
  flex: 0 0 50%;
}
.wrapper .first-row .item:nth-of-type(n + 2) {
  margin-left: 20px;
  flex: 1;
}

/* 第二行 */
.wrapper .second-row {
  display: flex;
  margin-top: 20px;
}
.wrapper .second-row .item {
  height: 100px;
  background-color: #eee;
  line-height: 100px;
}

.wrapper .second-row .item:nth-of-type(1) {
  flex: 1;
}
.wrapper .second-row .item:nth-of-type(2) {
  flex: 0 0 33.333%;
  margin-left: 20px;
}

/* 第三行 */
.wrapper .third-row {
  display: flex;
  margin-top: 20px;
}
.wrapper .third-row .item {
  height: 100px;
  background-color: #eee;
  line-height: 100px;
}
.wrapper .third-row .item:nth-of-type(1) {
  flex: 0 0 25%;
}

.wrapper .third-row .item:nth-of-type(2) {
  flex: 1;
}

.wrapper .third-row .item:nth-of-type(3) {
  flex: 0 0 33.33%;
}
.wrapper .third-row .item:nth-of-type(n + 2) {
  margin-left: 20px;
}
```

结果图示：

![image-20220326195232868](https://cdn.jsdelivr.net/gh/threey333/Picture/flex布局/20220326195233.png)



## 四、利用flex实现圣杯布局

`圣杯布局` 就是左右两边定宽，中间自适应，能根据屏幕大小做自适应。

实现 `圣杯布局` 的方式有很多，其中采用 `flex` 实现是最简单也是最容易懂的。

>**实现思路：**左右侧边栏设置 `flex-basis` 为固定值，中间 `flex` 为 `1` 自适应。

`HTML` 代码：

```html
<div class="wrapper">
  <header>#header</header>
  <div class="wrapper-body">
    <main>内容content</main>
    <nav>导航标签</nav>
    <aside>侧边栏标签</aside>
  </div>
  <footer>#footer</footer>
</div>
```

 `css` 代码：

```css
body {
  padding: 0;
  margin: 0;
}
.wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
header,
footer {
  flex: 1;
  background-color: #666666;
}

.wrapper .wrapper-body {
  display: flex;
  flex: 1;
}
nav,
aside {
  flex: 0 0 12em;
  background-color: skyblue;
}

main {
  flex: 1;
  background-color: springgreen;
}

nav {
  order: -1;
}

aside {
  background-color: tan;
}
```

结果图示：

![image-20220326221313173](https://cdn.jsdelivr.net/gh/threey333/Picture/flex布局/20220326221313.png)



当然，如果是小屏幕，`wrapper-body` 部分自动变为垂直叠加。

```css
@media (max-width: 768px) {
  .wrapper-body {
    flex-direction: column;
    flex: 1;
  }
  nav,
  main,
  aside {
    flex: auto;
  }
}
```

结果图示：

![image-20220326221729804](https://cdn.jsdelivr.net/gh/threey333/Picture/flex布局/20220326221729.png)



## 五、利用flex实现固定的底栏

有时，页面内容太少，无法占满一屏的高度，底栏就会抬高到页面的中间。这时可以采用Flex布局，让底栏总是出现在页面的底部。

`HTML` 代码：

```html
<body class="Site">
  <header>...</header>
  <main class="Site-content">...</main>
  <footer>...</footer>
</body>
```

`css` 代码：

```css
.Site {
  display: flex;
  min-height: 100vh;
  flex-direction: column;
}

.Site-content {
  flex: 1;
}
```

