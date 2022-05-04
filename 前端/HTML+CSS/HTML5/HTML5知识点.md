# HTML5知识点

## 一、HTML5语义化标签和属性

​		标签语义化，简单来说就是合理的标签做合理的事情。

​		例如标题用 `<hx>` 标签（`<h1>`，`<h2>` 等），列表用 `<ul>` 标签，这样我们从网页中查看每行源代码的时候，就能立马知道所要展示的内容是什么。

### 1.1 header标签

`<header>` 标签，作为HTML5引入的新标签之一，如果翻译成中文，可以理解成头部内容，或者是页眉内容。顾名思义，我们可以将网页最开始的部分内容放在 `<header>` 标签里面来显示。

示例：

```html
<header>
  <h1>I am header</h1>
  <p>header content</p>
</header>
```

`<header>` 标签在浏览器默认样式上和 `<div>` 是一致的，都属于**区块级元素**，只是在语义上有所区别，所以在规范上来说：

- `<header>` 标签应该作为一个容器，负责HTML页面顶部内容的显示，它可以有很很多子元素。
- 在一个HTML页面中，某些业务逻辑情况下，可以定义多个 `<header>` 元素，数量不受限制。
- 尽量不要把 `<header>` 标签放在 `<footer>` 标签中或者另一个 `<header>` 标签内部。



### 1.2 footer标签

同 `<header>` 标签对应的是 `<footer>` 标签。作为HTML5引入的新标签之一，如果翻译成中文，可以理解成底部内容，或者是页脚内容。顾名思义，我们可以将网页结尾的部分内容放在 `<footer>` 标签里面来显示。

示例：

```html
<footer>
  <p>Post by:移动web开发实践</p>
  <p>Contact information:<a href="mailto:someone@example.com">someone@example.com</a>.</p>
</footer>
```

`<footer>` 标签在样式上和 `<div>` 是一致的，都属于**区块级元素**，只是在语义上有所区别，此时需要注意语义化和规范：

- 根据语义化的规范，`<footer>` 元素大多数包括多个子元素，有网站的所有者信息，备案信息、姓名、文件的创建日期以及联系信息等。
- 在一个HTML页面中，某些业务逻辑情况下，可以定义多个 `<footer>` 元素，并且在每个 `<section>` 标签中都可以有一个 `<footer>` 标签，这一点不受限制。
- 尽量不要把 `<footer>` 标签放在 `<header>` 标签中或者另一个 `<footer>` 元素内部。

**注意：`<footer>` 标签通常是常驻在页面底部，与之搭配的css样式可以采用fixed定位。**

```html
<div class="wrapper">
  <footer>
    <p>name: threey</p>
    <p>Contact information</p>
  </footer>
</div>
```

```css
footer {
  position:fixed;
  bottom:0;
  width:100%;
  height:100px;
  background-color:#ffc0cb;
}
```

将网站的所有者信息、备案信息等放在 `<footer>` 中并置于页面底部，这是很多网站的标配。



### 1.3 section标签

​		`<section>` 标签是HTML5引入的另一个语义化标签，作用是对页面上的内容进行分块，这里的分块，主要是指按照功能来分，比如一个新闻消息的列表暂时页，诸如国际板块。娱乐板块、体育板块等等。每一个板块都可以使用 `<section>` 来划分，而每个板块都需要有自己的标题和内容，并且相对独立。

如下图所示的这个场景的页面：

![image-20220504014003219](https://cdn.jsdelivr.net/gh/threey333/Picture/HTML5/image-20220504014003219.png)

`<section>` 标签也没有特殊的样式，使用起来和 `<div>` 标签是一样的，示例代码如下：

```html
<section>
  <h3>Title</h3>
  <p>section info</p>
  <img src="test.png">
</section>
```

**总结一下 `<section>` 标签的使用场景和规范：**

- 使用 `<section>` 标签时，里面的内容一般要搭配标题和正文等，例如 `<h1>` ~ `<h6>` 或者 `<p>` 标签。
- 每个 `<section>` 标签都是一个独立的模块，这些独立模块内部不应该再嵌套 `<section>` 标签，但是多个 `<section>` 标签可以并列使用。
- 最后，`<section>` 标签不应当作为一个容器元素，它的语义化更强一些，当无法找到使用 `<section>` 标签的充分理由时，尽量不要使用。



### 1.4 nav标签

​		`<nav>` 标签是HTML5引入的另一个语义化标签，`<nav>` 标签用于表示HTML页面中的导航，可以是页面与页面之间的导航，也可以是页内 段与段之间的导航。`<nav>` 所代表的导航一般是位于页面顶部的横向导航，或者是面包屑导航。

如下图所示：

![image-20220504015333965](https://cdn.jsdelivr.net/gh/threey333/Picture/HTML5/image-20220504015333965.png)







### 1.5 aside标签







### 1.6 语义化标签总结



## 二、HTML5音频和视频

