# JavaScript常见面试题总结

## 一、数组去重

### 1.1 使用Set集合去重

使用ES6提供的数据结构 `Set` ，可以实现数组的去重。

```js
### 使用Set去重
const array = [1, 2, 4, 6, 8, 8, 9, 12, 13, 13, 23, 545, 32, 55, 2, 1, 5, 8,]
const uniqueArr = array => {
  return [...new Set(array)]
}
```



### 1.2 使用for循环和includes方法去重

使用for循环配合 `includes方法` 也可以实现数组去重。

```js
### 使用for循环和includes方法实现数组去重
const array = [1, 2, 4, 6, 8, 8, 9, 12, 13, 13, 23, 545, 32, 55, 2, 1, 5, 8,]
const uniqueArr = array => {
  const arr = []
  for (let i = 0, length = array.length; i < length; i++) {
    if (!arr.includes(array[i])) {
      arr.push(array[i])
    }
  }
  return arr.sort((a, b) => a - b)
}
```



### 1.3 使用filter高阶函数去重

使用 `filter高阶函数` 也是可以实现数组去重。

```js
### 使用filter高阶函数实现数组去重
const array = [1, 2, 4, 6, 8, 8, 9, 12, 13, 13, 23, 545, 32, 55, 2, 1, 5, 8,]
const uniqueArr = array => {
  return array
    .filter((item, index) => {
    // 里面再利用indexOf进行判断处理
    return array.indexOf(item) === index
  }).sort((a, b) => a - b)
}
```



### 1.4 使用reduce高阶函数去重

使用 `reduce高阶函数` 可以实现数组去重。

```js
### 利用reduce高阶函数实现去重
const array = [1, 2, 4, 6, 8, 8, 9, 12, 13, 13, 23, 545, 32, 55, 2, 1, 5, 8,]
const uniqueArr = array => {
  return array
    .sort((a, b) => a - b)
    .reduce((prev, next) => {
    if (prev.length === 0 || prev[prev.length - 1] !== next) {
      prev.push(next)
    }
    return prev
  }, [])
}
```

 

### 1.5 利用对象key的唯一性实现数组去重

利用对象key的唯一性实现数组的去重。

```js
const array = [
  1, 2, 4, 6, 8, 8, 9, 12, 13, 13, 23, 545, 32, 55, 2, 1, 5, 8,
]
const uniqueArr = array => {
  const obj = {},
        _arr = [],
        length = array.length
  for (let i = 0; i < length; i++) {
    if (!obj[array[i]]) {
      obj[array[i]] = 1
      _arr.push(array[i])
    } else {
      obj[array[i]]++
    }
  }
  return _arr.sort((a, b) => a - b)
}
console.log(uniqueArr(array))
```

