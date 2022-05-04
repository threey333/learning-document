# 	JavaScript API部分

## 一、DOM

### 1.1 什么是DOM

DOM即文档对象模型，是 W3C 组织推荐的处理可扩展标记语言（HTML或者XML）的`标准编程接口`。 

### 1.2 DOM树

![image-20210330163702083](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330163702083.png)

 文档：一个页面就是一个文档，DOM 中使用 document 表示

 元素：页面中的所有标签都是元素，DOM 中使用 element 表示

 节点：网页中的所有内容都是节点（标签、属性、文本、注释等），DOM 中使用 node 表示

==DOM 把以上内容都看做是对象==

### 1.3 console.dir

使用console.dir,能更好打印我们放回的元素对象，更好的查看里面的属性和方法。

### 1.4 获取元素

注意：==**获取元素放回的是对象。**==

> 传统获取元素方法 （2种）

**1.通过getElementById('id');**

```javascript
document.getElementById('id');
```

**2.通过 getElementsByTagName() 方法可以返回带有指定标签名的对象的集合。**

```javascript
document.getElementsByTagName('标签名');

注意：
1. 因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历。
2. 得到元素对象是动态的
```

### 1.5 DOM tagName 属性

tagName 属性返回元素的标签名。

在 HTML 中，tagName 属性的==**返回值始终是大写的**==。

语法:

```javascript
element.tagName
```

示例

```javascript
<div class="box"></div>
const box = document.querySelector('.box');
console.log(box.tagName);   //DIV
```

### 1.6 H5新增加的获取元素方法

**1.通过类名获取元素对象的集合**

```javascript
getElementsClassName('类名');
```

**2.getSelector('选择器')**

```javascript
document.querySelector('选择器'); // 根据指定选择器返回第一个元素对象
```

**3.querySelectorAll('选择器')**

```javascript
document.querySelectorAll('选择器'); // 根据指定选择器返回   //放回的是元素对象集合
```

### 1.7 获取特殊元素（body,html）

**获取body元素**

```javascript
doucumnet.body // 返回body元素对象
```

**获取html元素**

```javascript
document.documentElement // 返回html元素对象
```



### 1.8 事件基础

事件由三部分组成：事件源、事件类型、事件处理程序。（事件三要素）

1.事件源：事件被触发的对象。

2.事件类型：如何触发。

3.事件处理程序：通过一个函数赋值的方式完成。

>我对事件的理解

1. 先获取事件对象（也叫事件节点）
2. 该节点要如何触发事件
3. 为事件定义函数执行。



**常见的鼠标事件**

![image-20210330171743504](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330171743504.png)

### 1.9 操作元素

JavaScript 的 DOM 操作可以改变网页内容、结构和样式，我们可以利用 DOM 操作元素来改变元素里面的内

容 、属性等。

#### 操作元素---改变元素内容

```javascript
1.element.innerText
```

从起始位置到终止位置的内容, 但它去除 **html 标签， 同时空格和换行也会去掉**

```javascript
2.element.innerHTML
```

起始位置到终止位置的全部内容，**包括 html 标签，同时保留空格和换行**

#### 操作元素---改变元素属性

```html
<body>
  <button id="ldh">刘德华</button>
  <button id="zxy">张学友</button> <br>
  <img src="images/ldh.jpg" alt="" title="刘德华">

  <script>
    // 修改元素属性  src
    // 1. 获取元素
    var ldh = document.getElementById('ldh');
    var zxy = document.getElementById('zxy');
    var img = document.querySelector('img');
    // 2. 注册事件  处理程序
    zxy.onclick = function() {
      img.src = 'images/zxy.jpg';
      img.title = '张学友思密达';
    }
    ldh.onclick = function() {
      img.src = 'images/ldh.jpg';
      img.title = '刘德华';
    }
  </script>
</body>
```

#### 操作元素---表单属性设置

利用 DOM 可以操作表单元素的一些属性：`type`、`value`、`checked`、`selected`，`disabled`。

disabled：如果想要某个表单被禁用 不能再点击 disabled。

```javascript
语法：元素对象.disabled
```

#### 操作元素---修改样式属性

element.style 行内样式操作

```html
示例：
<style>
  div {
    width: 200px;
    height: 200px;
    background-color: pink;
  }
</style>
<body>
  <div></div>
  <script>
    // 1. 获取元素
    var div = document.querySelector('div');
    // 2. 注册事件 处理程序
    div.onclick = function() {
      // div.style里面的属性 采取驼峰命名法 
      this.style.backgroundColor = 'purple';
      this.style.width = '250px';
    }
  </script>
</body>
```

如果样式修改过多，可以采取==操作类名==来更改元素样式。

语法：

element.className 类名样式操作

```javascript
const box = document.querySelector("#box1");box.addEventListener("click", function () {    box.setAttribute("class", "box2");});
```

==注意：==

1.JS 里面的样式采取驼峰命名法 比如 fontSize、 backgroundColor

2.JS 修改 style 样式操作，产生的是行内样式，CSS 权重比较高



### 1.10 排他思想

如果有同一组元素，我们想要某一个元素实现某种样式， 需要用到循环的排他思想算法：

1.所有元素全部清除样式（干掉其他人）

2.给当前元素设置样式 （留下我自己）

3.注意顺序不能颠倒，首先干掉其他人，再设置自己



### 1.11 自定义属性操作

#### 获取元素的属性值

(1) element.属性

```javascript
<div id="demo" index="1" class="nav"></div>
var div = document.querySelector('div');// 1. 获取元素的属性值// (1) element.属性console.log(div.id);
```

(2) element.getAttribute('属性')    ------------一般用于获取程序员自己添加的属性，即自定义属性。

```javascript
console.log(div.getAttribute('id'));
console.log(div.getAttribute('index'));
```

#### 设置元素属性值

(1) element.属性= '值'

```javascript
div.id='text'
```

(2) element.setAttribute('属性', '值')  ------------主要针对于自定义属性

```javascript
div.setAttribute('index', 2);
div.setAttribute('class', 'footer'); // class 特殊  这里面写的就是class 不是className
```

#### element.属性和element.setAttribute(‘属性’)的区别

element.属性 ---------设置内置属性值

element.setAttribute(‘属性’); ------主要设置自定义的属性 （标准）

#### 移除属性

element.removeAttribute(属性)

```javascript
div.removeAttribute('属性');
```



### 1.12 H5自定义属性

**自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中。**

之前我们也总结到了，自定义属性可以通过`element.getAttribute('属性')`获得。

但是这样子玩不容易区别是`自己定义的属性`还是`内置属性`，所以H5推出新的自定义属性的方式

#### H5自定义属性设置方式

H5规定自定义属性用`data-`开头来作为属性名并且赋值。

```javascript
H5设置自定义属性
比如 <div data-index=“1”></div>
element.setAttribute('data-index',2);

获取方式
element.getAttribute('data-index');
```

==h5新增的获取自定义属性的方法 它只能获取data-开头的==    示例如下

```javascript
<div class="box" data-index="1" data-list-name="yangyeyong" data-id="218160"></div>
<script>
    var box = document.querySelector('.box');
console.log(box.getAttribute('data-index'));     //取得的到H5自定义属性的值  1
console.log(box.getAttribute('index'));			 //取不到H5自定义属性的值  null

//说明H5自定义属性的值只能用通过data-开头才能获取。
</script>
```

#### dataset

**dataset 是一个集合里面存放了所有以data开头的自定义属性**

H5新增 element.dataset.index 或者 element.dataset[‘index’]     -----    ie11才开始支持

```html
<body>
  <div class="box" data-index="1" data-list-name="yangyeyong"></div>
  <script>
    var box = document.querySelector('.box');
    console.log(box.getAttribute('data-index'));
    // H5方法
    console.log(box.dataset);    //DOMStringMap {index: "1", listName: "yangyeyong"}
    console.log(box.dataset.listName);   //yangyeyong
  </script>
</body>
```



### 1.13 节点操作

**利用节点层级关系获取元素**

1.利用父子兄节点关系获取元素
2.逻辑性强， 但是兼容性稍差

**节点概述**

在DOM中，节点用`node`来表示。

一般地，节点至少拥有`nodeType（节点类型`、`nodeName（节点名称）`和`nodeValue（节点值）`这三个基本属性。

#### 区别节点类型

```javascript
1.元素节点------nodeType为1
2.属性节点------nodeType为2
3.文本节点------nodeType为3（文本节点包含文字、空格、换行等）
```

我们在实际开发中，节点操作主要操作的是**元素节点**

#### 父节点操作

```javascript
node.parentNode
```

得到的是离元素最近的父级节点(亲爸爸) 如果找不到父节点就返回为 null。

示例

```javascript
<body>  <div class="demo">    <div class="box">      <span class="erweima">×</span></div></div><script>        // 1. 父节点 parentNode        var erweima = document.querySelector('.erweima');// 得到的是离元素最近的父级节点(亲爸爸) 如果找不到父节点就返回为 nullconsole.log(erweima.parentNode);</script></body>
```

![image-20210330201843285](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330201843285.png)



#### 子节点操作

**1.parentNode.childNodes（标准）**

**注意：**返回值里面包含了所有的子节点，包括`元素节点`，`文本节点`等。

如果只想要获得里面的元素节点，则需要专门处理。 所以我们一般不提倡使用childNodes

```javascript
var ul = document. querySelector(‘ul’);for(var i = 0; i < ul.childNodes.length;i++) {  if (ul.childNodes[i].nodeType == 1) {    // ul.childNodes[i] 是元素节点    console.log(ul.childNodes[i]);  }}
```

**2.parentNode.children（非标准）**  ==这个是我们经常用的==

parentNode.children 是一个只读属性，==返回所有的子元素节点==。**它只返回子元素节点，其余节点不返回**（**这个是我们重点掌握的**）

虽然children 是一个非标准，但是得到了各个浏览器的支持，因此我们可以放心使用



两个API方法的示例：

```html
<ul>  <li>我是li</li>  <li>我是li</li>  <li>我是li</li>  <li>我是li</li></ul>var ul = document.querySelector('ul');var lis = ul.querySelectorAll('li');// 1. 子节点  childNodes 所有的子节点 包含 元素节点 文本节点等等console.log(ul.childNodes);console.log(ul.childNodes[0].nodeType);    //第一个子节点是文本节点，类型为3。console.log(ul.childNodes[1].nodeType);		 //第二个子节点是元素节点，类型为1。// 2. children 获取所有的子元素节点 也是我们实际开发常用的console.log(ul.children);</script>
```

#### 第一个子节点和最后一个子节点

第一个子节点

```javascript
parentNode.firstChild     //第一个子节点 不管是文本节点还是元素节点
```

```javascript
parentNode.firstElementChild    //返回第一个子元素节点 ie9才支持
```

最后一个子节点

```javascript
parentNode.lastChild      //最后一个节点  不管是文本节点和元素节点
```

```javascript
parentNode.lastElementChild     //返回最后一个子元素节点   ie9才支持
```

实际开发中，firstChild 和 lastChild 包含其他节点，操作不方便，而 firstElementChild 和

lastElementChild 又有兼容性问题，那么我们如何获取第一个子元素节点或最后一个子元素节点呢？

**解决方案：**

```javascript
// 3. 实际开发的写法  既没有兼容性问题又返回第一个子元素节点和最后一个元素节点console.log(ol.children[0]);console.log(ol.children[ol.children.length - 1]);
```



#### 兄弟节点（用的少了解就行）

```javascript
node.nextSibling    //nextSibling 返回当前元素的下一个兄弟元素节点，找不到则返回null。同样，也是包含所有的节点。
```

```javascript
node.previousSibling   //previousSibling 返回当前元素上一个兄弟元素节点，找不到则返回null。同样，也是包含所有的节点。
```

**以下两个方法要注意：有兼容性问题， IE9 以上才支持。**

```javascript
node.nextElementSibling   //nextElementSibling 返回当前元素下一个兄弟元素节点，找不到则返回null。
```

```javascript
node.previousElementSibling  //previousElementSibling 返回当前元素上一个兄弟节点，找不到则返回null。
```

**封装一个兄弟节点兼容性函数**

```javascript
function getNextElementSibling(element) {  var el = element;  while (el = el.nextSibling) {    if (el.nodeType === 1) {      return el;    }  }  return null;}
```



#### 创建节点

```javascript
document.createElement('tagName')示例var li = document.createElement('li');
```

document.createElement() 方法创建由 tagName 指定的 HTML 元素。因为这些元素原先不存在，

是根据我们的需求动态生成的，所以我们也称为==动态创建元素节点==。

#### 添加节点

```javascript
node.appendChild(child)var li = document.createElement('li');// 2. 添加节点 node.appendChild(child)  node 父级  child 是子级 后面追加元素  类似于数组中的pushvar ul = document.querySelector('ul');ul.appendChild(li);
```

```javascript
node.insertBefore(child, 指定元素)//添加节点 node.insertBefore(child, 指定元素);var lili = document.createElement('li');ul.insertBefore(lili, ul.children[0]);
```

#### 删除节点

```javascript
node.removeChild(child)
```

node.removeChild() 方法从 DOM 中删除一个子节点，返回删除的节点。

示例：

```javascript
<button>删除</button><ul>  <li>熊大</li><li>熊二</li><li>光头强</li></ul><script>  // 1.获取元素  var ul = document.querySelector('ul');var btn = document.querySelector('button');// 2. 删除元素  node.removeChild(child)// ul.removeChild(ul.children[0]);// 3. 点击按钮依次删除里面的孩子btn.onclick = function() {  if (ul.children.length == 0) {    this.disabled = true;  } else {    ul.removeChild(ul.children[0]);  }}</script>
```

#### 复制节点（克隆节点）

```javascript
node.cloneNode()
```

==注意！==

1.node.cloneNode(); 括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容*

2.node.cloneNode(true); 括号为true 深拷贝 复制标签复制里面的内容*

```html
<body>  <ul>    <li>1111</li>    <!-- <li>2</li> -->    <!-- <li>3</li> -->  </ul>  <script>    var ul = document.querySelector('ul');    // 1. node.cloneNode(); 括号为空或者里面是false 浅拷贝 只复制标签不复制里面的内容    // 2. node.cloneNode(true); 括号为true 深拷贝 复制标签复制里面的内容    var lili = ul.children[0].cloneNode(true);    ul.appendChild(lili);  </script></body>
```



### 1.14 三种创建元素方式区别

#### 1.document.write()  创建元素

如果页面文档加载完毕，再调用这句话会导致页面重新绘制。

例如

```html
<body>  <h1>1233</h1>  <p>我好好爱你呀</p>  <button>点击</button>  <script>    var btn = document.querySelector('button');    btn.onclick = function () {      document.write('<div>456</div>');    }  </script></body>//点击按钮就会导致页面重新绘制，结果就成了<div>456</div>
```

#### 2.innerHTML   创建元素

```javascript
<div class="inner"></div>// 2.innnerHTML  创建元素var inner = document.querySelector('.inner');inner.innerHTML = '<a href="#">链接</>';//拼接字符串形式       拼接字符串形式花时间最长var inner = document.querySelector('.inner');for (var i = 0; i <= 100; i++) {  inner.innerHTML += '<a href="#">百度</a>'}//先存放数组里面，然后再转换字符串      此种方法比createElement创建元素花的时间还少var inner = document.querySelector('.inner');var arr = [];for (var i = 0; i <= 100; i++) {  arr.push('<a href="#">百度</a>');}inner.innerHTML = arr.join('');
```

#### 3.document.createElement()  创建元素

```javascript
<div class="create">1</div>var create = document.querySelector('.create');for (var i = 0; i <= 100; i++) {  var a = document.createElement('a');  a.innerHTML = '链接';  create.appendChild(a);}document.write是直接将内容写入页面的内容流，但是文档流执行完毕，则它会导致页面全部重绘innerHTML是将内容写入某个 DOM 节点，不会导致页面全部重绘innerHTML创建多个元素效率更高(不要拼接字符串，采取数组形式拼接），结构稍微复杂createElement(）创建多个元素效率稍低一点点，但是结构更清晰
```

**==区别==**

1.document.write是直接将内容写入页面的内容流，但是文档流执行完毕，则它会导致页面全部重。
2.innerHTML是将内容写入某个 DOM 节点，不会导致页面全部重绘。
3**.innerHTML创建多个元素效率更高(不要拼接字符串，采取数组形式拼接），结构稍微复杂。**
4.createElement(）创建多个元素效率稍低一点点，但是结构更清晰。

==**总结**==

**不同浏览器下，innerHTML效率要比createElement高**





## 二、事件高级

### 2.1 注册事件（绑定事件）

------

给元素添加事件，称为`注册事件`或者`绑定事件`。

注册事件有两种方式：`传统方式`和`方法监听注册方式`。

#### 传统方式

```html
利用on开头的事件，比如onclick<button onclick="alert(hi~)"></button>或者<script>	var btn=document.querySelector('button');  btn.onclick=function(){    alert('hi~');  }</script>
```

==特点：==注册事件的**唯一性**

`同一个元素同一个事件`只能设置一个处理函数，最后注册的处理的函数会覆盖前面所注册的处理函数。

#### 方法监听注册方式

```javascript
<button>传统注册事件</button><button>方法监听注册事件</button><button>ie9 attachEvent</button><script>  var btns = document.querySelectorAll('button');btns[1].addEventListener('click', function() {  alert(22);})btns[1].addEventListener('click', function() {  alert(33);})// 3. attachEvent ie9以前的版本支持btns[2].attachEvent('onclick', function() {  alert(11);})</script>
```

1.W3C标准推荐方式

2.addEventListener()它是一个方法

3.IE9之前的IE不支持此方法，可使用`attachEvent()`代替。

4.==特点：==同一个元素同一个事件可以注册多个监听器。

5.按注册顺序依次执行。



>addEventListener事件监听方式

```javascript
eventTarget.addEventListener(type, listener[, useCapture])
```

eventTarget.addEventListener()方法将指定的监听器注册到 eventTarget（目标对象）上，当该对

象触发指定的事件时，就会执行事件处理函数。

==接收的三个参数分别为：==

1.`type`:事件类型字符串，比如click、mouseover，**注意在监听事件中不用带on**。

2.`listener`:事件处理函数，事件发生时，会调用该监听函数。

3.`useCapture`:可选参数，是一个布尔值，默认是false。**参数如果是true，表示在事件`捕获阶段`调用事件处理程序；如果是false，表示在`事件冒泡`阶段调用事件处理程序。**

```html
<button>传统注册事件</button>
<button>方法监听注册事件</button>
<button>ie9 attachEvent</button>
<script>
  var btns = document.querySelectorAll('button');
  btns[0].addEventListener('click', function () {
    alert('按钮孩子1');
  })
  btns[1].addEventListener('click', function () {
    alert('按钮孩子2');
  })
  btns[2].addEventListener('mouseover', function () {
    console.log('按钮孩子3');
  })
</script>
```

==注意==：**1.因为事件类型是字符串，所以必定要加`引号`**而且不带on

​			**2.同一个元素，同一个事件可以添加多个监听器（事件处理程序）**

注意事项如下

```html
<button>传统注册事件</button>
<button>方法监听注册事件</button>
<button>ie9 attachEvent</button>
<script>
  btns[0].addEventListener('click', function () {    //事件监听1
    alert('按钮孩子1');
  })
  btns[0].addEventListener('mousemove', function () {    //事件监听2
    console.log('同一个元素对象，不同的事件');
  })
  //结果同一个元素对象既可以触发事件监听1和事件监听2
  
  
</script>
```



> attachEvent事件监听方式     （了解就行，如果要涉及到兼容IE，可以看下）

==注意==：IE8及早期的版本支持     

```javascript
eventTarget.attachEvent(eventNameWithOn, callback)
```

**它是后绑定先执行，执行顺序和addEventListener()相反**

eventTarget.attachEvent()方法将指定的监听器注册到 eventTarget（目标对象） 上，当该对象触

发指定的事件时，指定的回调函数就会被执行。

==接收的两个参数分别为：==

1.eventNameWithOn:事件类型字符串，比如onclick,onmouseover,**这里要带on**。

2.callback:事件处理函数，当目标触发事件时`回调函数`被调用

```javascript
btns[2].attachEvent('onclick', function() {  alert(11);})//报错，ie9以前的版本才支持
```



> 注册事件兼容性解决方案

兼容性处理的原则： 首先照顾大多数浏览器，再处理特殊浏览器

```javascript
<button>传统注册事件</button><button>方法监听注册事件</button><button>ie9 attachEvent</button>//自己封装一个具有兼容浏览器功能的监听函数function addEventListener(element, eventName, fn) {  // 判断当前浏览器是否支持 addEventListener 方法  if (element.addEventListener) {    element.addEventListener(eventName, fn); // 第三个参数 默认是false  } else if (element.attachEvent) {    element.attachEvent('on' + eventName, fn);  } else {    // 相当于 element.onclick = fn;    element['on' + eventName] = fn;  }}//调用var btns = document.querySelectorAll('button');addEventListener(btns[1], 'click', function () {  alert('1111');});
```



### 2.2 删除事件（解绑事件）

#### 传统解绑注册方式

```javascript
eventTarget.onclick=null;      //eventTarget:事件对象
```

示例：

```javascript
<div>1</div>
<div>2</div>
<div>3</div>
var divs = document.querySelectorAll('div');
divs[0].onclick = function() {
  alert(11);
  // 1. 传统方式删除事件
  divs[0].onclick = null;
}
```

#### 方法监听解绑注册方式

```javascript
① eventTarget.removeEventListener(type, listener[, useCapture]);
② eventTarget.detachEvent(eventNameWithOn, callback);   //IE6~10
```

==注意：**EventTarget.detachEvent()；该特性是非标准的，请尽量不要在生产环境中使用它！**==

各删除事件示例：

```javascript
// 2. removeEventListener 删除事件
divs[1].addEventListener('click', fn) // 里面的fn 不需要调用加小括号

function fn() {
  alert(22);
  divs[1].removeEventListener('click', fn);
}
```

```javascript
// 3. detachEventdivs[2].attachEvent('onclick', fn1);function fn1() {  alert(33);  divs[2].detachEvent('onclick', fn1);}
```

#### 删除事件兼容性解决方案

```javascript
function removeEventListener(element, eventName, fn) {
  // 判断当前浏览器是否支持 removeEventListener 方法
  if (element.removeEventListener) {
    element.removeEventListener(eventName, fn); // 第三个参数 默认是false
  } else if (element.detachEvent) {
    element.detachEvent('on' + eventName, fn);
  } else {
    element['on' + eventName] = null;
  }
}
```



### 2.3 DOM事件流

事件流描述的是从页面中接收事件的顺序。

事件发生时会在元素节点之间按照特定的顺序传播，这个`传播过程`即 `DOM事件流`。



比如我们给一个div 注册了点击事件：

DOM 事件流分为3个阶段：`捕获阶段`、`当前目标阶段`、`冒泡阶段`

![image-20210329233437512](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210329233437512.png)

事件冒泡： IE 最早提出，事件开始时由最具体的元素接收，然后逐级向上传播到到 DOM 最顶层节点的过程。

事件捕获： 网景最早提出，由 DOM 最顶层节点开始，然后逐级向下传播到到最具体的元素接收的过程。

> **DOM事件流理解**

![image-20210329233634632](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210329233634632.png)

==注意==：

1.JS 代码中只能执行捕获或者冒泡其中的一个阶段。

2.onclick 和 attachEvent 只能得到冒泡阶段。

3.addEventListener(type, listener[, useCapture])第三个参数如果是 true，表示在事件捕获阶段调用事件处理程序；如果是 false（不写默认就是false），表示在事件冒泡阶段调用事件处理程序。

4.**实际开发中我们很少使用事件捕获，我们更关注事件冒泡。**

5.**有些事件是没有冒泡的，比如 onblur、onfocus、onmouseenter、onmouseleave**

6.**事件冒泡有时候会带来麻烦，有时候又会帮助很巧妙的做某些事件，我们后面讲解。**

>第三点示例

```html
//当addEventListener第三个参数是true，表示在事件捕获阶段调用函数<div class="father">  <div class="son">son盒子</div></div><script>  //捕获阶段 如果addEventListener 第三个参数是 true 那么则处于捕获阶段  document -> html -> body -> father -> son  var son = document.querySelector('.son');  son.addEventListener('click', function() {    alert('son');  }, true);  var father = document.querySelector('.father');  father.addEventListener('click', function() {    alert('father');  }, true);</script>
```

结果是，先弹出father,后弹出son,说明在事件捕获阶段调用函数。

当参数为false时，则在事件冒泡阶段调用。示例同理。*son -> father ->body -> html -> document*



### 2.4 事件对象

------

官方解释：event 对象代表事件的状态，比如键盘按键的状态、鼠标的位置、鼠标按钮的状态。

简单理解：事件发生后，`跟事件相关的一系列信息数据的集合`都放到这个对象里面，这个对象就是`事件对象event`，它有很多属性和方法。

```javascript
 eventTarget.onclick = function(event) {} 
 eventTarget.addEventListener('click', function(event) {}）
 // 这个 event 就是事件对象，我们还喜欢的写成 e 或者 evt
```

比如： 

 1.谁绑定了这个事件。

 2.鼠标触发事件的话，会得到鼠标的相关信息，如鼠标位置。

 3.键盘触发事件的话，会得到键盘的相关信息，如按了哪个键。



**事件对象的使用语法**

```javascript
eventTarget.onclick = function(event) {
  // 这个 event 就是事件对象，我们还喜欢的写成 e 或者 evt 
}
eventTarget.addEventListener('click', function(event) {
  // 这个 event 就是事件对象，我们还喜欢的写成 e 或者 evt 
}）
```

这个 event 是个形参，系统帮我们设定为事件对象，不需要传递实参过去。

当我们注册事件时， event 对象就会被系统自动创建，并依次传递给事件监听器（事件处理函数）。

示例：

```javascript
// 事件对象
var div = document.querySelector('div');
div.onclick = function(e) {
  // console.log(e);
  console.log(window.event);
  // e = e || window.event;
  console.log(e);
//这时会打印出该触发事件的事件对象相关信息数据。如下图
}
```

![image-20210330110816014](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330110816014.png)



> **总结**

1.event 就是一个事件对象 写到我们侦听函数的 小括号里面 当形参来看

2.事件对象只有有了事件才会存在，它是系统给我们自动创建的，不需要我们传递参数

3.事件对象 是 我们事件的一系列相关数据的集合 跟事件相关的 比如鼠标点击里面就包含了鼠标的相关信息，鼠标坐标啊，如果是键盘事件里面就包含的键盘事件的信息 比如 判断用户按下了那个键

4.这个事件对象我们可以自己命名 比如 event 、 evt、 e

**5.事件对象也有兼容性问题 ie678 通过 window.event 兼容性的写法 e = e || window.event;**



>**事件对象的常见属性和方法**

`e.target` 返回的是触发事件的对象（元素）      this 返回的是绑定事件的对象（元素）。

==区别==： e.target 点击了那个元素，就返回那个元素。

​			 this 那个元素绑定了这个点击事件，那么就返回谁。

示例：

```javascript
var table = document.querySelector('table');console.log(table);table.addEventListener('click', function (e) {  console.log(this);   //放回的是绑定事件的元素对象table。  console.log(e.target);   //返回的是点击那个的元素。})  //如下图所示。
```

![image-20210330112618564](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330112618564.png)



**事件对象的常见属性和方法：**

![image-20210330112810074](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330112810074.png)



### 2.5 事件对象阻止默认行为

#### **让链接不跳转方法**

```javascript
a标签中的href属性值填'javascript:;'    //点击后网页地址后面不会出现#
利用事件对象方法:e.preventDefault();  //阻止默认事件。
```

示例

```javascript
var lianjie = document.querySelector('a');
lianjie.addEventListener('click', function (e) {
  e.preventDefault();
});    //这样链接不跳转
```

#### **按钮失效**

```javascript
//也是利用e.preventDefault();

<form action="http://www.baidu.com">
  <input type="submit" value="提交" name="sub">
    </form>

var btnss = document.querySelector('input');
btnss.addEventListener('click', function (e) {
  e.preventDefault();
})
```

有时候利用return false也能阻止默认行为

但特点是：**return 后面的代码不执行了， 而且只限于传统的注册方式**

```javascript
lianjie.onclick = function () {
  return false;
  alert('1'); //这段不执行了。
}
```



### 2.6 事件冒泡

事件的冒泡指的是事件向上传导，当后代元素上的事件被触发时，将会导致其祖先元素上的==同类事件==也会触发。

事件的冒泡大部分情况下都是有益的，如果需要取消冒泡，则需要使用事件对象来取消

### 2.7 阻止事件冒泡

**阻止事件冒泡的两种方式**

标准写法：利用事件对象里面的 stopPropagation()方法

```javascript
e.stopPropagation();
```

非标准写法：IE 6-8 利用事件对象 cancelBubble 属性

```javascript
e.cancelBubble = true;
```

示例：

```html
<body>
  <div class="father">
    <div class="son">son儿子</div>
  </div>
  <script>
    // 常见事件对象的属性和方法
    // 阻止冒泡  dom 推荐的标准 stopPropagation() 
    var son = document.querySelector('.son');
    son.addEventListener('click', function(e) {
      alert('son');
      e.stopPropagation(); // stop 停止  Propagation 传播
      e.cancelBubble = true; // 非标准 cancel 取消 bubble 泡泡
    }, false);

    var father = document.querySelector('.father');
    father.addEventListener('click', function() {
      alert('father');
    }, false);
    document.addEventListener('click', function() {
      alert('document');
    })
  </script>
</body>
```

**阻止事件冒泡的兼容性解决方案**

```javascript
if(e && e.stopPropagation){
  e.stopPropagation();
}else{
  window.event.cancelBubble = true;
}
```



### 2.8 事件委托

**事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点**

```html
<body>  <ul>    <li>知否知否，点我应有弹框在手！</li>    <li>知否知否，点我应有弹框在手！</li>    <li>知否知否，点我应有弹框在手！</li>    <li>知否知否，点我应有弹框在手！</li>    <li>知否知否，点我应有弹框在手！</li>  </ul>  <script>    // 事件委托的核心原理：给父节点添加侦听器， 利用事件冒泡影响每一个子节点    var ul = document.querySelector('ul');    ul.addEventListener('click', function (e) {      alert('知否知否，点我应有弹框在手！');      console.log(e.target);      // e.target 这个可以得到我们点击的对象      e.target.style.backgroundColor = 'pink';    })  </script></body>
```



### 2.9 常见的鼠标事件

![image-20210330154510057](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330154510057.png)

**禁止鼠标右键菜单**

```javascript
document.addEventListener('contextmenu', function (e) {
  e.preventDefault();
})
```

**禁止鼠标选中（selectstart 开始选中）**

```javascript
document.addEventListener('selectstart', function(e) {
  e.preventDefault();
})
```

**鼠标事件对象**

![image-20210330155947431](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330155947431.png)

```javascript
document.addEventListener('click', function (e) {
  console.log(e);
  // 1. client 鼠标在可视区的x和y坐标
  console.log(e.clientX);
  console.log(e.clientY);
  console.log('---------------------');

  // 2. page 鼠标在页面文档的x和y坐标
  console.log(e.pageX);
  console.log(e.pageY);
  console.log('---------------------');

  // 3. screen 鼠标在电脑屏幕的x和y坐标
  console.log(e.screenX);
  console.log(e.screenY);
})
```

### 2.10 物体跟随鼠标事件对象移动的代码及方案

```javascript
var pic = document.querySelector('img');
document.addEventListener('mousemove', function(e) {
  var x = e.pageX;
  var y = e.pageY;
  pic.style.top = y - 40 + 'px';
  pic.style.left = x - 50 + 'px';
})
```



### 2.11 常见的键盘事件

![image-20210330161700593](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330161700593.png)

==注意==：

**1.** **如果使用**`addEventListener` **不需要加** **on**

2.onkeypress 和前面2个的区别是，它不识别功能键，比如左右箭头，shift 等。

3.三个事件的执行顺序是： keydown -- keypress --- keyup

>键盘事件对象

![image-20210330161919970](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210330161919970.png)

**注意：** onkeydown 和 onkeyup 不区分字母大小写，onkeypress 区分字母大小写。

在我们实际开发中，我们更多的使用keydown和keyup， 它能识别所有的键（包括功能键）

Keypress 不识别功能键，但是keyCode属性能区分大小写，返回不同的ASCII值





## 三、BOM 浏览器对象模型

### 3.1 BOM概述

BOM（Browser Object Model）即`浏览器对象模型`，它提供了独立于内容而与`浏览器窗口进行交互的对象`，其核心对象是 **window**。

BOM 由一系列相关的对象构成，并且每个对象都提供了很多方法与属性。

BOM 缺乏标准，JavaScript 语法的标准化组织是 ECMA，DOM 的标准化组织是 W3C，BOM 最初是Netscape 浏览器标准的一部分。

#### BOM和DOM区别

![image-20210401131941152](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401131941152.png)

#### BOM构成

![image-20210401132102233](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401132102233.png)

`window 对象是浏览器的顶级对象`，它具有双重角色。

1.它是 JS 访问浏览器窗口的一个接口。

2.它是一个全局对象。定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法。

在调用的时候可以省略 window，前面学习的对话框都属于 window 对象方法，如 alert()、prompt() 等。

==注意：==window下的一个特殊属性 `window.name`



### 3.2 window对象常用一些的属性

[`Window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window) 接口的`devicePixelRatio`返回当前显示设备的*物理像素*分辨率与*CSS像素*分辨率之比。

 简单来说，它告诉浏览器应使用多少屏幕实际像素来绘制单个CSS像素。



不同显示设备的`devicePixelRatio`不同。

![image-20210426165606365](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210426165606365.png)



### 3.3 window对象的常见事件

#### 窗口加载事件

第一种：

```javascript
window.onload = function(){}
或者
window.addEventListener("load",function(){});
```

window.onload 是窗口 (页面）加载事件,当**文档内容完全加载完成**会触发该事件(包括图像、脚本文件、CSS 文件等), 就调用的处理函数。

==注意：==

1.有了 window.onload 就可以把 JS 代码写到页面元素的上方，因为 onload 是等页面内容全部加载完毕， 再去执行处理函数。

2.window.onload 传统注册事件方式 只能写一次，如果有多个，会以最后一个 window.onload 为准。 

3.如果使用 addEventListener 则没有限制。

第二种：

```javascript
document.addEventListener('DOMContentLoaded',function(){})
```

DOMContentLoaded 是DOM 加载完毕，不包含图片 flash css 等就可以执行 加载速度比 load更快一些

**两者区别：**

load 等页面内容全部加载完毕，包含页面dom元素 图片 flash css 等等

DOMContentLoaded 是DOM 加载完毕，不包含图片 falsh css 等就可以执行 加载速度比 load更快一些



#### 调整窗口大小事件

```javascript
window.onresize = function(){}window.addEventListener("resize",function(){});
```

window.onresize 是调整窗口大小加载事件, 当触发时就调用的处理函数。

![image-20210426161257567](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210426161257567.png)



#### innerWidth和innerHeight

界面的只读**`innerHeight`**属性 [`Window`](https://developer.mozilla.org/en-US/docs/Web/API/Window)口的内部高度（以像素为单位,包括**水平滚动条的高度**(如果存在)的值，`innerHeight`是从窗口的[布局视口](https://developer.mozilla.org/en-US/docs/Glossary/Layout_viewport)的高度获取的 。

同理，可以使用该[`innerWidth`](https://developer.mozilla.org/en-US/docs/Web/API/Window/innerWidth)属性获得宽度 。

> 注意：

1. 只要窗口大小发生像素变化，就会触发这个事件。
2. **我们经常利用这个事件完成响应式布局。 window.innerWidth 当前屏幕的宽度。**

示例：

```js
var div = document.querySelector('div');
console.log(window.innerHeight);
window.addEventListener('resize', function () {  console.log('大小要调整了！');  
if (window.innerWidth < 800) {    div.style.display = 'none';  } 
else {    div.style.width = '300px';    div.style.height = '300px';  }})
```



### 3.4 定时器

定时器有两种：1.`setTimeout()`、2.`setInternal()`。

#### setTimeout() 定时器

```javascript
 window.setTimeout(调用函数, [延迟的毫秒数]);
```

setTimeout() 方法用于设置一个定时器，该定时器在定时器到期后执行调用函数。

==注意：==

语法规范： window.setTimeout(调用函数, 延时时间);

1.这个window在调用的时候可以省略。

2.这个延时时间单位是毫秒 但是可以省略，如果省略默认的是0。

3.这个调用函数**可以直接写函数** 还可以写 **函数名** 还有一个写法 '函数名()'（第三种不建议）

4.页面中可能有很多的定时器，我们经常给定时器加标识符 （名字)。

#### 停止setTimeout()定时器

```javascript
window.clearTimeout(timeoutID)

示例：
var btn = document.querySelector('button');
var timer = setTimeout(function() {
  console.log('爆炸了');

}, 5000);
btn.addEventListener('click', function() {
  clearTimeout(timer);
})
```

clearTimeout()方法取消了先前通过调用 setTimeout() 建立的定时器。

**注意：**

1.window 可以省略。

2.里面的参数就是定时器的标识符 。

#### 进一步对回调函数的理解

**setTimeout() 这个调用函数我们也称为回调函数 callback**

简单理解： 回调，就是回头调用的意思。上一件事干完，再`回头`再`调用这个函数`。

以前我们讲的 `element.onclick = function(){} `或者` element.addEventListener(“click”, fn); `里面的 函数也是回调函数。

#### setInterval() 定时器

```javascript
 window.setInterval(回调函数, [间隔的毫秒数]);
```

setInterval() 方法重复调用一个函数，每隔这个时间，就去调用一次回调函数。(**即可以多次触发执行**)

#### 停止 setInterval() 定时器

```javascript
window.clearInterval(intervalID);
```

clearInterval()方法取消了先前通过调用 setInterval()建立的定时器。	

==注意：==1.如果参数是一个有效的定时器的标识，则停止对应的定时器

​			2.如果参数不是一个有效的标识，则什么都不做	



### 3.5 JS 执行机制

JavaScript 语言的一大特点就是==单线程==，也就是说，**同一个时间只能做一件事**。

单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。

这样所导致的问题是： 如果JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉

```javascript
第一个问题
console.log(1);
setTimeout(function () {
  console.log(3);
}, 1000);
console.log(2);
//结果：1 2 3

第二个问题
console.log(1);
setTimeout(function () {
  console.log(3);
}, 0);
console.log(2);
//结果：1 2 3

```

为什么会出现这种问题呢？原因是js有了`同步`和`异步`的功能

#### 同步和异步

本质区别：**这条流水线上各个流程的执行顺序不同。**

**同步任务**

同步任务都在主线程上执行，形成一个==执行栈==

![image-20210401163322395](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401163322395.png)

**异步任务**

JS 的异步是通过回调函数实现的。

一般而言，异步任务有以下三种类型: 

1、普通事件，如 click、resize 等 

2、资源加载，如 load、error 等 

3、定时器，包括 setInterval、setTimeout 等

异步任务相关==回调函数==添加到**任务队列**中（任务队列也称为消息队列）。

![image-20210401163518599](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401163518599.png)

#### 同步和异步顺序

1.先执行==执行栈中的同步任务==。

2.异步任务（回调函数）放入任务队列中。

3.一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取==任务队列==中的异步任务，于是被读取的异步任

务结束等待状态，进入执行栈，开始执行。

![image-20210401164009629](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401164009629.png)

```javascript
console.log(1);
document.onclick = function() {
  console.log('click');
}
console.log(2);
setTimeout(function() {
  console.log(3)
}, 3000)
//结果 1 2 click 3 或者 1 2 3 click (click取决于你是在3秒钟点的还是3秒后点的)
```

由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为==事件循环（ event loop）==。

![image-20210401164201135](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401164201135.png)

### 3.6 Location对象

window 对象给我们提供了一个` location 属性`用于`获取或设置窗体的 URL`，并且可以用于`解析 URL` 。 因为这个属性返回的是一个对象，所以我们将这个属性也称为 `location 对象`。

#### URL

`统一资源定位符 (Uniform Resource Locator, URL)` 是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的 URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

URL 的一般语法格式为：

![image-20210401165748848](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401165748848.png)

![image-20210401165808987](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401165808987.png)

#### Location对象的属性

![image-20210401170046388](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401170046388.png)

示例：

```html
我先在一个页面创建表单
<form action="./测试.html">
  <input type="text" name="uname">
  <input type="submit" value="提交">
</form>

在 测试.html页面 接收来自那个页面的参数
localtion.search    //?uname=13907025547 即得到得是query部分
```

#### location 对象的方法

![image-20210401170838038](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401170838038.png)

示例：

```javascript
var btn = document.querySelector('button');
btn.addEventListener('click', function() {
  // 记录浏览历史，所以可以实现后退功能
  // location.assign('http://www.itcast.cn');
  // 不记录浏览历史，所以不可以实现后退功能
  location.replace('http://www.itcast.cn');
  // location.reload(true);
})
```



### 3.7 navigator 对象(了解 ，一般后台人员处理)

navigator 对象包含有关浏览器的信息，它有很多属性，我们最常用的是 userAgent，该属性可以返回由客户机发送服务器的 **user-agent 头部的值**

```javascript
if((navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|                              Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i))) {  window.location.href = ""; //手机} else {  window.location.href = ""; //电脑}
```



### 3.8 history 对象

window 对象给我们提供了一个 history 对象，与浏览器历史记录进行交互。该对象包含用户（在浏览器窗口中）访问过的 URL。

![image-20210401171928484](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401171928484.png)

history 对象一般在实际开发中比较少用，但是会在一些 OA 办公系统中见到。



## 四、PC端网页特效

### 4.1 元素偏移量offset系列

offset 翻译过来就是偏移量， 我们使用 offset 系列相关属性可以==动态的得到该元素的位置（偏移）==、大小等。

1.获得元素距离带有定位父元素的位置

2.获得元素自身的大小（宽度高度）

3.注意： 返回的数值都不带单位

#### offset系列常用属性

![image-20210401173436898](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401175742338.png)

![image-20210401175742338](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401175742338.png)

示例：

```javascript
window.addEventListener('load', function () {
  var father = document.querySelector('.father');
  var son = document.querySelector('.son');
  var i = son.offsetParent;
  console.log(i);  //如果当前元素对象得父级元素有定位则返回父级元素，没有则返回body。
  console.log(son.offsetTop);   //300
  console.log(son.offsetLeft);   //300
  console.log(son.offsetWidth);//返回自身内容+padding+border。
  console.log(son.offsetHeight);//返回自身内容+padding+border。
})
```

#### offsetParent属性和node.parentNode区别

```javascript
console.log(son.offsetParent); // 返回带有定位的父亲 否则返回的是body 
console.log(son.parentNode); // 返回父亲 是最近一级的父亲 亲爸爸 不管父亲有没有定位
```

####  offset 与 style 区别

![image-20210401175633992](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401175633992.png)

>封装一个方法，获取当前元素距离body的左(上偏移)，不论其父参照物是谁

```js
function offset(element) {
  let l = element.offsetLeft,
      t = element.offsetTop,
      p = element.offsetParent;
  //p&&p.tagName==>意思是p的参照物存在同时p的标签命
  //如果是BODY则不执行while循环，直接返回p.offsetLeft和p.offsetTop
  while (p && p.tagName !== 'BODY') {
    // /MSIE 8/==>即IE8浏览器  ==>正则表达式
    if (!/MSIE 8/.test(navigator.userAgent)) {
      l += p.clientLeft;
      t += p.clientTop;
    }
    l += p.offsetLeft;
    t += p.offsetTop;
    p = p.offsetParent;
  }
  return {
    top: t,
    left: l
  };
}
```

图示：

![image-20210503223914093](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210503223914093.png)

#### 获取鼠标在盒子中的坐标

1.先得到鼠标在页面中的坐标（e.pageX, e.pageY）

2.其次得到盒子在页面中的距离 ( box.offsetLeft, box.offsetTop)

3.用鼠标距离页面的坐标减去盒子在页面中的距离，得到 鼠标在盒子内的坐标（e.pageX-box.offsetLeft,e.pageY-box.offsetTop）

4.如果想要移动一下鼠标，就要获取最新的坐标，使用鼠标移动事件 mousemove



### 4.2 元素可视区 client 系列

client 翻译过来就是客户端，我们使用 client 系列的相关属性来获取元素可视区的相关信息。

通过 client 系列的相关属性可以==动态的得到该元素的边框大小、元素大小等==。

![image-20210401181651403](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401181651403.png)

![image-20210401181821732](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401181821732.png)

示例：

```javascript
.son {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-100px, -100px);
  width: 200px;
  height: 200px;
  padding: 10px;
  border: 10px solid #333;
  background-color: pink;
} 

<div class="father">
  <div class="son"></div>
</div>
var son = document.querySelector('.son');
console.log(son.clientTop);  //返回该元素的上边框的大小 10
console.log(son.clientLeft);  //返回该元素的左边框的大小 10
console.log(son.clientWidth);  //返回不包括边框的元素宽大小 220  padding+content
console.log(son.clientHeight);  //返回不包括边框的元素高大小 220  padding+content
```



### 4.3 元素滚动 scroll 系列

`scroll` 翻译过来就是滚动的，我们使用 scroll 系列的相关属性可以==动态的得到该元素的大小、滚动距离等==。

![image-20210401200247543](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401200247543.png)

![image-20210401200325352](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401200325352.png)

**页面被卷去的头部**

如果浏览器的高（或宽）度不足以显示整个页面时，会自动出现滚动条。当滚动条向下滚动时，页面上面被隐藏掉的高度，我们就称为页面被卷去的头部。滚动条在滚动时会触发 onscroll 事件。

示例：

```javascript
// scroll 系列
var div = document.querySelector('div');
console.log(div.scrollHeight);     //返回整个滑动的自身高度  padding+content
console.log(div.clientHeight);    //返回可视区的自身高度 padding+content
// scroll滚动事件当我们滚动条发生变化会触发的事件
div.addEventListener('scroll', function() {
  console.log(div.scrollTop);   //返回被卷去的上侧的距离.
})
```



#### 页面被卷去的头部兼容性解决方案

1.声明了 DTD，使用 **document.documentElement.scrollTop**

​		DTD:（html骨架的第一行）![image-20210503162959596](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210503162959596.png)

2.未声明 DTD（即有些编辑器生成HTML骨架没有这个或者人为手动删除），使用 **document.body.scrollTop**

3.新方法` window.pageYOffset` 和 `window.pageXOffset`，IE9 开始支持

```javascript
function getScroll() {
  return {
    left: window.pageXOffset || document.documentElement.scrollLeft || document.body.scrollLeft||0,
    top: window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0
  };
} 
使用的时候 getScroll().left


//document.documentElement===>相当于获取HTML
//document.body==>相当于获取body
```



### 4.4 offset、client和scroll总结

![image-20210401201940503](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401201940503.png)

  1.`offsetWidth`:content+paddding+border;

​     `clientWidth`:content+padding;

​     `scrollWidth`:content+padding;

  2.clientWidth相对于客户端，不能检测滑动时的总宽，而scrollWidth可以。

![image-20210401202431595](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210401202431595.png)

> 他们的主要用法

1.offset系列 经常用于获得元素位置 `offsetLeft` `offsetTop`
2.client 经常用于获取元素大小` clientWidth` `clientHeight`
3.scroll 经常用于获取滚动距离 `scrollTop` `scrollLeft` 
4.注意`页面滚动`的距离通过 `window.pageXOffset` 获得

**5.offset、client、scroll属性都是只读的，不能修改。**



### 4.5getBoundingClientRect()

`Element.getBoundingClientRect()` 方法返回`元素的大小`及其`相对于浏览器可视化窗口的位置`。

如果是标准盒子模型，元素的尺寸等于`width/height` + `padding` + `border`的总和。如果`box-sizing: border-box`，元素的的尺寸等于 `width/height`。

图示：

![image-20210503203302950](https://cdn.jsdelivr.net/gh/threey333/Picture/js-dom/image-20210503203302950.png)

示例：

```javascript
console.log('box元素对象上偏移:' + box.offsetTop); //box元素对象上偏移:300
console.log('box元素对象窗口大小:' + box.clientWidth); //box元素对象窗口大小:420
console.log('box元素对象窗口大小:' + box.clientHeight);//box元素对象窗口大小:420

console.log('--------');
console.log('利用getBoundingClientRect()获得:' + box.getBoundingClientRect().bottom + ',包含(content+padding+border)');
//利用getBoundingClientRect()获得:740,包含(content+padding+border)

console.log('利用getBoundingClientRect()获得:' + box.getBoundingClientRect().top + ',包含(content+padding+border)');
//利用getBoundingClientRect()获得:300,包含(content+padding+border)

console.log(box.getBoundingClientRect());


```

### 4.6 mouseenter和mouseover的区别

 mouseover 鼠标经过自身盒子会触发，经过子盒子还会触发。 **mouseenter 只会经过自身盒子触发**

原因:为mouseenter不会冒泡，而mouseover会冒泡。

跟mouseenter搭配 鼠标离开 mouseleave 同样不会冒泡



## 五、动画函数封装

### 5.1 动画实现原理

**核心原理：**通过==定时器etInterval()==不断移动盒子位置。

示例

```javascript
//实现步骤：
//1. 获得盒子当前位置
//2. 让盒子在当前位置加上1个移动距离
//3. 利用定时器不断重复这个操作
//4. 加一个结束定时器的条件
//5. 注意此元素需要添加定位，才能使用element.style.left


.father {
  position: absolute;
  left: 0;
  top: 0;
  width: 200px;
  height: 200px;
  background-color: skyblue;
}

// //动画实现原理
window.addEventListener('load', function () {
  var fatherLocation = document.querySelector('.father');
  var timer = setInterval(function () {
    var x = fatherLocation.offsetLeft; //每次都会变
    // var y = fatherLocation.offsetTop;
    // console.log(x);
    console.log(fatherLocation.clientWidth);
    if (x >= innerWidth - fatherLocation.clientWidth) {
      console.log(fatherLocation.offsetLeft);
      clearInterval(timer);
    }
    fatherLocation.style.left = x + 10 + 'px';
    // fatherLocation.style.top = y + 10 + 'px';
  }, 10);
```



### 5.2 动画函数封装

```javascript
var fatherLocation = document.querySelector('.father');
//封装动画函数
//obj--目标对象 target--目标位置
function animate(obj, target, callback) {
  //点击之前先清空监听器。
  clearInterval(obj.timer);
  obj.timer = setInterval(function () {
    var x = obj.offsetLeft;
    var step = (target - x) / 10;
    step = step > 0 ? Math.ceil(step) : Math.floor(step);
    if (x == target) {
      clearInterval(obj.timer);
      if (callback) {
        callback();
      }
    } else {
      obj.style.left = x + step + 'px';
    }
  }, 30)
}
```

### 5.3 动画函数给不同元素记录不同定时器

核心原理：利用 JS 是一门动态语言，可以很方便的给当前对象添加属性。

用如上例子代码，给定时器的命名改成----obj.属性名

```javascript
function animate(obj, target) {
 obj.timer = setInterval(function () {   //timer改成obj.timer
    var x = obj.offsetLeft; //每次都会变
    // console.log(x);
    console.log(fatherLocation.clientWidth);
    if (x == target) {
      clearInterval(timer);kaiqkai
    }else{
      obj.style.left = x + 1 + 'px';
    }
  }, 10);
}

```

但是有个BUG，就是给个按钮的时候，点击触发定时器，会无限叠加，这时我们就要**在开启定时器前清除定时器**。

```javascript
window.addEventListener('load', function () {
  var fatherLocation = document.querySelector('.father');
  var yyy = document.querySelector('.yyy');
  var btn = document.querySelector('button');
  //封装函数
  //obj--目标对象 target--目标位置
  function animate(obj, target) {
    // 当我们不断的点击按钮，这个元素的速度会越来越快，因为开启了太多的定时器
    // 解决方案就是 让我们元素只有一个定时器执行
    // 先清除以前的定时器，只保留当前的一个定时器执行
    clearInterval(obj.timer);
    obj.timer = setInterval(function () {
      x = obj.offsetLeft;
      // var y = fatherLocation.offsetTop;
      // console.log(x);
      if (x == target) {
        clearInterval(obj.timer);
      } else {
        obj.style.left = x + 1 + 'px';
      }
    }, 15);
  }

  animate(fatherLocation, (innerWidth - fatherLocation.clientWidth));
  btn.addEventListener('click', function () {
    console.log('1111');
    animate(yyy, 1000);
  });
})
```

但是又有一个问题，就是每次移动的距离是固定的，这个==步长==能否改变为动态呢？有，使用==缓动效果原理==

#### 缓动效果原理

缓动动画就是让元素运动速度有所变化，最常见的是让速度慢慢停下来

步长公式：`(目标值-现在的位置)/10`。

==注意要取整数==，因为出现除法，会出现小数情况，所以当正数时，用Math.ceil函数向上取整，当是负数的时候，用Math.float函数向下取整。

正因为步长变成动态后，可以通过按钮实现前进和后退功能，这个功能主要取决于`目标值-现在的位置`是正数还是负数。

#### 动画函数添加回调函数

**回调函数原理**：函数可以作为一个参数。将这个函数作为参数传到另一个函数里面，当那个函数执行完之后，

再执行传进去的这个函数，这个过程就叫做回调。

示例：

```javascript
function animate(obj, target, callback) {

  // 当我们不断的点击按钮，这个元素的速度会越来越快，因为开启了太多的定时器
  // 解决方案就是 让我们元素只有一个定时器执行
  // 先清除以前的定时器，只保留当前的一个定时器执行
  clearInterval(obj.timer);
  obj.timer = setInterval(function () {
    var x = obj.offsetLeft;
    //步长值加在定时器里面
    var step = (target - x) / 10;
    //加判断条件，如果是正值用天花板函数，如果是负值用地板函数。
    step = step > 0 ? Math.ceil(step) : Math.floor(step);//因为表达式有返回值。
    if (x == target) {
      clearInterval(obj.timer);
      //回调函数写在定时器里面
      if (callback) {
        callback();
      }
    } else {
      obj.style.left = x + step + 'px';
    }
  }, 30);
}

btnOne.addEventListener('click', function () {
  animate(yyy, 800, function () {
    yyy.style.backgroundColor = 'red';
  });
});
```

