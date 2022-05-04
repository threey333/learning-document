# JavaScript案例总结

## 一、图片延迟加载（重要）

**延迟加载(懒加载)** 

图片延迟加载在真实项目中是非常重要的一个==性能优化手段==。

在没有加载真实的图片之前，图片区域用默认的背景图（背景色）占位即可。

- 首屏：先加载首屏中的其它数据（内容、样式、文本），等待其它数据加载完成，再去加载真实的图片
- 其它屏：滚动到相应的区域(一露面、出来一半、完全出现在屏幕中)，再去加载真实的图片



>1.图片延迟加载的意义

如果不做延迟加载，那么页面渲染的时候，会同时把图片资源请求回来，并且加载，这样会阻碍页面的渲染速度，导致首次加载页面速度会很慢；延迟加载一方面提高页面的加载速度、另外也可以减少用户没必要的网络消耗等等....

>案例

案例1（没有滚动事件）：

```javascript
开始图片是隐藏的：在某些IE浏览器中，如果图片的src是空的、或者加载的图片是错误的，如果不弄图片隐藏，就会显示一个图标，很难看，所以同图片没有加载真实地址并且加载正确之前，让图片隐藏
方法一：display:none； =>这种方式在加载完毕真实图片后，还需要让其display：block;这样会触发DOM的回流重绘，型能消耗大...

方法二：opacity：0；
transition:opacity .3s;
->推荐这种方案，一方面后期加载真实图片后，我们只需要设置opacity：1;这样一方面不会引发DOM回流重绘，另一方面还可以跟CSS3过度动画实现出渐变效果...

```

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>延迟加载代码优化</title>
        <style>
            .imageLazyBox {
                margin: auto;
                width: 658px;
                height: 438px;
                background: url(./images/default.gif) no-repeat center center #eee;
            }

            img {
                width: 100%;
                height: 100%;
                opacity: 0;
                /* 用过度效果更好 */
                transition: opacity .3s;
            }
        </style>
    </head>

    <body>
        <div class="imageLazyBox">
            <img src="" alt="" lazy-image="./images/webp (4).webp">
        </div>
        <script>
            function lazyImage(imageLazyBox) {
                const imageItem = imageLazyBox.querySelector('img');
                let lazy_images = imageItem.getAttribute('lazy-image');

                //当图片加载成功后触发事件     onload or onerror
                imageItem.onload = () => {
                    //加载成功后把图片的opacity变为1
                    imageItem.style.opacity = 1;
                }
                imageItem.src = lazy_images;

                //不论图片是否加载成功，只要处理过，就把lazy-image自定义属性移除(证明其已经处理过)
                imageItem.removeAttribute('lazy-image');
            }

            const imageLazyBox = document.querySelector('.imageLazyBox')

            setTimeout('lazyImage(imageLazyBox)', 1000);

        </script>
    </body>

</html>
```

案例链接：[图片延迟加载1](file:///D:/LearningZiLiao/%E5%89%8D%E7%AB%AF/JS/js/%E6%A1%88%E4%BE%8B/%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD/01-%E5%8D%95%E5%BC%A0%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD.html)



案例2（滚动触发事件的延迟加载）：

利用封装好的offset函数来获取当前元素相当于BODY的偏移量来实现

![image-20210503164020142](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210503164020142.png)

```html
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>单张图片延迟加载02</title>
        <style>
            .imageLazyBox {
                margin: 1000px 0;
                width: 658px;
                height: 438px;
                background: url(./images/default.gif) no-repeat center center #eee;
            }

            img {
                width: 100%;
                height: 100%;
                opacity: 0;
                /* 用过度效果更好 */
                transition: opacity .3s;
            }
        </style>
    </head>

    <body>
        <div class="imageLazyBox">
            <img src="" alt="" lazy-image="./images/webp (4).webp">
        </div>
        <script>
            function offset(element) {
                let left = element.offsetLeft;  //获取当前元素相当于有定位的参照物的左偏移
                let top = element.offsetTop;    //获取当前元素相当于有定位的参照物的上偏移
                let p = element.offsetParent;   //获取当前元素的有定位的父元素对象
                while (p && p.tagName !== "BODY") {
                    //兼容IE浏览器，因为IE浏览器的偏移量是从边框开始的
                    if (/MSIE 8/.test(navigator.userAgent)) {
                        //把边框都加进去
                        left += p.clientLeft;
                        top += p.clientTop;
                    }
                    left += p.offsetLeft;
                    top += p.offsetTop;
                    p = p.offsetParent;
                }
                return {
                    left: left,
                    top: top
                }
            }

            function lazyImage(imageLazyBox) {
                const imageItem = imageLazyBox.querySelector('img');
                let lazy_images = imageItem.getAttribute('lazy-image');

                //方法1 用lazy_images自定义属性判断是否加载过图片，如果加载过就return直接结束
                // if (!lazy_images) return;

                //方法2 用一个布尔值确定是否加载过图片，加载了就不触发滚动事件（推荐）
                //当图片加载成功后触发事件     onload or onerror
                imageItem.onload = () => {
                    //加载成功后把图片的opacity变为1
                    imageItem.style.opacity = 1;
                }
                imageItem.src = lazy_images;

                //不论图片是否加载成功，只要处理过，就把lazy-image自定义属性移除(证明其已经处理过)
                imageItem.removeAttribute('lazy-image');
                imageLazyBox.data_isLoad = true;
            }

            const imageLazyBox = document.querySelector('.imageLazyBox');
            const HTML = document.documentElement;

            //onscroll特点：只要页面滚动就会触发，而且触发频率特别频繁。
            window.onscroll = function () {
                //判断imageLazyBox的自定义属性imageLazyBox.data_isLoad的值是否为true，如果是就说明图片第一次已延迟加载成功了，后面就不需要再进行。
                if (imageLazyBox.data_isLoad) {
                    return 0;
                }

                //计算出当前盒子底边距距离BODY的偏移量和盒子自身的高度  跟  浏览器底边距离BODY的距离 做比较，如果A<=B的时候，我们去加载真实的图片即可。
                let A = offset(imageLazyBox).top + imageLazyBox.offsetHeight,
                    B = HTML.clientHeight + HTML.scrollTop;
                if (A <= B) {
                    lazyImage(imageLazyBox);
                }
            }

        </script>
    </body>

</html>
```

案例2链接:[案例2](file:///D:/LearningZiLiao/%E5%89%8D%E7%AB%AF/JS/js/%E6%A1%88%E4%BE%8B/%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD/02-%E5%8D%95%E5%BC%A0%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD.html)



案例3：

利用getBoundingClientRect()来获取距离浏览器可视区窗口的偏移量，从而在滚动事件中延迟加载

![image-20210503203302950](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210503203302950.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单张图片延迟加载03</title>
    <style>
        .imageLazyBox {
            margin: 1000px 0;
            width: 658px;
            height: 438px;
            background: url(./images/default.gif) no-repeat center center #eee;
        }

        img {
            width: 100%;
            height: 100%;
            opacity: 0;
            /* 用过度效果更好 */
            transition: opacity .3s;
        }
    </style>
</head>

<body>
    <div class="imageLazyBox">
        <img src="" alt="" lazy-image="./images/webp (4).webp">
    </div>
    <script>
        function lazyImage(imageLazyBox) {
            const imageItem = imageLazyBox.querySelector('img');
            let lazy_images = imageItem.getAttribute('lazy-image');

            imageItem.onload = () => {
                imageItem.style.opacity = 1;
            }
            imageItem.src = lazy_images;

            imageItem.removeAttribute('lazy-image');
            imageLazyBox.data_isLoad = true;
        }

        const imageLazyBox = document.querySelector('.imageLazyBox');
        const HTML = document.documentElement;

        window.onscroll = function () {
            if (imageLazyBox.data_isLoad) {
                return 0;
            }


            let A = imageLazyBox.getBoundingClientRect().bottom,
                B = HTML.clientHeight;
            if (A <= B) {
                lazyImage(imageLazyBox);
            }

        }

    </script>
</body>

</html>
```

案例3链接：[案例3](file:///D:/LearningZiLiao/%E5%89%8D%E7%AB%AF/JS/js/%E6%A1%88%E4%BE%8B/%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD/04-%E5%8D%95%E5%BC%A0%E5%9B%BE%E7%89%87%E5%BB%B6%E8%BF%9F%E5%8A%A0%E8%BD%BD.html)



## 二、利用懒加载实现瀑布流效果

