# 移动WEB开发

## 一、移动端基础

#### 1.1常见移动端屏幕尺寸

![image-20210314151516484](D:\Typora记录文档\img\image-20210314151516484.png)

#### 2.移动端手机调试方法

①Chrome DevTools（谷歌浏览器）的模拟手机调试

②搭建本地web服务器，手机和服务器一个局域网内，通过手机访问服务器

③使用外网服务器，直接IP或域名访问

#### 

## 二、视口

#### 什么是视口

**视口**就是浏览器显示页面内容的屏幕区域。视口可以分为布局视口、视觉视口和理想视口。

#### 2.1 布局视口layout viewport

①一般移动设备的浏览器都默认设置了一个布局视口，用于解决早期的PC端页面在手机上显示的问题。

②iOS, Android基本都将这个视口分辨率设置为 980px，所以PC上的网页大多都能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页。

#### 2.2 视觉视口visual viewport

①字面意思，它是用户正在看到的网站的区域。**注意：是网站的区域。**

②我们可以通过缩放去操作视觉视口，但不会影响布局视口，布局视口仍保持原来的宽度。

#### 2.3 理想视口ideal viewport

①为了使网站在移动端有最理想的浏览和阅读宽度而设定。

②理想视口，对设备来讲，是最理想的视口尺寸。

③需要手动添写meta视口标签通知浏览器操作。

④meta视口标签的主要目的：布局视口的宽度应该与理想视口的宽度一致，简单理解就是设备有多宽，我们布局的视口就多宽。

#### 2.4 总结

①视口就是浏览器显示页面内容的屏幕区域。

②视口分为布局视口、视觉视口、理想视口。

③移动端布局想要的是理想视口就是手机屏幕有多宽，我们的布局视口就有多宽。

④想要理想视口，我们需要给我们的移动端页面添加 **meta视口标签**

#### 2.5 meta视口标签

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

![image-20210314153704329](D:\Typora记录文档\img\image-20210314153704329.png)



加了meta视口标签在手机端是这样的

![image-20210314160601812](D:\Typora记录文档\img\image-20210314160601812.png)

如果不加meta视口标签则是这样的---看不清

![image-20210314160646457](D:\Typora记录文档\img\image-20210314160646457.png)

**结论：在手机移动端开发一定要加上meta视口标签**



#### 2.6 标准的viewport设置

①视口宽度和设备保持一致

②视口的默认缩放比例1.0

③不允许用户自行缩放

④最大允许的缩放比例1.0

⑤最小允许的缩放比例1.0



## 三、二倍图

#### 3.1 **物理像素&物理像素比**

①PC端 和 早前的手机屏幕 / 普通手机屏幕: 1CSS像素 = 1 物理像素的。

②Retina（视网膜屏幕）是一种显示技术，可以将把更多的物理像素点压缩至一块屏幕里，从而达到更高的分辨率，并提高屏幕显示的细腻程度。

![image-20210314154951923](D:\Typora记录文档\img\image-20210314154951923.png)

#### 3.2多倍图

①对于一张 50px*50px 的图片,在手机 Retina 屏中打开，按照刚才的物理像素比会放大倍数，这样会造成图片模糊。

②在标准的viewport设置中，使用倍图来提高图片质量，解决在高清设备中的模糊问题。（**比如如果插入一张50*50像素的图片可以先把它放大二倍或者多倍，然后在再手动变成我们所需要的像素）**

```
 /* 在 iphone8 下面 */
 img{
 /*原始图片100*100px*/
 width: 50px;
 height: 50px;
 } 
.box{
 /*原始图片100*100px*/
 background-size: 50px 50px;
 }
```

③通常使用二倍图， 因为iPhone 6\7\8 的影响,但是现在还存在3倍图4倍图的情况，这个看实际开发公司需求。

④背景图片 注意缩放问题。



#### 3.3 背景缩放 background-size

background-size 属性规定背景图像的尺寸。

```
background-size: 背景图片宽度 背景图片高度;
```

单位:长度|百分比|cover|contain。

cover把背景图像扩展至足够大，以使背景图像完全覆盖背景区域。

contain把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。(只要`宽或高`填满就不再扩展。)

#### 3.4 多倍图切图cutterman

![image-20210314162053872](D:\Typora记录文档\img\image-20210314162053872.png)



## 四、移动端开发选择

#### 4.1单独制作移动端页面（主流）

①流式布局

②flex弹性布局（强烈建议）。

③less+rem+媒体查询。

④混合布局。

#### 4.2 响应式页面兼容端（其次）

①媒体查询

②bootstarp



## 五、移动端技术解决方案

#### 5.1 移动端浏览器

①移动端浏览器基本以webkit内核为主，因此我们就考虑webkit兼容性问题。

②我们可以放心使用 H5 标签和 CSS3 样式。

③同时我们浏览器的私有前缀我们只需要考虑添加 webkit 即可。

![image-20210314163805669](D:\Typora记录文档\img\image-20210314163805669.png)



#### 5.1 CSS初始化 normalize.css

![image-20210314163430135](D:\Typora记录文档\img\image-20210314163430135.png)

移动端 CSS 初始化推荐使用 normalize.css/

Normalize.css：保护了有价值的默认值。

Normalize.css：修复了浏览器的bug。

Normalize.css：是模块化的。

Normalize.css：拥有详细的文档。



#### 5.3 CSS3盒子模型 box-sizing

①传统模式宽度计算：盒子的宽度 = CSS中设置的width + border + padding 。

②CSS3盒子模型： 盒子的宽度 = CSS中设置的宽度width 里面包含了 border 和 padding  也就是说，我们的CSS3中的盒子模型， padding 和 border 不会撑大盒子了。

```css
/*CSS3盒子模型*/
 box-sizing: border-box;
 /*传统盒子模型*/
 box-sizing: content-box;
```

问题？什么时候选择用传统盒子模型，什么时候选择CSS3盒子模型？

答：①移动端可以全部CSS3 盒子模型。

​		②PC端如果完全需要兼容，我们就用传统模式，如果不考虑兼容性，我们就选择 CSS3 盒子模型。

#### 5.4 特殊样式

```
 /*CSS3盒子模型*/
 box-sizing: border-box;
 -webkit-box-sizing: border-box;
 /*点击高亮我们需要清除清除 设置为transparent 完成透明*/
 -webkit-tap-highlight-color: transparent;
 /*在移动端浏览器默认的外观在iOS上加上这个属性才能给按钮和输入框自定义样式*/
 -webkit-appearance: none;
 /*禁用长按页面时的弹出菜单*/
 img,a { -webkit-touch-callout: none; }
```



## 六、flex布局

### 6.1 传统布局和flex布局

![image-20210315215315096](D:\Typora记录文档\img\image-20210315215315096.png)

### 6.2 布局原理

任何一个容器都考研指定为flex布局。

①当父盒子设为flex布局以后，子元素的`float`、`clear`和`vertical-align`属性将是是失效。

②伸缩布局 = 弹性布局 = 伸缩盒布局 = 弹性盒布局 =flex布局

### 6.3 基本概念

①采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。

②它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

![image-20210315221524861](D:\Typora记录文档\img\image-20210315221524861.png)



### 6.4 常见父项属性

①flex-direction：设置主轴的方向。

②justify-content：设置主轴上的子元素排列方式。

③flex-wrap：设置子元素是否换行。

④align-content：设置侧轴上的子元素排列方式（多行）。

⑤align-items：设置侧轴上的子元素排列方式（单行）。

⑥flex-flow：复合属性，相当于同时设置flex-direction和flex-wrap。

------



#### # flex-direction设置主轴方向

![image-20210315223145969](D:\Typora记录文档\img\image-20210315223145969.png)



flex-direction 属性决定主轴的方向（即项目的排列方向）

注意： 主轴和侧轴是会变化的，就看 flex-direction 设置谁为主轴，剩下的就是侧轴。而我们的子元素是跟着主轴来排列的

![image-20210315222628717](D:\Typora记录文档\img\image-20210315222628717.png)

依次对应示范图：

![image-20210315222843525](D:\Typora记录文档\img\image-20210315222843525.png)

------



#### # justify-content设置主轴上的子元素排列方式

**注意： 使用这个属性之前一定要确定好主轴是哪个！**

![image-20210315223224464](D:\Typora记录文档\img\image-20210315223224464.png)

各属性值示范图：

- flex-start(默认值)：与main start对齐

![image-20210315223416474](D:\Typora记录文档\img\image-20210315223416474.png)



- flex-end: 与main end对齐

![image-20210315223528761](D:\Typora记录文档\img\image-20210315223528761.png)



- center: 居中对齐

![image-20210315223628986](D:\Typora记录文档\img\image-20210315223628986.png)



- space-between: flex items之间距离相等，与main start、main end两端对齐.（两边贴边 再平分剩余空间）

![image-20210315223913817](D:\Typora记录文档\img\image-20210315223913817.png)



- space-around: flex items之间的距离相等，flex items与main start、main end之间的距离是flex items之间距离的一半（平分剩余空间）

![image-20210315224042579](D:\Typora记录文档\img\image-20210315224042579.png)

------



#### # flex-wrap设置子元素是否换行

flex-wrap:nowrap（默认值，不换行）

![image-20210315224350632](D:\Typora记录文档\img\image-20210315224350632.png)



flex-wrap:wrap （换行显示）

![image-20210315224446786](D:\Typora记录文档\img\image-20210315224446786.png)

#### # align-content设置侧轴上的子元素排列方式（多行）

设置子项在侧轴上的排列方式 并且只能用于子项出现 换行 的情况（多行），在**单行下是没有效果的。**

![image-20210315224729513](D:\Typora记录文档\img\image-20210315224729513.png)



- flex-start：与侧轴头部对齐

![image-20210315224911390](D:\Typora记录文档\img\image-20210315224911390.png)



- flex-end：与cross end对齐

  ![image-20210315225142560](D:\Typora记录文档\img\image-20210315225142560.png)



- center: 居中对齐

![image-20210315225228871](D:\Typora记录文档\img\image-20210315225228871.png)



- space-between: 子侧再侧轴先分布在两头，再平分剩余空间。

![image-20210315225536044](D:\Typora记录文档\img\image-20210315225536044.png)



- space-around: 子侧在侧轴平分剩余空间。

  ![image-20210315225656044](D:\Typora记录文档\img\image-20210315225656044.png)



- stretch：拉伸，设置子项元素高度平分父元素高度。（子项元素不设置高度前。）

#### # align-items设置侧轴上的子元素排列方式(单行)

该属性是控制子项在侧轴（默认是y轴）上的排列方式 在子项为单项（单行）的时候使用。

![image-20210315230113836](D:\Typora记录文档\img\image-20210315230113836.png)



- flex-start: 从上到下

  ![image-20210315230400022](D:\Typora记录文档\img\image-20210315230400022.png)



- flex-end: 从下到上

![image-20210315230447231](D:\Typora记录文档\img\image-20210315230447231.png)



- center: 居中对齐

  ![image-20210315230532843](D:\Typora记录文档\img\image-20210315230532843.png)



- stretch: 当flex items在cross axis方向的size为auto时，会自动拉伸至填充flex container，当未设置item1 item2 item3高度时，会自动拉伸填满。

  ![image-20210315230637258](D:\Typora记录文档\img\image-20210315230637258.png)



​			当设置item高度时，效果如下：

![image-20210315230717592](D:\Typora记录文档\img\image-20210315230717592.png)



------

#### #flex-flow复合属性，相当于同时设置flex-direction和flex-wrap。

flex-flow 属性是 flex-direction 和 flex-wrap 属性的复合属性。

```css
flex-flow:row wrap;
```



### 6.5 子项常用属性

①flex 子项目占的份数

②align-self 控制子项自己在侧轴的排列方式

③order属性定义子项的排列顺序（前后顺序）

------

①flex属性★

flex 属性定义子项目分配剩余空间，用flex来表示占多少份数。

```
.item {
 flex: <number>; /* default 0 */ }
```



② align-self 控制子项自己在侧轴上的排列方式

align-self 属性允许单个项目有与其他项目不一样的对齐方式，可覆盖 align-items 属性。

默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

```
span:nth-child(2) {
 /* 设置自己在侧轴上的排列方式 */
 align-self: flex-end;
 }
```



③**order** 属性定义项目的排列顺序

数值越小，排列越靠前，默认为0。

注意：和 z-index 不一样。

```
.item {
 order: <number>;
}
```

### 6.6 flex问题总结

**1.flex盒子不用担心外边距合并的问题。**



## 七、rem适配布局

#### 7.1rem基础

①rem单位em相对于父元素 的字体大小来说的。

②rem 相对于 html元素 字体大小来说的。

③rem的优点就是可以通过修改html里面的文字大小来改变页面中元素的大小可以整体控制。

比如：

```css
/* 根html 为 12px */
html {
 font-size: 12px;
}/* 此时 div 的字体大小就是 24px */ 
div {
 font-size: 2rem;
}
```



#### 7.2媒体查询

7.2.1什么是媒体查询？

![image-20210317134402091](D:\Typora记录文档\img\image-20210317134402091.png)

7.2.2 语法规范

```css
@media mediatype and|not|onlt (media feature) {
		CSS-Code;
}
```

①用@media开头 注意@符号

②mediatype媒体类型

③关键字and not only

④media feature 媒体特性 必须有小括号包含



**1.mediatype查询类型**

![image-20210317134844827](D:\Typora记录文档\img\image-20210317134844827.png)

**2.关键字**

关键字将媒体类型或多个媒体特性连接到一起做为媒体查询的条件。

![image-20210317134943669](D:\Typora记录文档\img\image-20210317134943669.png)

**3.媒体特性**

每种媒体类型都具体各自不同的特性，根据不同媒体类型的媒体特性设置不同的展示风格。我们暂且了解三个。

！！！注意他们要加小括号包含

![image-20210317135056530](D:\Typora记录文档\img\image-20210317135056530.png)

**4.媒体查询+rem实现元素动态大小变化**

①rem单位是跟着html来走的，有了rem页面元素可以设置不同大小尺寸

②媒体查询可以根据不同设备宽度来修改样式

③媒体查询+rem 就可以实现不同设备宽度，实现页面元素大小的动态变化

例如：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>rem适配</title>
    <style>
        * {
            padding: 0;
            margin: 0;
        }
        @media screen and (min-width: 320px) {
            html {
                font-size: 50px;
            }
        }
        @media screen and (min-width: 640px) {
            html {
                font-size: 100px;
            }
        }
        div {
            height: 1rem;
            background-color: green;
            line-height: 1rem;
            color: 0.5rem;
            color: #fff;
            text-align: center;
        }
    </style>
</head>
<body>
    <div>
        购物车
    </div>
</body>
</html>
```

**5. 引入资源**

当样式比较繁多的时候，我们可以针对不同的媒体使用不同 stylesheets（样式表）。

原理，就是直接在link中判断设备的尺寸，然后引用不同的css文件。

示例

```html
<link rel="stylesheet" href="styleA.css" media="screen and (min-width: 400px)">
```



#### 7.3Less基础

###### **7.3.1 css弊端**

CSS 是一门非程序式语言，没有变量、函数、SCOPE（作用域）等概念。

![image-20210317141207866](D:\Typora记录文档\img\image-20210317141207866.png)

###### **7.3.2 Less介绍**

①Less （Leaner Style Sheets 的缩写） 是一门 CSS 扩展语言，也成为CSS预处理器。

②做为 CSS 的一种形式的扩展，它并没有减少 CSS 的功能，而是在现有的 CSS 语法上，为CSS加入程序式语言的特性。

③它在 CSS 的语法基础之上，引入了**变量**，**Mixin（混入）**，**运算**以及**函数**等功能，大大简化了 CSS 的编写，并且降低了 CSS 的维护成本，就像它的名称所说的那样，Less 可以让我们用更少的代码做更多的事情。

![image-20210317141422695](D:\Typora记录文档\img\image-20210317141422695.png)



**Less中文网址： http://lesscss.cn/**



###### Less变量

变量是指没有固定的值，可以改变的。因为我们CSS中的一些颜色和数值等经常使用。

```less
@变量名:值
```

变量命名规范

①必须有@作为前缀。

②不能包含特殊字符

③不能以数字作为变量开头

④大写敏感。

示例：

```css
@BassFont: 50px;  //定义一个变量，变量名为@BassFont，大小为50px。
```

**变量使用规范**

```
//直接使用
body{
 color:@color;
}
a:hover{
 color:@color;
}
```

###### Less编译

在vscode中使用 Easy LESS插件。★

![image-20210317143854231](D:\Typora记录文档\img\image-20210317143854231.png)

Easy LESS插件用来把less文件编译为css文件。



###### Less嵌套

①less嵌套:子元素的样式直接写到父元素里面就行。

②如果有伪类、交集选择器、 伪元素选择器 我们内层选择器的前面需要加&。

示例

```less
.header {
    width: 200px;
    height: 200px;
    background-color: pink;
    // 1. less嵌套 子元素的样式直接写到父元素里面就好了
    a {
        color: red;
        // 2. 如果有伪类、交集选择器、 伪元素选择器 我们内层选择器的前面需要加&
        &:hover {
            color: blue;
        }
    }
}
.nav {
    	.logo {
        color: green;
    }
    &::before {
        content: "";
    }
}
```

less文件编译后的css文件代码

```css
.header {
  width: 200px;
  height: 200px;
  background-color: pink;
}
.header a {
  color: red;
}
.header a:hover {
  color: blue;
}
//
.nav .logo {
  color: green;
}
.nav::before {
  content: "";
}
```

###### Less运算 ★

任何数字、颜色或者变量都可以参与运算。就是Less提供了加（+）、减（-）、乘（*）、除（/）算术运算。 

```css
/*Less 里面写*/
@witdh: 10px + 5;
div {
 border: @witdh solid red;
}/*生成的css*/
div {
 border: 15px solid red;
}/*Less 甚至还可以这样 */
width: (@width + 5) * 2;
```

注意：

1.乘号（*）和除号（/）的写法。

2.运算符中间左右要有个空格隔开。比如1px + 5;

3.对于两个不同的单位的值之间的运算，运算结果的值取第一个值的单位。

4.如果两个值之间只有一个值有单位，则运算结果就取该单位。



#### 7.4 rem适配方案

###### rem适配方案

1.让一些不能等比自适应的元素，达到当设备尺寸发生改变的时候，等比例适配当前设备。

2.使用媒体查询根据不同设备按比例设置html的字体大小，然后页面元素使用rem做尺寸单位，当html字体大小变化元素尺寸也会发生变化，从而达到等比缩放的适配。

------

###### rem实际开发适配方案

①按照设计稿与设备宽度的比例，**动态计算并设置 html 根标签的 font-size 大小**；（媒体查询）

②CSS 中，设计稿元素的宽、高、相对位置等取值，按照同等比例换算为 rem 为单位的值；

###### rem适配方案技术使用

技术方案1：less+媒体查询+rem。

技术方案2：flexible.js+rem。

总结：1.两种方案现在都存在。

​			2.方案2 更简单



###### rem实际开发适配方案1

less+媒体查询+rem

**1.设计稿常见尺寸宽度**

![image-20210317145922630](D:\Typora记录文档\img\image-20210317145922630.png)

一般情况下，我们以一套或两套效果图适应大部分的屏幕，放弃极端屏或对其优雅降级，牺牲一些效果。

现在基本以750为准。



**2.动态设置 html 标签 font-size 大小**

① 假设设计稿是750px

② 假设我们把整个屏幕划分为15等份（划分标准不一可以是20份也可以是10等份）

③ 每一份作为html字体大小，这里就是50px

④ 那么在320px设备的时候，字体大小为320/15 就是 21.33px

⑤ 用我们页面元素的大小 除以不同的 html 字体大小会发现他们比例还是相同的

⑥ 比如我们以 750为标准设计稿

⑦ 一个100*100像素的页面元素 在 750屏幕下， 就是 100 / 50 转换为rem 是 2rem * 2 rem 比例是 1比1 

⑧ 320屏幕下， html 字体大小为 21.33 则 2rem = 42.66px 此时宽和高都是 42.66 但是 宽和高的比例还是 1比1 

⑨ 但是已经能实现不同屏幕下 页面元素盒子等比例缩放的效果



3.**元素大小取值方法**

① 最后的公式： 页面元素的rem值 = 页面元素值（px） / （屏幕宽度 / 划分的份数）

② 屏幕宽度/划分的份数 就是 html font-size 的大小

③ 或者： 页面元素的rem值 = 页面元素值（px） / html font-size 字体大小



###### rem实际开发适配方案2

**1.简洁高效的rem适配方案flexible.js**

手机淘宝团队出的简洁高效 移动端适配库

我们再也不需要在写不同屏幕的媒体查询，因为里面js做了处理

它的原理是把当前设备划分为10等份，但是不同设备下，比例还是一致的。

我们要做的，就是确定好我们当前设备的html 文字大小就可以了

比如当前设计稿是 750px， 那么我们只需要把 html 文字大小设置为 75px(750px / 10) 就可以

里面页面元素rem值： 页面元素的px 值 / 75 

剩余的，让flexible.js来去算



github地址：https://github.com/amfe/lib-flexible



**px转换rem插件------cssrem**

要注意的一点，这个插件要自己手动设置html字体大小(设置--搜索cssrem就可以设置.)





## 八、响应式布局

#### 1. 响应式开发

**1.响应式开发原理**

就是使用媒体查询针对不同宽度的设备进行布局和样式的设置，从而适配不同设备的目的。

```css
        /* 1. 超小屏幕下  小于 768  布局容器的宽度为 100% */
        
        @media screen and (max-width: 767px) {
            .container {
                width: 100%;
            }
        }
        /* 2. 小屏幕下  大于等于768  布局容器改为 750px */
        
        @media screen and (min-width: 768px) {
            .container {
                width: 750px;
            }
        }
        /* 3. 中等屏幕下 大于等于 992px   布局容器修改为 970px */
        
        @media screen and (min-width: 992px) {
            .container {
                width: 970px;
            }
        }
        /* 4. 大屏幕下 大于等于1200 布局容器修改为 1170 */
        
        @media screen and (min-width: 1200px) {
            .container {
                width: 1170px;
            }
        }
```

![image-20210319224446203](D:\Typora记录文档\img\image-20210319224446203.png)

**2.响应式布局容器**

响应式需要一个父级做为布局容器，来配合子级元素来实现变化效果。

原理就是在不同屏幕下，通过媒体查询来改变这个布局容器的大小，再改变里面子元素的排列方式和大小，从而实现不同

屏幕下，看到不同的页面布局和样式变化



#### 2. Bootstrap前端开发框架

**1.Bootstrap简介**

Bootstrap 来自 Twitter（推特），是目前最受欢迎的前端框架。Bootstrap 是基于 HTML、CSS 和 JAVASCRIPT 的，它

简洁灵活，**使得 Web 开发更加快捷。**

中文官网：**http://www.bootcss.com/**

官网：**http://getbootstrap.com/**

 推荐使用：**http://bootstrap.css88.com/**

优点：①标准化的html+css编码规范

​		    ②提供了一套简洁、直观、强悍的组件

​		    ③有自己的生态圈，不断的更新迭代

​		    ④让开发更简单，提高了开发效率

**2.版本**

2.x.x：停止维护,兼容性好,代码不够简洁，功能不够完善。

3.x.x：目前使用最多,稳定,但是放弃了 IE6-IE7。对 IE8 支持但是界面效果不好,偏向用于开发响应式布局、**移**

**动设备优先的 WEB 项目。**

4.x.x ：最新版，目前还不是很流行



**3.使用**

Bootstrap 使用四步曲： **1. 创建文件夹结构** 2. 创建 html 骨架结构 3. 引入相关样式文件 4. 书写内容

**①.创建文件夹结构**

![image-20210319230620386](D:\Typora记录文档\img\image-20210319230620386.png)

**②创建html骨架结构**

```
<!--要求当前网页使用IE浏览器最高版本的内核来渲染-->
 <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--视口的设置：视口的宽度和设备一致，默认的缩放比例和PC端一致，用户不能自行缩放-->
 <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=0">
 <!--[if lt IE 9]>
 <!--解决ie9以下浏览器对html5新增标签的不识别，并导致CSS不起作用的问题-->
 <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
 <!--解决ie9以下浏览器对 css3 Media Query 的不识别 -->
 <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
 <![endif]-->
```

**③引入相关样式文件**

**初学的时候只引入了压缩版的css文件按，后续深入学习后在添加！**

```html
<link rel="stylesheet" href="bootstrap/css/bootstrap.min.css">
```

**④书写内容**

```
1.直接拿Bootstrap 预先定义好的样式来使用。

2.修改Bootstrap 原来的样式，注意权重问题。

3.学好Bootstrap 的关键在于知道它定义了哪些样式，以及这些样式能实现什么样的效果。
```



**4.布局容器**

Bootstrap 需要为页面内容和栅格系统包裹一个 .container 容器，它提供了两个作此用处的类。

**1.container类**

```
响应式布局的容器 固定宽度。

大屏 ( >=1200px) 宽度定为 1170px。

中屏 ( >=992px) 宽度定为 970px。

小屏 ( >=768px) 宽度定为 750px。

超小屏 (100%) 。
```

**2.container-fluid类**

```
流式布局容器 百分百宽度。

占据全部视口（viewport）的容器。
```



#### 3. 栅格系统简介

栅格系统是指将页面布局划分为**等宽的列**，然后通过列数的定义来模块化页面布局。

Bootstrap 提供了一套响应式、移动设备优先的流式栅格系统，随着屏幕或视口（viewport）尺寸的增加，系统会

自动分为最多**12列**。



**栅格选项参数**

栅格系统用于通过一系列的行（row）与列（column）的组合来创建页面布局，你的内容就可以放入这些创建好的布局中。

![image-20210319231633185](D:\Typora记录文档\img\image-20210319231633185.png)

```
1.按照不同屏幕划分为1~12 等份。

2.行（row） 可以去除父容器作用15px的边距。

3.xs-extra small：超小； sm-small：小； md-medium：中等； lg-large：大；

4.列（column）大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。

5.每一列默认有左右15像素的 padding。

6.可以同时为一列指定多个设备的类名，以便划分不同份数 例如 class="col-md-4 col-sm-6"。
```

**注意：栅格系统是按页面内容划分12列的，并不是按屏幕宽度划分!!!**

还有注意的点:

①如果孩子的份数相加等于12,则孩子能占满整个的container的宽度

②如果孩子的份数相加小于12,则占不满整个container的宽度,会有空白

③如果孩子的分数相加大于12,多余的那一列会另起一行显示.（最后那一列另起一行显示。）



**列嵌套**

​		为了在内容中嵌套默认的网格，请添加一个新的 **.row**，并在一个已有的 **.col-md-\*** 列内添加一组 **.col-md-\*** 列。被嵌套的行应包含一组列，这组列个数不能超过12（其实，**没有要求你必须占满12列**）

```
<div class="container">
        <div class="row">
            <div class="col-md-5">
                <div class="row">
                    <div class="col-md-4">123</div>
                    <div class="col-md-4">456</div>
                </div>
            </div>
            <div class="col-md-5">2</div>
            <div class="col-md-2">3</div>
        </div>
    </div>
```



**列偏移**

使用 .col-md-offset-* 类可以将列向右侧偏移。这些类实际是通过使用 * 选择器为当前元素增加了左侧的边距（margin）。

![image-20210320151630261](D:\Typora记录文档\img\image-20210320151630261.png)

```
    <div class="container">
        <!-- 列偏移 -->
        <div class="row">
            <div class="col-lg-4">左侧</div>
            <div class="col-lg-4 col-lg-offset-4">右侧</div>
        </div>
    </div>
```

列偏移量+列数>12，偏移的元素会换到另一行。



**列排序**

通过使用 .col-md-push-* 和 .col-md-pull-* 类就可以很容易的改变列（column）的顺序。

```
 <!-- 列排序 -->
 <div class="row">
 <div class="col-lg-4 col-lg-push-8">左侧</div>// push：推
 <div class="col-lg-8 col-lg-pull-4">右侧</div> //pull：拉
 </div>
```

![image-20210320152351869](D:\Typora记录文档\img\image-20210320152351869.png)

**响应式工具**

为了加快对移动设备友好的页面开发工作，利用媒体查询功能，并使用这些工具类可以方便的针对不同设备展示或隐藏页面内容。

![image-20210320164924657](D:\Typora记录文档\img\image-20210320164924657.png)

Bootstrap 其他（按钮、表单、表格） 请参考Bootstrap 文档。

**visible-xx：在某种屏幕下才显示**