# web storage

## 一、Storage类型

Storage类型用于`保存键值对数据`，直到存储空间上限。

Storage的实例有如下几种方法：

- clear()：删除所有值。==注意：火狐浏览器中不能实现。==
- getItem(name)：获得给定的属性值。
- key(index)：获取给定位置的属性名。
- removeItem(name)：删除给定属性名的键值对。
- setItem(name,value)：设置给定的属性名的值。



注意：==Storage类型只能存放**字符串**==，非字符串的数据在存储之前会自动转换为字符串。这种转换不能在获取数据时销毁。

## 二、sessionStorage

sessionStorage只存储会话数据，即当前浏览器关闭后，数据就会消失。

示例：

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>本地存储-sessionstorage</title>
  </head>

  <body>
    <input type="text">
    <button>存储数据</button><button>获取数据</button><button>删除数据</button><button>清空所有数据</button>
    <script>
      const btns = document.querySelectorAll('button');
      let ipt = document.querySelector('input');
      //存储数据
      btns[0].addEventListener('click', function () {
        let value = ipt.value;
        sessionStorage.setItem('shusju', value);
        console.log(sessionStorage.key(0));
      })
      //获取数据
      btns[1].addEventListener('click', () => {
        console.log(sessionStorage.getItem('shuju'));
      })
      //删除数据
      btns[2].addEventListener('click', () => {
        sessionStorage.removeItem('shuju');
      })

      //清空所有的数据
      btns[3].addEventListener('click', () => {
        sessionStorage.clear();
      })
    </script>
  </body>

</html>
```



## 三、localStorage

localStorage可以在客户端持久存储数据，即使浏览器关闭后数据依旧存在。

示例：

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>本地存储-localStorage</title>
  </head>

  <body>
    <input type="text">
    <button>存储数据</button>
    <button>获取数据</button>
    <button>删除数据</button>
    <button>清空所有数据</button>
    <script>
      const btns = document.querySelectorAll('button');
      let ipt = document.querySelector('input');
      btns[0].addEventListener('click', function () {
        let value = ipt.value;
        localStorage.setItem('username', value);
      })
      //获取数据
      btns[1].addEventListener('click', () => {
        console.log(localStorage.getItem('username'));
      })
      //删除数据
      btns[2].addEventListener('click', () => {
        localStorage.removeItem('username');
      })

      //清空所有的数据
      btns[3].addEventListener('click', () => {
        localStorage.clear();
      })
    </script>
  </body>

</html>
```



