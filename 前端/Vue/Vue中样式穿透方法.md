# Vue中CSS Scope简述

## 一、简介

### 1.1 Vue中的CSS Scope是什么

在 vue 文件中的 style 标签上，有一个特殊的属性：`scoped` 。当一个 style 标签拥有 `scoped 属性`时，它的 CSS 样式就只能作用于当前的组件，也就是说，该样式只能适用于当前组件元素。

==通过该属性，可以使得组件之间的样式不互相污染==。如果一个项目中的所有 style 标签全部加上了 `scoped` ，相当于实现了样式的模块化。

示意图：

![image-20210713214722738](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004108.png)



### 1.2 实现原理

采用 ==**PostCSS**== 转义实现。

PostCSS 给一个组件中的所有dom添加了一个**独一无二的动态属性**，然后，给 CSS 选择器额外添加一个对应的`属性选择器`来选择该组件中 dom ，这种做法使得样式只作用于含有该属性的 dom ——组件内部 dom。

>简单来说就是：
>
>- **添加scoped属性之后，该组件内的dom节点都添加了`html5`的自定义属性（`data-v-唯一值`）,该组件内的每一个dom节点对应一个唯一值。**
>
>  ![image-20210713220258352](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004118.png)
>
>- **有了scoped属性的该组件会默认在该组件内使用`属性选择器`来选择该组件中的dom节点**
>
>  ![image-20210713220711742](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117004128.png)

代码总结：

```html
<p class="example" data-v-5558831a>hi</p>
 
.example[data-v-5558831a] {}
```



## 二、Scoped的不良反应及解决方法

### 2.1 Scoped的不良反应

- `html`、`body` 不属于当前组件内的东西,所以设置样式没有效果。
- 动态添加的内容不起效果的 ( `innerHTML`、 `v-html` )

### 2.2 解决方法

#### # 方法一

在该Vue组件文件**再新增一个`<style>`标签**，这个标签不加`scoped`。

==这个方法是个偷懒的方法，不推荐首先使用==

示例：

```css
<style>
.ql-container {
  height: 300px;
  background: red;
}

.red {
  color: red;
}
</style>
```

#### # 方法二

如果在`.less`文件中，则可以使用 `穿透 /deep/`

用法：

```txt
父元素 /deep/ 子元素
```

其中子元素就是你要设置样式的标签/组件。

#### # 方法三

如果不在`.less`文件中，则可以使用 `父元素 >>> 子元素 (不支持 less 支持 css scss)`

用法：

```txt
父元素 >>> 子元素 
```

