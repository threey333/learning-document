# 移动Web单击事件

## 一、touch事件

在PC端，用户的点击操作主要是由鼠标的左键或者右键来产生。

而在移动Web端，是用手指执行所以有了`手指触摸事件(touch)`

### 1.1 touch事件分类

| touch触摸的类型 |              解释              |
| :-------------: | :----------------------------: |
|   touchstart    |    当手指与屏幕接触时触发。    |
|    touchmove    | 当手指在屏幕上滑动时连续地触发 |
|    touchend     |    当手指从屏幕上离开时触发    |
|   touchcancel   |        用的少，不解释。        |

示例：

![image-20210520200550442](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520200550442.png)

结果：

![image-20210520200628682](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520200628682.png)

### 1.2 touch事件对象

​		对于`touch事件`，每一次触发都可以得到一个事件对象，在JavaScript中这个对象叫作`TouchEvent`,利用这个对象我们可以知道它里面有什么属性和方法。

示例：

```javascript
document.addEventListener('touchstart', function (e) {
    console.log(e);
});
```

结果：

![image-20210520201350314](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520201350314.png)

​		从图中`TouchEvent`的属性中，经常使用的就是`touch`、`targetTouches`和`changedTouches`，它们的含义分别是：

- touches：当前页面(屏幕)上所有的触摸点。
- targetTouches：当前绑定事件的元素上的触摸点。
- changedTouches：当前屏幕上刚刚接触的手指或者离开的手指的触摸点。

这3个属性返回的是`TouchList`对象，代表的是一个`touch`的集合数组。也就是==说每一次touch触发，都会兼顾到多指触摸的场景==。

>注意点：

1.如果监听的是一个DOM元素，`touches`和`targetTouches`返回的集合数组是一样的。

```javascript
box.addEventListener('touchstart', function (e) {
    console.log('我手点击触摸你'); 
    console.log(e.touches);
    console.log(e.targetTouches);
});
```

![image-20210520204515164](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520204515164.png)

2.当我们手指离开屏幕的时候，就没有了`touches`和`targetTouches`列表，但会有`changedTouches`。

```javascript
box.addEventListener('touchend', function (e) {
    console.log('我收点击滑动离开了你触发');
    console.log(e.touches);
    console.log(e.targetTouches);
    console.log(e.changedTouches);
})
```

![image-20210520205255856](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520205255856.png)

### 1.3 offsetX/Y、pageX/Y、clientX/Y属性

​		在`touches`、`targetTouches`和`changedTouches`属性中有用到==**offsetX/Y、pageX/Y、clientX/Y属性**==这3个属性。

区别和含义分别是：

- offsetX/Y：触摸位置相当于事件源元素的位置坐标。
- pageX/Y：触摸位置相当于**整个页面内容区域**的位置坐标，当页面过长时，包括滚动隐藏的部分内容，以页面完整内容区域的左上角为原点。
- clientX/Y：触摸位置相当于浏览器视区(屏幕)区域的位置坐标，以可视区内容区域的左上角为原点。

![image-20210520212037736](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210520212037736.png)



## 二、移动Web端单击事件

​		移动Web端的单击事件完全可以由`touchstart`、`touchmove`和`touchend`来组合实现，同时移动Web端也提供了`原生的click事件`，它和传统的PC端的click事件是一样的。

>移动Web端的click事件触发要求：**用户要完成一次完整的手指触摸事件**，之后就会触发。

代码如下：

```javascript
document.addEventListener("click",function(e){
    console.log(e);
})
```

### 2.1 iOS单击延迟

在之前呢，我们先了解下什么是**视口(Viewport)**

> 视口

**视口**就是浏览器显示页面内容的屏幕区域。视口可以分为`布局视口`、`视觉视口`和`理想视口`。

#### 布局视口

1. 一般移动设备的浏览器都默认设置了一个`布局视口`，用于解决早期的PC端页面在手机上显示的问题。

2. iOS, Android基本都将这个视口分辨率设置为 980px，所以PC上的网页大多都能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页。

#### 视觉视口

- 字面意思，它是用户正在看到的网站的区域。**注意：是网站的区域。**
- 我们可以通过缩放去操作视觉视口，但==不会影响布局视口==，布局视口仍保持原来的宽度。

#### 理想视口

最好的理解就是将前面两者结合在一起，形成一个最理想视口，这时就需要我们手动添写`meta视口标签`通知浏览器操作。

`meta视口标签`的主要目的：布局视口的宽度应该与理想视口的宽度一致，简单理解就是设备有多宽，我们布局的视口就多宽。

meta视口标签：

```html
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```



#### 早期布局视口引发的问题

​		因为早期布局视口的概念，Android和iOS的移动端浏览器访问页面时，需要双击操作将页面放大，而这个双击操作之间有300毫秒的时间间隔。

​		也就是说，哪怕是单击这个事件，也都会经过双击放大这个判断逻辑，导致300毫秒之后才能收到单击事件的逻辑反馈，这就是==300毫秒的单击延迟问题。==

​		对于Android系统的浏览器而言，可以通过给视口设置`user-scalable=no`来禁止用户进行缩放，而且还可以正常使用原生的click事件而没有延迟。

​		对于iOS系统而言就不行了，无法通过简单的设置来达到正常使用原生click事件的目的。

### 2.2 iOS移动端单击延迟解决方案

使用`FastClick.js库`解决；

在后来，苹果公司意识到了这个问题的严重性，于是在iOS9.3版本时就解决了这个问题，从此，移动Web端的300毫秒单击延迟问题得到了彻底的改善。

