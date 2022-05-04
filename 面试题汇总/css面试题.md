# css面试题

## 一、什么是标签语义化？

答：标签语义化就是：合理的标签做合理的事情。

**1.1追问：都有哪些标签，而且那些标签都是啥意思？**

答：标签有：块级标签，行内块标签和行内标签。

- 块级标签有：div、p、h1-h5、ul、li等。
- 行内块标签有：img、input、td标签等。
- 行内标签有：span、a、i、em标签等。

**1.2追问：那块级标签和行内标签的区别？**

答：

- 块级标签默认独占一行，宽度的大小跟随父元素容器的大小，而且还可以自定义宽高、内边距和外边距。
- 行内标签的宽度默认是本身内容的宽度，而且在一行上可以显示多个。它不可以自定义宽高。

**1.3追问：那块级标签、行内标签和行内块标签之间如何转换？**

答：通过display进行转换。示例如下：

```css
/*转换为块级标签元素*/
display:block;

/*转换为行内块标签元素*/
display:inline-block;

/*转换为行内标签元素*/
display:inline;
```

**1.4追问：display除了这几个值还有哪些？**

答：dsiplay还有：flex、table、none、grid。其中转换为flex即将该盒子变成弹性盒子；转换为table即将布局变成表格布局、转换为none即将元素隐藏起来。

**1.5追问：对于display:none，请问还有哪些让元素隐藏的？**

- 一：visibility:hidden。
- 二：opacity:0。

**1.6追问：display:none和visibility:hidden的区别？**

答：对于display:none,它会让元素隐藏，隐藏的元素不占据原来的位置；而visibility:hidden也是让元素隐藏，但是隐藏的元素会占据原理的位置。



## 二、盒子模型有哪些？

答：盒子模型有：1.标准盒子模型。2.怪异盒子模型。3.弹性盒子模型。

- 由于标准盒子模型的width是等于内容content的大小，所以标准盒子模型的大小=(width+padding+border)+margin。
- 由于怪异盒子模型的width是等于(content+border+padding)，所以怪异盒子模型的大小=(width)+margin。
- 弹性盒子模型就是flex布局下的盒子模型。

在真实项目中，用的最多的盒子模型就是IE怪异盒子模型，包括现在很多开源的UI库，它们的底层源码里面的公共样式很多都是怪异盒子模型(box-sizing:border-box。)



## 三、水平垂直居中的方法有哪些？

答：水平居中的方法我目前知道的有3种。第一种是使用flex布局。第二种是定位+位移。第三种是利用行内元素的特点进行水平垂直居中。

```css
/*flex布局实现*/
.box {
  display:flex;
  justify-content:center;
  align-items:center;
}

/*等位+位移*/
.box .smallbox{
  position:absolute;
  top:50%;
  left:50%;
  transform: translate(-50%, -50%); 
}

/*利用行内元素的特点*/
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



## 四、flex相关问题

**4.1 flex：1代表什么？**

答：flex：1代表flex：1 1 0%。即支持项目的放大比例和缩小比例。

​		
