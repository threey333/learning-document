# 前端学习(HTML+CSS)

## day1

###### 		标签学习

**h1~h6标签、p段落标签、br/换行标签**。

**加粗:<strong></strong>或<b></b>**

**斜体:<em></em>或<i></i>**

删除<del></del>或<s></s>

下划线:<ins></ins>或<u></u>

![image-20210207231331910](D:\Typora记录文档\img\image-20210207231331910.png)

**<div>标签独占一行，可以理解为占一行的大盒子。**

**<span>标签不独占一行，一行中可以有多个span盒子，可以理解为小盒子。**



##### 图像标签的属性：

![image-20210207233342666](D:\Typora记录文档\img\image-20210207233342666.png)

**！！这里要说一下，在实际开发中，width和height这两个属性改其中一个就行了，以免造成失真现象。**

## Day2 

1.<a>标签超链接中的**锚点链接**之前没用过，这个功能主要是用来点击一处能快速直接跳转相应的位置。

​	用法：①先在相应的位置标签添加id属性值，比如

```html
<h3 id="one">第三处</h3>
```

​				②在链接文本a标签中的href属性中，设置属性值为**#+id值**,如

```
<a href="#one">点击这里跳转到h3处</a>
```

，这样就会跳转到h3的位置处。

​	还有一点要注意的，如果超链接地址是一个文件或者压缩包，点击则会下载该文件。

2.表格总结:①标签有table、tr、th、td、thead、tbody。

​					②相关属性![image-20210208220035318](D:\Typora记录文档\img\image-20210208220035318.png)

这里解释一下方便区分cellspacing和cellpadding：**cellpadding是内容和边距之间的距离。cellspacing是边框和边框之间的距离。**

​					③合并单元格(这个内容我之前也用的少，在此总结一下方法):

​							rowspan是跨行合并，colspan是跨列合并。![image-20210208220647448](D:\Typora记录文档\img\image-20210208220647448.png)

```html
合并单元格步骤:
1.先确定是跨行还是跨列合并。
2.找到目标单元格.写上合并方式=合并的单元格数量。
比如:<td colspan= "2” ></td>。
3．删除多余的单元格。(跨行合并则删除行上多余的单元格，跨列合并则删除列上多余的单元格。)
```

3.列表标签：**有序列表**、无序列表、**自定义列表**。

①有序列表的标签：<ol>、<li>。（理解就行）

​	语法格式：

```html
<ol>
  <li>列表1</li>
  <li>列表2</li>
  <li>列表3</li>
</ol>
```

![image-20210209010527355](D:\Typora记录文档\img\image-20210209010527355.png)

②无序列表的标签：<ul>、<li>。

​	语法格式:

```html
<ul>
	<li>列表1</li>
  <li>列表2</li>
  <li>列表3</li>
</ul>
```

![image-20210209010854660](D:\Typora记录文档\img\image-20210209010854660.png)

③自定义列表标签：<dl>、<dt>、<dd>。

​	语法格式：

```html
<dl>
	<dt>名词1</dt>
  <dd>名词1解释1</dd>
  <dd>名词1解释2</dd>
  <dd>名词1解释3</dd>
</dl>
```

​	使用的场景一般是以下界面：![img](D:\Typora记录文档\img\E(BGXAM1W0VT%PSQPW%)ZX.png)

![image-20210209011544648](D:\Typora记录文档\img\image-20210209011544648.png)

​		①<dl></dl>里面只能包含<dt>和<dd>。

​		②<dt>和<dd>个数没有限制，经常是一个<dt>对应多个<dd>。

4.表单标签(跟后台打交道要用的)：表单域：<form></form>、表单控件(表单元素)、提示信息。

​	在表单域中可以定义各种表单元素，其中有：`input输入表单元素`、`select表单元素`、`textarea文本域元素`。

​	①input表单元素有以下常用属性：type属性、name属性、value属性、checked属性、maxlength属性。

**type属性的属性值**有很多：如图![image-20210209013854979](D:\Typora记录文档\img\image-20210209013854979.png)

**注意！！属性值是text、password时,用placeholder属性可以有提示信息**

其余4个属性的属性值如图：

![image-20210209014031654](D:\Typora记录文档\img\image-20210209014031654.png)

1. name和value是每个表单元素都有的属性值,主要给后台人员使用.

2. name表单元素的名字,要求**单选按钮和复选框要有相同的name值**.

3. **checked属性主要针对于单选按钮和复选框**,主要作用一打开页面,就要可以默认选中某个表单元素.

4. maxlength是用户可以在表单元素输入的最大字符数,一般较少使用.

   

5.label标签：用于绑定表单元素的提示信息，当点击<label>标签内的文字时可以直接将焦点聚集跳转在表单元素上。

​	语法格式：

```html
<label for="boy">男</label>
<input type="radio" name="sex" id="boy" />
<label for="girl">女</label>
<input type="radio" name="sex" id="girl" />
```

核心:label标签的**for属性值**对应相关元素的**id属性值**



6.select表单元素

语法:

```html
<select>
    <option>选择1</option>
    <option>选择2</option>
    <option>选择3</option>
    <option>选择4</option>
</select>
```

![image-20210209184111229](D:\Typora记录文档\img\image-20210209184111229.png)



> textarea文本域元素

语法

```html
<textarea name="js实时预览" id="txt" cols="2" rows="3">
    文本内容
</textarea>
```

==注意：==

在谷歌chrome内核的浏览器中：

​			rows:1==>Height是21px,后面每次加1行，都是加15px；

​			cols:2==>Width是30px,后面每次加1列，都是加7px;

在其他浏览器中：

​			rows:1==>Height是21px,后面每次加1行，都是加15px；

​			cols:2==>Width是31px,后面每次加1列，都是加8px;

![image-20210209184244470](D:\Typora记录文档\img\image-20210209184244470.png)

8.查阅文档的网址:

百度：http://www.baidu.com

W3C:http://www.w3school.com.cn/

MDN: https://developer.mozilla.org/zh-CN/



## Day3

1.CSS选择器主要分为**基础选择器**和**复合选择器**这两大类。

基础选择器有：标签选择器、类选择器、id选择器和通配符选择。(类选择器用的会比较多)。

​	①这里说明一下类选择器中的多类名选择器(这个我之前没怎么用过这里要注意一下)：

```html
<style>
  .red {
    background-color:red;
  }
  .size {
    width:100px;
  }
</style>
<div class="red size">盒子</div>
```

如以上案例，在标签class属性中写多个类名，那这个标签就可以具有这些类名的样式。

**多类名选择器在后期布局比较复杂的情况下，还是较多的使用的。**

![image-20210210012247275](D:\Typora记录文档\img\image-20210210012247275.png)

②id选择器只能调用一次，即一个标签用过后，其它标签不能在用。（id选择器在样式中用的不多）

③总结基础选择器：

![image-20210210012706097](D:\Typora记录文档\img\image-20210210012706097.png)

**复合选择器：**常用的有后代选择器、子选择器、并集选择器、伪类选择器等等。

​	①后代选择器语法:       元素1 元素2 { 样式声明 }  **（重要）**

例如：

```html
ul li{ 样式声明 }
```

注意！！![image-20210220144647497](D:\Typora记录文档\img\image-20210220144647497.png)

​	②子选择器：

语法：

```html
元素1>元素2 { 样式声明 }
例如：
div>p { 样式声明 }
```

③并集选择器：

语法：元素1，元素2 { 样式声明 }

例如：

```html
ul,div { 样式声明 }
```

- **任何形式的选择器**（包括标签选择器、class类选择器 id选择器等），都可以作为并集选择器的一部分。
- 并集选择器通常用于**集体声明** ，逗号隔开的，所有选择器都会执行后面样式，逗号可以理解为**和**的意思。



④链接伪类选择器：

​		链接伪类，是利用交集选择器.

- `a:link` 未访问的链接
- `a:visited` 已访问的链接
- `a:hover` 鼠标移动到链接上
- `a:active` 选定的链接，即鼠标按时不放手时的状态。

实际开发中a:hover这个用比较多。一般写法如下：

```CSS
a {
	text-decoration: none;
	color: red;
}
a:hover {
  color:blue;
}

```

此时鼠标移动到链接上，链接会变颜色。

​	⑤:focus伪类选择器

**:focus伪类选择器**用于选取获得焦点的表单元素。

用法：

```CSS
input:focus {
	background-color:yellow;
}
```

**总结：**

![image-20210220151548951](D:\Typora记录文档\img\image-20210220151548951.png)



2.CSS字体样式

![image-20210210013430164](D:\Typora记录文档\img\image-20210210013430164.png)

字体样式综合写法：

```css
body {
	font: font-style font-weight font-size/line-height font-family;
}
```

- 注意：使用font属性时，必须按上面语法格式中的顺序书写，不能更换顺序，各个属性以`空格`隔开

- - 其中不需要设置的属性可以省略(取默认值),但**必须保留`font-size`和`font-family`属性**，否则font属性将不起作用。
  - **font-family:  'Microsoft Yahei'这个属性必须写，因为有写浏览器默认的字体不是微软雅黑，所以要考虑不同浏览器的兼容性。**

3.text-indent

​	①text-indent属性用来将段落首行缩进。

​	②段落首行缩进建议用em为单位，em是一个相对单位，即会跟随font-size属性值的大小，如果没有设置font-size大小则会按照父元素的1个文字大小。比如：如果是汉字的段落，则1em就是一个汉字的宽度。

4.line-height

​	①行高的表示：

![image-20210210234054988](D:\Typora记录文档\img\image-20210210234054988.png)

​	②多段落行高的测量：

![image-20210210234319705](D:\Typora记录文档\img\image-20210210234319705.png)

#### 行高与高度的三种关系

- 如果 行高 等 高度(height)  文字会 垂直居中
- 如果行高 大于 高度(height)  文字会 偏下
- 如果行高小于高度(height)  文字会  偏上



5.文本修饰text-decoration	![image-20210210235642351](D:\Typora记录文档\img\image-20210210235642351.png)





## Day4

![image-20210219225348127](D:\Typora记录文档\img\image-20210219225348127.png)

​	针对第5点说明：快速在标签中添加类名可以用以下方法，如果是直接输入类名，生成的就是<div>

```html
比如我输入.demo然后按tab键会生成<div class="demo"></div>
		 输入#demo按tab键会生成<div id="demo"></div>
		 输入span.demo按tab键会生成<span class="demo"></span>
		 输入table.demo按tab键会生成
```

​	针对第6点说明:

```html
比如输入.demo$*5会生成五个class类名后面有数值的<div class="demo1"></div>
```



2.**标签显示模式**：

​	标签显示模式是标签以什么方式进行显示。

​	HTML标签一般分为块标签和行内标签两种类型，它们也称为**块元素**和**行内元素**

3.**标签显示模式转换方法display**：

​	**①转为块元素：display:block;**

​	②转为行内元素:display:inline;

​	**③转为行内块元素:display:inline-block;**

①和③用的比较多，②用的比较少。



4.常用的块元素有：h1~h6、<p>、<div>、<ul>、<li>、<ol>等等。

​	常见的行内元素有:<span>、<a> 、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>等等。

​	常见的行内块元素有<img />、<input /> 、<td>。

**块元素特点：**

- 独占一行
- 高度，宽度，外边距以及内边距都可以控制。
- 宽度默认是容器(父级宽度)的100%
- 是一个容器及盒子，里面可以放行内或者块级元素

注意：像<p>这种文字类的块元素内是不能放块级元素的，同理<h1>~<h6>标签内也是不能放块级元素。



**行内元素特点:**

1. 相邻行内元素在一行上，一行可以显示多个。
2. 高度、宽度直接设置是无效的。
3. 默认宽度就是它本身内容的宽度。
4. **行内元素只能容纳文本或其他行内元素。**

**注意：** 

 链接里面不能再放链接

** 特殊情况链接 <a> 里面可以放块级元素，但是给 <a> 转换一下块级模式最安全**



**行内块元素特点:**

1. 和相邻行内元素(行内块)在一行上，但是之间会有空白风险。一行可以显示多个(行内元素特点)
2. 默认宽度就是它本身内容的宽度。(行内元素特点)
3. 高度、宽度，行高，外边距以及内边距都可以控制。(块级元素特点)



5**标签显示模式总结**

![image-20210220153821591](D:\Typora记录文档\img\image-20210220153821591.png)





## Day5



## CSS背景(background)

**「1. 背景颜色」**

```css
background-color: 颜色值;   默认的值是 transparent  透明的
```

**「2. 背景图片(image)」**

实际开发常见于 logo 或者一些装饰性的小图片或者是超大的背景图片, 优点是非常便于控制位置. (精灵图也是一种运用场景)

```css
语法：
background-image : none | url (url) ;
例如:
background-image: url(images/1.png);
```

**「3. 背景平铺（repeat）」**

```
background-repeat : repeat | no-repeat | repeat-x | repeat-y 
```

| 参数      |                 作用                 |
| :-------- | :----------------------------------: |
| repeat    | 背景图像在纵向和横向上平铺（默认的） |
| no-repeat |            背景图像不平铺            |
| repeat-x  |         背景图像在横向上平铺         |
| repeat-y  |          背景图像在纵向平铺          |

**「4. 背景位置(position)」**

```css
background-position : length || length
background-position : position || position 
```

| 参数     |                              值                              |
| :------- | :----------------------------------------------------------: |
| length   |         百分数 \| 由浮点数字和单位标识符组成的长度值         |
| position | top \| center \| bottom \| left \| center \| right  方位名词 |

#### 注意：

- 必须先指定background-image属性
- position 后面是x坐标和y坐标。可以使用方位名词或者 精确单位。
- 如果指定两个值，两个值都是方位名字，则两个值前后顺序无关，比如left  top和top  left效果一致
- 如果只指定了一个方位名词，另一个值默认居中对齐。
- 如果position 后面是精确坐标， 那么第一个，肯定是 x 第二个一定是y
- 如果只指定一个数值,那该数值一定是x坐标，另一个默认垂直居中
- 如果指定的两个值是 精确单位和方位名字混合使用，则第一个值是x坐标，第二个值是y坐标

**「5.背景图片滚动(attachment)」**

```css
background-attachment : scroll|fixed
```

![image-20210220231140367](D:\Typora记录文档\img\image-20210220231140367.png)



#### 背景简写：

- background：属性的值的书写顺序官方没有强制的标准。为了可读性，建议如下写：
- background: 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;

```css
/* 有背景图片背景颜色可以不用写*/
background: transparent url(image.jpg) repeat-y  scroll center top ;
```

**「5. 背景半透明(CSS3)」**

```css
background: rgba(0, 0, 0, 0.3);
background: rgba(0, 0, 0, .3);
```

- 等同于background-color: rgba(0, 0, 0, .3)
- 最后一个参数是alpha 透明度 取值范围 0~1之间
- 我们习惯把0.3 的 0 省略掉 这样写 background: rgba(0, 0, 0, .3);
- 注意：背景半透明是指盒子背景半透明，盒子里面的内容不受影响
- 低于IE 9的版本不支持

##### 盒子半透明 opacity

- 设置opacity元素的所有后代元素会随着一起具有透明性，**一般用于调整图片或者模块的整体不透明度**

```css
opacity: .2;
```

**「6. 背景总结」**

| 属性                  | 作用             | 值                                                           |
| :-------------------- | :--------------- | :----------------------------------------------------------- |
| background-color      | 背景颜色         | 预定义的颜色值/十六进制/RGB代码                              |
| background-image      | 背景图片         | url(图片路径)                                                |
| background-repeat     | 是否平铺         | repeat/no-repeat/repeat-x/repeat-y                           |
| background-position   | 背景位置         | length/position   分别是x  和 y坐标， 切记 如果有 精确数值单位，则必须按照先X 后Y 的写法 |
| background-attachment | 背景固定还是滚动 | scroll/fixed                                                 |
| 背景简写              | 更简单           | 背景颜色 背景图片地址 背景平铺 背景滚动 背景位置;  他们没有顺序 |
| 背景透明              | 让盒子半透明     | background: rgba(0,0,0,0.3);  后面必须是 4个值               |



## CSS三大特性

**「1. CSS 层叠性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bMdc6UDuvyOoic6olvqFUm4ug4lWznfZfhN6vVtE9LUtKr5zQk5EgtYw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

-`概念`：

- 所谓层叠性是指多种CSS样式的叠加
- 是浏览器处理冲突的一个能力,如果一个属性通过两个相同选择器设置到同一个元素上，那么这个时候一个属性就会将另一个属性层叠掉

-`原则`：

- 样式冲突，遵循的原则是就近原则。 那个样式离着结构近，就执行那个样式。
- 样式不冲突，不会层叠。

**「2. CSS 继承性」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bQ0WCHTI80JbCGUSnPJQvvvFvaUjGLIY7phiacwicpcPiaBunJewuXOdIg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)-`概念`：

- 子标签会继承父标签的某些样式，如文本颜色和字号。
- 想要设置一个可继承的属性，只需将它应用于父元素即可。

-`注意`：

- 恰当地使用继承可以简化代码，降低CSS样式的复杂性。比如有很多子级孩子都需要某个样式，可以给父级指定一个，这些孩子继承过来就好了。
- 子元素可以继承父元素的样式（**text-，font-，line-这些元素开头的可以继承，以及color属性**）

**「3. CSS 优先级(CSS特殊性)」**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bYhyc8Ticia3dGdKSsOkKbQRmkpsy6rS4nZmJG2ibvK6yfGcqeBbZYGyibw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)-`概念`：定义CSS样式时，经常出现两个或更多规则应用在同一元素上，此时，

- 选择器相同，则执行层叠性
- 选择器不同，就会出现优先级的问题。

-`权重计算公式`：

| 标签选择器               | 计算权重公式 |
| :----------------------- | :----------- |
| 继承或者 *               | 0,0,0,0      |
| 每个元素（标签选择器）   | 0,0,0,1      |
| 每个类，伪类             | 0,0,1,0      |
| 每个ID                   | 0,1,0,0      |
| 每个行内样式 style=""    | 1,0,0,0      |
| 每个!important  最重要的 | ∞ 无穷大     |

- 值从左到右，左面的最大，一级大于一级，数位之间没有进制，级别之间不可超越。
- 关于CSS权重，我们需要一套计算公式来去计算，这个就是 CSS Specificity（特殊性）
- div { color: pink !important; }

-`权重叠加`：

```
 div ul  li   ------>      0,0,0,3
 .nav ul li   ------>      0,0,1,2
 a:hover      -----—>      0,0,1,1
 .nav a       ------>      0,0,1,1
```

-`继承的权重是0`：

- 我们修改样式，一定要看该标签有没有被选中
- 如果选中了，那么以上面的公式来计权重。谁大听谁的。
- 如果没有选中，那么权重是0，因为继承的权重为0.



**注意：<a>标签默认的样式是 a { color: blue}且有下划线，所以<a>标签在继承中是不会改变的。**





## day6



## 盒子模型

css学习三大重点： **css 盒子模型 、 浮动 、 定位**  

**网页布局的本质**

- 首先利用CSS设置好盒子的大小，然后摆放盒子的位置。
- 最后把网页元素比如文字图片等等，放入盒子里面。

### 1. 盒子模型(Box Model)

- 盒子模型就是把HTML页面中的布局元素看作是一个矩形的盒子，也就是一个盛装内容的容器。
- 盒子模型由元素的内容、边框（border）、内边距（padding）、和外边距（margin）组成。
- 盒子里面的文字和图片等元素是 内容区域
- 盒子的厚度 我们称为为盒子的边框
- 盒子内容与边框的距离是内边距
- 盒子与盒子之间的距离是外边距

**W3c标准盒子模型**

标准 w3c 盒子模型的范围包括 margin、border、padding、content

当设置为**box-sizing: content-box;**时，将采用标准模式解析计算，也是**默认模式**；

```
内盒尺寸计算(元素实际大小)
```

- 高度：Element Height = content height + padding + border （Height为内容高度）

- 宽度：Element  Width = content width + padding + border （Width为内容宽度）

- 盒子的实际大小：**内容的宽度和高度 +  内边距  +  边框**  ![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b3UoX2rImDIa63t2y9NfoobMuKib4I0Dhn0szXElibTw9YsdBDbPsavPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

  **IE盒子模型**

IE 盒子模型的 content 部分包含了 border 和 pading

当设置为**box-sizing: border-box**时，将采用**怪异模式**解析计算；

### 2. 盒子边框(border)

| 属性         |          作用          |
| :----------- | :--------------------: |
| border-width | 定义边框粗细，单位是px |
| border-style |       边框的样式       |
| border-color |        边框颜色        |

**边框的样式：**

- none：没有边框即忽略所有边框的宽度（默认值）
- **solid：边框为单实线(最为常用的)**
- dashed：边框为虚线
- dotted：边框为点线

```
边框综合设置
border : border-width || border-style || border-color 

border: 1px solid red;  没有顺序要求  
```

**盒子边框写法总结表：**

很多情况下，我们不需要指定4个边框，我们是可以单独给4个边框分别指定的。

| 上边框                     | 下边框                        | 左边框                      | 右边框                       |
| :------------------------- | :---------------------------- | :-------------------------- | :--------------------------- |
| border-top-style:样式;     | border-bottom-style:样式;     | border-left-style:样式;     | border-right-style:样式;     |
| border-top-width:宽度;     | border- bottom-width:宽度;    | border-left-width:宽度;     | border-right-width:宽度;     |
| border-top-color:颜色;     | border- bottom-color:颜色;    | border-left-color:颜色;     | border-right-color:颜色;     |
| border-top:宽度 样式 颜色; | border-bottom:宽度 样式 颜色; | border-left:宽度 样式 颜色; | border-right:宽度 样式 颜色; |

**表格的细线边框：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bjM8eDXUbtfG62HugJaC84WZTjk08dNWiasE1nvCdX7OgnCoovIaKovg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 通过表格的`cellspacing="0"`,将单元格与单元格之间的距离设置为0，

- 但是两个单元格之间的边框会出现重叠，从而使边框变粗

- 通过css属性：**table{ border-collapse:collapse; }**  

- - `collapse` 单词是合并的意思,`border-collapse: collapse;`表示相邻边框合并在一起。

```css
<style>
 table {
  width: 500px;
  height: 300px;
  border: 1px solid red;
 }
 td {
  border: 1px solid red;
  text-align: center;
 }
 table, td {
  border-collapse: collapse;  /*合并相邻边框*/
 }
</style>
```

### 2. 内边距(padding)

padding属性用于设置内边距。是指边框与内容之间的距离。

**设置**

| 属性           | 作用     |
| :------------- | :------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

**padding简写**

| 值的个数 | 表达意思                                        |
| :------- | :---------------------------------------------- |
| 1个值    | padding：上下左右内边距;                        |
| 2个值    | padding: 上下内边距   左右内边距 ；             |
| 3个值    | padding：上内边距  左右内边距  下内边距；       |
| 4个值    | padding: 上内边距 右内边距 下内边距 左内边距 ； |

当我们给盒子指定padding值之后， 发生了2件事情：

1. 内容和边框 有了距离，添加了内边距。
2. 盒子会**变大**

**解决措施：**通过给设置了宽高的盒子，减去相应的内边距的值，维持盒子原有的大小。

**padding不影响盒子大小情况：👉**如果没有给一个盒子指定宽度， 此时，如果给这个盒子指定padding， 则不会撑开盒子。

### 3. 外边距（margin）

margin属性用于设置外边距。margin就是控制`盒子和盒子之间的距离`

**设置**

| 属性          | 作用     |
| :------------ | :------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

margin值的简写 （复合写法）代表意思  跟 padding 完全相同。

**块级盒子水平居中**

- 盒子必须**指定宽度（width）**
- 然后就给**左右的外边距都设置为auto**

实际工作中常用这种方式进行网页布局，示例代码如下：

```css
.header  { width: 960px; margin: 0 auto;}
```

常见的写法，以下下三种都可以👇👇。

- margin-left: auto;  margin-right: auto;
- margin: auto;
- **margin: 0 auto;（建议用这种）**

**文字居中和盒子居中区别👇👇**

1. 盒子内的文字水平居中是 text-align: center; 而且还可以让 行内元素和行内块居中对齐
2. 块级盒子水平居中  左右margin 改为 auto

**插入图片和背景图片区别👇👇**

1. `插入图片`我们用的最多 比如产品展示类  移动位置只能靠盒模型 padding margin
2. `背景图片`我们一般用于小图标背景或者超大背景图片、背景图片，移动位置只能通过  background-position

**清除元素的默认内外边距👇👇**

- **行内元素为了照顾兼容性,尽量只设置左右内外边距，不要设置上下内外边距。**

```css
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```

### 4.外边距合并

使用margin定义块元素的**「垂直外边距」**时，可能会出现外边距的合并。

###### (1). 相邻块元素垂直外边距的合并

- 当上下相邻的两个块元素相遇时，如果上面的元素有下外边距margin-bottom
- 下面的元素有上外边距margin-top，则他们之间的垂直间距不是margin-bottom与margin-top之和
- **「取两个值中的较大者」**这种现象被称为相邻块元素垂直外边距的合并（也称外边距塌陷）。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b15wO9U7G1QHGZRB5OIsdJaCdPeg7MzVYtTsbhfGPusK7OCOwWtvs4A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**「解决方案：尽量只给一个盒子添加margin值」**。



###### (2). 嵌套块元素垂直外边距的合并（塌陷）

- 对于两个嵌套关系的块元素，如果父元素没有上内边距及边框
- 父元素的上外边距会与子元素的上外边距发生合并
- 合并后的外边距为两者中的较大者

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8by8DvQuDYlXKtzlIWtltPGPW0HtWAqoaWKr6cy6jYzc4hrVmR6CFlDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**「解决方案：」**

1. 可以为父元素定义上边框。
2. 可以为父元素定义上内边距
3. 可以为父元素添加overflow: hidden。

还有其他方法，比如浮动、固定、绝对定位的盒子不会有问题，后面咱们再总结。。。

#### 盒子模型布局稳定性

优先使用  宽度 （width）  其次 使用内边距（padding）   再次  外边距（margin）

```
width >  padding  >   margin   
```

**原因：**

- margin 会有外边距合并 还有 ie6下面margin 加倍的bug（讨厌）所以最后使用。
- padding  会影响盒子大小， 需要进行加减计算（麻烦） 其次使用。
- width  没有问题（嗨皮）我们经常使用宽度剩余法 高度剩余法来做。



###### **1.问题：如何去掉li前面的项目符号(小圆点)**

```CSS
用 li {
	list-style: none;
}
```





## Day7



###### 1.圆角边框：

```
border-radius:length;

border-top-left-radius   定义了左上角的弧度
border-top-right-radius   定义了右上角的弧度
border-bottom-right-radius   定义了右下角的弧度
border-bottom-left-radius   定义了左下角的弧度
```

- 其中每一个值可以为 **数值**或**百分比**的形式。
- 技巧：让一个正方形 变成圆圈

```
border-radius: 50%;
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8beaL3cUZXibAoj8ibW8c2cTVhXndJ1ELNMvttZxzRdTD15uqN0G72mM5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)如果要在四个角上一一指定，可以使用以下规则👇👇：

```
border-radius: 左上角 右上角  右下角  左下角;
```

1. 四个值: 第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角。
2. 三个值: 第一个值为左上角, 第二个值为右上角和左下角，第三个值为右下角
3. 两个值: 第一个值为左上角与右下角，第二个值为右上角与左下角
4. 一个值：四个圆角值相同



**如果让一个长方形变成一个椭圆形，则给border-radius的属性值设置高的一般就行。**

​	例如：

```css
        width: 200px;
        height: 100px;
        border-radius: 50px;
```

![image-20210223162959558](D:\Typora记录文档\img\image-20210223162959558.png)

###### 2.盒子阴影

```
box-shadow: offset-x offset-y [blur [spread]] [color] [inset]
```

| 值       | 描述                                                 |
| :------- | :--------------------------------------------------- |
| h-shadow | 阴影的水平偏移量。正数向右偏移，负数向左偏移。(必需) |
| v-shadow | 阴影的垂直偏移量。正数向下偏移，负数向上偏移。(必需) |
| blur     | 可选。阴影模糊距离，不能取负数。                     |
| spread   | 可选。阴影大小                                       |
| color    | 可选。阴影的颜色                                     |
| inset    | 可选。表示添加内阴影，默认为外阴影                   |

```css
div {
   width: 200px;
   height: 200px;
   border: 10px solid red;
   /* box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);  */
   /* box-shadow:水平位置 垂直位置 模糊距离 阴影尺寸（影子大小） 阴影颜色  内/外阴影； */
   box-shadow: 0 15px 30px  rgba(0, 0, 0, .4);   
}
```



###### 3.文字阴影

```css
text-shadow: h-shadow v-shadow blur color
```

![image-20210223163456687](D:\Typora记录文档\img\image-20210223163456687.png)



###### 4.浮动

​	**「1. CSS布局的三种机制」**

> 网页布局的核心——就是**用CSS来摆放盒子**。

CSS 提供了3种机制来设置盒子的摆放位置，分别是普通流（标准流）、浮动和定位，其中：

**A. 普通流（标准流）**

- 块级元素会独占一行，从上向下顺序排列；

- - 常用元素：div、hr、p、h1~h6、ul、ol、dl、form、table

- 行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行；

- - 常用元素：span、a、i、em等

**B. 浮动**

- 让盒子从普通流中浮起来,主要作用让多个块级盒子一行显示。

**C. 定位**

- 将盒子定在浏览器的某一个位置——CSS 离不开定位，特别是后面的 js 特效。

  

  **「2. 什么是浮动」**元素的浮动是指设置了浮动属性的元素会

- 脱离标准普通流的控制,**不占位置**，脱标

- 移动到指定位置。

##### 作用

1. 让多个盒子(div)水平排列成一行，使得浮动称为布局的重要手段。
2. 可以实现盒子的左右对齐等等。
3. 浮动最早是用来控制图片，实现文字环绕图片效果。
4. float属性会改变元素的display属性，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。生成的块级框和我们前面的行内块极其相似。

##### 语法

```
选择器 { float: 属性值; }
```

| 属性值 | 描述                 |
| :----- | :------------------- |
| none   | 元素不浮动（默认值） |
| left   | 元素向左浮动         |
| right  | 元素向右浮动         |



> 浮动只会影响当前的或者是后面的标准流盒子，不会影响前面的标准流。
> **建议:**如果一个盒子里面有多个子盒子，如果其中一个盒子浮动了，其他兄弟也应该浮动。防止引起问题

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bnahuic1SXtFcVOiaDrcDGb8S21rrKf46W1p6ibqmFSqW6mlTzGodxBwyA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**浮动(float)小结**

| 特点 | 说明                                                         |
| :--- | :----------------------------------------------------------- |
| 浮   | 加了浮动的盒子**「是浮起来」**的，漂浮在其他标准流盒子的上面。 |
| 漏   | 加了浮动的盒子**「是不占位置的」**，它原来的位置**「漏给了标准流的盒子」**。 |
| 特   | **「特别注意」**：浮动元素会改变display属性， 类似转换为了行内块，但是元素之间没有空白缝隙 |

###### 5.网页布局准则

​	①第一准则：先用标准流(文档流)的父元素排列上下位置，之后在内部子元素采用浮动排列左右位置。

​	②第二准则：先设置盒子的大小，之后设置盒子的位置。



###### 6.清除浮动

​	因为父级盒子很多情况下，不方便给高度，但是子盒子浮动就不占有位置，最后父级盒子高度为0，就影响了下面的标准流盒子。![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bMdS71HhXE4ZoSlJzhd7YhgiczkM9JgE3NeVkgs1V15CzQXdSKMCf81Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8baoekQGiaPwbIKRHticXfUA2rEM8grBmN1u6NcEvvb3tY44EQ9SiaZledA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)**总结：**

- 由于浮动元素不再占用原文档流的位置，所以它会对后面的元素排版产生影响
- 准确地说，并不是清除浮动，而是清除浮动后造成的影响

**清除浮动本质**清除浮动主要为了解决父级元素因为子级浮动引起内部高度为0 的问题。清除浮动之后， 父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了

#### 清除浮动的方法

```
选择器 { clear: 属性值; }   clear 清除  
```

| 属性值 | 描述                                       |
| :----- | :----------------------------------------- |
| left   | 不允许左侧有浮动元素（清除左侧浮动的影响） |
| right  | 不允许右侧有浮动元素（清除右侧浮动的影响） |
| both   | 同时清除左右两侧浮动的影响                 |

**实际工作中,几乎只用clear: both**

**清除浮动的策略是：闭合浮动**



**1).额外标签法(隔墙法)**

是W3C推荐的做法是通过在浮动元素末尾添加一个空的标签例如 <div style=”clear:both”></div>，或则其他标签br等亦可。

- 优点：通俗易懂，书写方便
- 缺点：添加许多无意义的标签，结构化较差。

**2).父级添加overflow属性方法**

```
可以给父级添加： overflow为 hidden| auto| scroll  都可以实现。
```

- 优点： 代码简洁
- 缺点： 内容增多时候容易造成不会自动换行导致内容被隐藏掉，无法显示需要溢出的元素。

**3).使用after伪元素清除浮动**:after 方式为空元素额外标签法的升级版，好处是不用单独加标签了

```css
.clearfix:after {
  content: "";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}

/* IE6、7 专有 */
.clearfix {
  *zoom: 1;
}

```

- 优点：符合闭合浮动思想  结构语义化正确
- 缺点：由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

**4).使用双伪元素清除浮动**

```css
.clearfix:before,
.clearfix:after {
  content: "";
  display: table;
}

.clearfix:after {
  clear: both;
}

.clearfix {
  *zoom: 1;
}
```

- 优点： 代码更简洁
- 缺点： 由于IE6-7不支持:after，使用 zoom:1触发 hasLayout。

#### 清除浮动总结

```
什么时候用清除浮动呢？
```

1. **父级没高度**
2. **子盒子浮动了**
3. **影响下面布局了，我们就应该清除浮动了。**



| 清除浮动的方式       | 优点               | 缺点                               |
| :------------------- | :----------------- | :--------------------------------- |
| 额外标签法（隔墙法） | 通俗易懂，书写方便 | 添加许多无意义的标签，结构化较差。 |
| 父级overflow:hidden; | 书写简单           | 溢出隐藏                           |
| 父级after伪元素      | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |
| 父级双伪元素         | 结构语义化正确     | 由于IE6-7不支持:after，兼容性问题  |



###### 7.CSS属性书写顺序(重点)（良好的书写规范）

​	建议遵循以下顺序：

​		1.布局定位属性：display / position / float / clear / visibility / overflow（建议 display 第一个写，毕竟关系到模式）

​		2.自身属性：width / height / margin / padding / border / background

​		3.文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word

​		4.其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

```css
.jdc {
    display: block;
    position: relative;
    float: left;
    width: 100px;
    height: 100px;
    margin: 0 10px;
    padding: 20px 0;
    font-family: Arial, 'Helvetica Neue', Helvetica, sans-serif;
    color: #333;
    background: rgba(0,0,0,.5);
    -webkit-border-radius: 10px;
    -moz-border-radius: 10px;
    -o-border-radius: 10px;
    -ms-border-radius: 10px;
    border-radius: 10px;
}
```





## Day8



### 8.1 定位(position)

#### 8.1.1 定位详解

将盒子**「定」**在某一个**「位」**置  自由的漂浮在其他盒子(包括标准流和浮动)的上面。

所以，我们脑海应该有三种布局机制的上下顺序
标准流在最底层 (海底)  -------   浮动 的盒子 在 中间层  (海面)  -------  定位的盒子 在 最上层  （天空）

**定位**是用来布局的，它有两部分组成：定位 = 定位模式 + 边偏移在 CSS 中，通过 `top`、`bottom`、`left` 和 `right` 属性定义元素的**「边偏移」**：（方位名词）

| 边偏移属性 | 示例           | 描述                                                         |
| :--------- | :------------- | :----------------------------------------------------------- |
| `top`      | `top: 80px`    | **「顶端」**偏移量，定义元素相对于其父元素**「上边线的距离」**。 |
| `bottom`   | `bottom: 80px` | **「底部」**偏移量，定义元素相对于其父元素**「下边线的距离」**。 |
| `left`     | `left: 80px`   | **「左侧」**偏移量，定义元素相对于其父元素**「左边线的距离」**。 |
| `right`    | `right: 80px`  | **「右侧」**偏移量，定义元素相对于其父元素**「右边线的距离」** |

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b1MmIfz1wBVX3PXMGUVrAqRxdibY8EYy1q4xPibScXf5xQpicMYLJe71DQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 8.1.2  定位模式(position)

在 CSS 中，通过 `position` 属性定义元素的**「定位模式」**，语法如下：

```
选择器 { position: 属性值; }
```

| 值         |   语义   |
| :--------- | :------: |
| `static`   | 静态定位 |
| `relative` | 相对定位 |
| `absolute` | 绝对定位 |
| `fixed`    | 固定定位 |

#### 8.1.3 静态定位(static)

- 静态定位是元素的默认定位方式，无定位的意思。它相当于border里面的none，不要定位的时候用。
- 静态定位 按照标准流特性摆放位置。它没有边偏移。
- 静态定位在布局时几乎不用

###### **「2.4 相对定位(relative)」**

- 相对定位是元素相对于它原来在标准流中的位置来说的。![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bVfXeeVIdicb4akm3WYFGbwMqbs1ObZiaiauiaFkxSPX2NDmDkEH2BlY1uA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
- 相对于**自己原来在标准流中位置来移动的(即相对于自己原来的位置移动。)**
- 原来在标准流的区域继续占有，后面的盒子仍然以标准流的方式对待它。

###### **「1.5 绝对定位(absolute)」**

绝对定位是元素以带有定位的父级元素来移动位置

- 完全脱表--完全不占位置；
- **父元素没有定位，则以浏览器为准定位(Document文档)。（如果是嵌套的盒子，则绝对定位的盒子是以最近的父级有定位的作为参照，如果都没有，则以浏览器作为定位。）**



​						**多个盒子的情况：**

![image-20210304162809335](D:\Typora记录文档\img\image-20210304162809335.png)



代码：

```
<style type="text/css">
    .ffather {
        position: relative;
        width: 400px;
        height: 400px;
        background-color: brown;  //ffather是大盒子，棕色，有相对定位
    }
    .father {
        width: 200px;
        height: 200px;
        background-color: aqua;  
    }
    .son {
        position: absolute;  //相对于父级有相对定位作为参照物进行定位。
        bottom: 50px;
        left: 50px;
        width: 100px;
        height: 100px;
        background-color: blue;
    }
</style>
<body>
    <div class="ffather">
        <div class="father">
            <div class="son"></div>
        </div>
    </div>
</body>
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8b3kQIJPlKcMTgGicm7Hia18o1O538GDL2liaZBAnVXyhh1B142qx1zHO9A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 父元素有定位

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bqCrQxW1Sb43kqPZ9BUyXoBYljAM6kFSAsC9sPGWIZWo904f6iaib3kPA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 				!!定位口诀--子绝父相

###### **「1.6.固定定位(fixed)」**

固定定位是绝对定位的一种特殊形式;

- 完全脱标--完全不占位置；

- 只认**浏览器的可视窗口**--浏览器可视窗口+边偏移属性来设置元素的位置

- - 跟父元素没有任何关系；单独使用
  - 不随滚动条滚动

- 

###### 2.定位(position)的扩展

###### 2.1绝对定位的盒子居中

> 绝对定位/固定定位的盒子不能通过设置margin: auto设置水平居中 在使用绝对定位时要向实现水平居中，可以按照下面的方法：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bN6SCtF5tG61rscl5xZ0icicUDB8jtFtq5xibEqiba1WfUXOzkiadNjnX4nQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1. **left : 50%:让盒子的左侧移动到父级元素的水平中心位置；**
2. **margin-left: -100px;让盒子向左移动自身宽度的一半。**
3. **同理垂直居中。**

###### 2.2堆叠顺序（z-index）

在使用**「定位」**布局时，可能会**「出现盒子重叠的情况」**。

加了定位的盒子，默认**「后来者居上」**， 后面的盒子会压住前面的盒子。

应用 `z-index` 层叠等级属性可以**「调整盒子的堆叠顺序」**。如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8beCsHfDxy5VBkWU3ET4s4qtBpHm9iajaajxo2GeWWb81a04PfG5rmH0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)z-index的特性如下:

1. **属性值**：正整数、负整数或 0，默认值是 0，数值越大，盒子越靠上；

2. 如果属性值相同，则按照书写顺序，**后来居上**；

3. 数字后面不能加单位

4. **z-index只能用于相对定位、绝对定位和固定定位的元素，其他标准流、浮动和静态定位无效。**

   

###### 2.3定位改变display属性

前面提过， display 是 显示模式， 可以通过以下方式改变显示模式:

- 可以用inline-block  转换为行内块
- 可以用浮动 float 默认转换为行内块（类似，并不完全一样，因为浮动是脱标的）
- **绝对定位和固定定位也和浮动类似， 默认转换的特性 转换为行内块。**

所以说， 一个行内的盒子，如果加了**「浮动」**、**「固定定位」**和**「绝对定位」**，不用转换，就可以给这个盒子直接设置宽度和高度等。



###### 2.4定位小结

| 定位模式         | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| :--------------- | :------------------- | :--------------------- | :----------------- | :----------------------- |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |

**注意：**

1. `边偏移` 需要和 `定位模式` 联合使用，`单独使用无效`；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。



###### 3.元素的显示与隐藏

- **目的:**让一个元素在页面中消失或者显示出来
- **场景:**类似网站广告，当我们点击关闭就不见了，但是我们重新刷新页面，会重新出现！



###### 3.1 display 显示（重点）

display设置或检索对象是否显示或如何显示。

- display: none 隐藏对象

- - **特点：隐藏之后，不再保留位置。**

- display: block 除了转换为块级元素之外，同时还有显示元素的意思。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bIsqWiaIk6NLYYVTLwCWLThczx6srresNQxzaqanNPia06CDlCoibib4JiaQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)实际开发场景：配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛



###### 3.2 visibility 可见性

设置或检索是否显示对象

```
visibility：visible ;  对象可视

visibility：hidden;    对象隐藏
```

- 特点：隐藏之后，继续保留原有位置。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bWR8zicEo3L5npkKeSd74ibl4D2ZseBNAH4PviaAQmbdq9XRL0ef7ee3aA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



###### 3.3 overflow 溢出

检索或设置当对象的内容超过其指定高度及宽度时如何管理内容。

| 属性值  | 描述                                       |
| :------ | :----------------------------------------- |
| visible | 不剪切内容也不添加滚动条                   |
| hidden  | 不显示超过对象尺寸的内容，超出的部分隐藏掉 |
| scroll  | 不管超出内容否，总是显示滚动条             |
| auto    | 超出自动显示滚动条，不超出不显示滚动条     |

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bEFUTYKtKQ3Dm0dDAGKsSAw5QaSqPLgETx84IuU20677gOjya8BAR7A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)`实际开发场景`：

1. 清除浮动
2. 隐藏超出内容，隐藏掉,  不允许内容超过父盒子。



###### 3.4 显示与隐藏总结

| 属性       | 区别                   | 用途                                                         |
| :--------- | :--------------------- | :----------------------------------------------------------- |
| display    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
| visibility | 隐藏对象，保留位置     | 使用较少                                                     |
| overflow   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |



###### 4.CSS用户界面样式

所谓的界面样式， 就是更改一些用户操作样式，以便提高更好的用户体验。

- 更改用户的鼠标样式
- 表单轮廓等。
- 防止表单域拖拽

###### 4.1 鼠标样式

设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。

| 属性值      | 描述       |
| :---------- | :--------- |
| default     | 小白  默认 |
| pointer     | 小手       |
| move        | 移动       |
| text        | 文本       |
| not-allowed | 禁止       |

```
<ul>
  <li style="cursor:default">我是小白</li>
  <li style="cursor:pointer">我是小手</li>
  <li style="cursor:move">我是移动</li>
  <li style="cursor:text">我是文本</li>
  <li style="cursor:not-allowed">我是文本</li>
</ul>
```

###### 4.2 轮廓线 outline

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bdicbvJ2vdWvVm9PuHt5WLFn7XqYAx60k0El53qx9JfgLB734At4Ru9w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

```
outline : outline-color ||outline-style || outline-width 
```

但是我们都不关心可以设置多少，我们平时都是去掉的。
最直接的写法是 ： outline: 0;  或者  outline: none;

###### 4.3 防止拖拽文本域resize

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bHX3RwpjHtdppaZKxrEhq5QjZB339Zn5b7xMvy2XsrPblC9V7Z478JQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
<textarea  style="resize: none;"></textarea>
```

###### 4.4 用户界面样式总结

| 属性     | 用途                 | 用途                                                         |
| :------- | :------------------- | :----------------------------------------------------------- |
| 鼠标样式 | 更改鼠标样式cursor   | 样式很多，重点记住 pointer                                   |
| 轮廓线   | 表单默认outline      | outline 轮廓线，我们一般直接去掉，border是边框，我们会经常用 |
| 防止拖拽 | 主要针对文本域resize | 防止用户随意拖拽文本域，造成页面布局混乱，我们resize:none    |

###### 4.5 vertical-align 垂直对齐

- 有宽度的块级元素居中对齐，是margin: 0 auto;
- 让文字居中对齐，是 text-align: center;

vertical-align 垂直对齐，它只针对于**「行内元素」**或者**「行内块元素」**

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bdibQ1oDm9HIxTxiclw7Fq54j5nmKRK04kmDMrPsg6VZiaL6kDcKsBdibSw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```
设置或检索对象内容的垂直对其方式。
vertical-align : baseline |top |middle |bottom 
```

注意：

vertical-align 不影响块级元素中的内容对齐，它只针对于**「行内元素」**或者**「行内块元素」**，

特别是行内块元素， 通常用来控制图片/表单与文字的对齐。



###### 5.1 图片、表单和文字对齐

我们可以通过`vertical-align` 控制图片和文字的垂直关系了。默认的图片会和文字基线对齐。

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bfWPgKVgBq5JoRLkTdcjQxPG1eUAL8g1N9iaxOUHfD7ZN6otR5Kh4NAw/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bX32nStNY16aY9odqroA3Mpia6nia2fuh9DYmYCszG7V1to2VsNwibY8Sg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###### 5.2 去除图片底侧空白缝隙

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8boGWdjhQUvkicibGiaRYd0KDL2Y1kmqWLl75piaeMlRTMVh4go056Pj1OLA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**原因：**图片或者表单等行内块元素，他的底线会和父级盒子的基线对齐。

就是图片底侧会有一个空白缝隙。

**解决方法：**

- 给img vertical-align:middle | top| bottom等等。 让图片不要和基线对齐。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bNO8sogwyKtqHT6Bg0iaDeAkWqlbbSWqKJIGqtt8As1oFz17wBkQYb4g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

- 给img 添加 display：block; 转换为块级元素就不会存在问题了。



###### 6.溢出的文字省略号显示

###### 6.1 white-space

- white-space设置或检索对象内文本显示方式。通常我们使用于强制一行显示内容。

```css
white-space:normal ；默认处理方式

white-space:nowrap ； 强制在同一行内显示所有文本，直到文本结束或者遭遇br标签对象才换行。
```

###### 6.2 text-overflow 文字溢出

- 设置或检索是否使用一个省略标记（...）标示对象内文本的溢出

```
text-overflow : clip ；不显示省略标记（...），而是简单的裁切 

text-overflow：ellipsis ； 当对象内文本溢出时显示省略标记（...）
```

**「注意」**：

一定要首先强shen制一行内显示，再次和overflow属性  搭配使用

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmqtcdL7HZYccBic0jicaWzR8bJMSFBl0zS6bbLaYbicsptgr6KC0uTOEBhZViaFYNY96FibxUF3Xp3fReQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

###### 6.3 单行文字溢出省略号总结三步曲

```css
/*1. 先强制一行内显示文本*/
white-space: nowrap;
/*2. 超出的部分隐藏*/
overflow: hidden;
/*3. 文字用省略号替代超出的部分*/
text-overflow: ellipsis;
```

###### 6.4多行文字省略溢出（了解就行。一般推荐让后台人员负责处理。）

```css
overflow:hidden;
text-overflow:ellipsis;
/*弹性伸缩盒子模型显示*/
display:-webkit-box;
/*弹性伸缩盒子模型显示*/
-webkit-line-clamp:2;
/*设置或检索伸缩盒对象的子元素的排列方式*/
-webkit-box-orient:vertical;
```



## Day9

1.之前知识点补充：①因为相对定位不脱离标准流，所以还是可以用margin：auto进行居中对齐。

2.盒子间的线条问题：如图

![image-20210305223257613](D:\Typora记录文档\img\image-20210305223257613.png)

​	方法:①先利用margin负值技巧，例如margin:-1px;

​			 ②然后利用伪类元素进行添加相对定位即可。

​			 ③如果父级盒子有定位，则利用z-index提高层级

```css
<style>
        ul li {
            position: relative;
            float: left;
            width: 150px;
            height: 200px;
            list-style: none;
            border: 1px solid black;
            background-color: pink;
            margin-left: -1px;
        }
        ul li:hover {
            /* 如果盒子没有定位则用相对定位即可 */
            position: relative;
            border: 1px solid red;
        }
</style>
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
    </ul>
</body>
```

盒子有定位则利用z-index。	

```css
<style>
	      ul li:hover {
            如果盒子有定位则用z-index提高层级
            z-index: 1;
            border: 1px solid blue;
        } 
</style>
<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
    </ul>
</body>
```



3.CSS初始化代码。

```css
/* 把我们所有标签的内外边距清零 */
* {
    margin: 0;
    padding: 0;
}
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



4.利用三角组合想要的图形：

​	例如：![image-20210306142818702](D:\Typora记录文档\img\image-20210306142818702.png)

​	方法:利用小三角形定位。

​	三角形的做法:

```css
width:0;
height 0;
border-color:transparent 加想要的颜色 transparent transparent
border-style:solid;
border-width:数值 数值 0 0；
```

