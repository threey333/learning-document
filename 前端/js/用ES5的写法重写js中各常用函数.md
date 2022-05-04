# 用ES5的写法重写js中各常用函数

## 一、call函数重写

重写 `call()`：

```js
/*重写call方法*/
Function.prototype.call = function (obj, ...args) {
  const fn = this // 保存当前调用的this指向
  let key = Symbol('KEY')
  let result
  // 1.先判断是否是null,如果是则不改变this指向
  obj = obj === null ? window : obj
  !/^(object|function)$/.test(typeof obj) ? (obj = Object(obj)) : null
  obj[key] = fn
  result = obj[key](...args)
  delete obj[key]
  return result
}
```



## 二、apply函数重写

重写 `apply()`：

```js
/* 重写apply() */
Function.prototype.apply = function (obj) {
  const fn = this
  const key = Symbol('KEY')
  const param = arguments[1]
  let result
  obj === null ? (obj = window) : null
  !/^(object|function$)/.test(typeof obj) ? (obj = Object(obj)) : null
  obj[key] = fn
  result = obj[key](param)
  delete obj[key]
  return result
}
```



## 三、bind函数重写

重写 `bind()`：

```js
// 重写bind方法
Function.prototype.bind = function () {
  const fn = this // 保存当前调用bind的this
  const _this = arguments[0] //记录改动后的this指向
  const args = Array.prototype.slice.call(arguments, 1)
  return function () {
    return fn.apply(_this, args.concat(Array.prototype.slice.call(arguments)))
  }
}

// 验证:
function foo(...arg) {
  console.log(this, arg) // => {a: 1}   [3, 4, 5, 6, 7, 8, 10, 23, 45]
}
foo.bind({ a: 1 }, 3, 4, 5, 6, 7, 8)(10, 23, 45)
```

 

