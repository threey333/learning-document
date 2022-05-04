# JavaScript常用的数组方法总结

## 基础函数方法

### 一、push()方法

`push()方法` 将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

**语法：**

>```js
>arr.push(element1, ..., elementN)
>```

**方法参数：**

- `{String | number | Array | Object} elementN`  新的任何类型的元素。

使用示例：

```js
const animals = ['pigs', 'goats', 'sheep']

const count = animals.push('cows')

console.log(count) //=> 4

console.log(animals) //=>['pigs', 'goats', 'sheep', 'cows']
```



### 二、pop()方法

`pop()方法` 从数组中删除最后一个元素，并返回该元素的值。

==注意：此方法会更改数组的长度。==

**语法：**

>```js
>arr.pop()
>```

使用示例：

```js
const plants = ['broccoli', 'cauliflower', 'cabbage', 'kale', 'tomato']

const dleEle = plants.pop()

console.log(dleEle) //=>"tomato"

console.log(plants) //=>['broccoli', 'cauliflower', 'cabbage', 'kale']
```



### 三、unshift()方法

`unshift()方法` 将一个或多个元素添加到数组的头部，并返回该数组的新长度。

**语法：**

>```js
>arr.unshift(element1, ..., elementN)
>```

**方法参数：**

- `{String | number | Array | Object} elementN`  新的任何类型的元素。

使用示例：

```js
const array1 = [1, 2, 3, 4, 5]

const result = array1.unshift(10)

console.log(result) //=> 6

console.log(array1) //=>[10, 1, 2, 3, 4, 5]
```



### 四、shift()方法

`shift()方法` 从数组中删除第一个元素，并返回该元素的值。

==注意：该方法会改变原数组。==

**语法：**

>```js
>arr.shift()
>```

使用示例：

```js
const array2 = ['a', 'b', 'c', 'd']

const result2 = array2.shift()

console.log(result2) //=>"a"

console.log(array2) //=>['b', 'c', 'd']
```



### 五、Array.from()方法

`from()方法 `是将一个**伪数组**或一个**可遍历的对象**转换为一个真正的数组,并且返回这个新数组。

==注意:这个方法是个浅拷贝过程。==

**语法：**

>```js
>Array.from(arrayLike[, mapFn[, thisArg]])
>```

**方法参数：**

- `{String | Set | Map | NodeList等} arrayLike` 要转换成真正数组的可遍历的对象。
- `{Function} mapFn` 对原对象要进行处理的回调函数。
- `{Object} thisArg` 回调函数的this对象。

使用示例：

```js
### 对String的遍历器对象
const str = 'foo'
console.log(Array.from(str)) //=>['f', 'o', 'o']

### 对Set的遍历器对象
const set = new Set([1, 2, 3, 4, 5])
console.log(Array.from(set)) //=>[1, 2, 3, 4, 5]

### 对Map的遍历器对象
const map = new Map([
  ['name', 'MIYA'],
  ['age', 20],
  ['title', 'Author'],
])
console.log(map) //{"name" => "MIYA", "age" => 20, "title" => "Author"}
console.log(Array.from(map)) //=>[Array(2), Array(2), Array(2)]
```



### 六、Array.isArray()方法

`isArray()方法` 用来检测该值是否是Array.如果是返回true,否则返回false。

**语法：**

>```js
>Array.isArray(obj)
>```

**方法参数：**

- `{any} obj` 可以是任何类型的元素。

使用示例：

```js
Array.isArray([1, 2, 3])//=> true

Array.isArray({ foo: 123 })//=> false

Array.isArray('foobar')//=> false

Array.isArray(undefined)//=> false
```



### 七、reverse()方法

`reverse()方法 `将数组中元素的位置都颠倒，并返回该数组。

==注意：该方法会改变原数组。==

**语法：**

>```js
> arr.reverse()
>```

使用示例：

```js
const array3 = ['one', 'two', 'three', 'four']

console.log('原数组的各元素的位置', array3) //=>['one', 'two', 'three', 'four']

const res = array3.reverse()

console.log(res) //=>['four', 'three', 'two', 'one']

console.log('原数组发生改变', array3) //=>['four', 'three', 'two', 'one']
```



### 八、indexOf()方法

`indexOf()方法  `是用来查找给定的元素是否在数组中，如果在则返回改元素的第一个索引位置，否则返回-1。

**语法：**

>```js
>arr.indexOf(searchElement[, fromIndex])
>```

**方法参数：**

- `{string} searchElement` 要查找的元素。
- `{number} fromIndex` 选择查找的开始位置。

使用示例：

```js
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison']
console.log(beasts.indexOf('ant')) //=>0

console.log(beasts.indexOf('bison')) // =>1

console.log(beasts.indexOf('camel')) //=>2

### 该方法第二个参数表示从什么位置开始查找。
console.log(beasts.indexOf('bison', 2)) //=>4

console.log(beasts.indexOf('apple')) //=>-1
```



### 九、lastIndexOf()方法

`lastIndexOf()方法` 返回指定元素在数组中的最后一个的索引，如果不存在则返回-1。

**语法：**

>```js
>arr.lastIndexOf(searchElement[, fromIndex])
>```

**方法参数：**

- `{string} searchElement` 要查找的元素。
- `{number} fromIndex` 选择查找的开始位置。

使用示例：

```js
const animals = ['Dodo', 'Tiger', 'Penguin', 'Dodo']

console.log(animals.lastIndexOf('Dodo')) //=>3

console.log(animals.lastIndexOf('Tiger')) //=>1

console.log(animals.lastIndexOf('apple')) //=>-1

### 该方法的第二个参数表示从什么位置开始从左往右进行查找。
console.log(animals.lastIndexOf('Dodo', 2)) //=>0
```



### 十、toString()方法

`toString()方法` 将数组变成一个字符串的形式并且返回这个字符串。

**语法：**

>```js
>arr.toString()
>```

使用示例：

```js
const array6 = [1, 3, 'a', 'hot']

console.log(array6.toString()) //=>"1,3,a,hot"
```



### 十一、join()方法

`join()方法` 将一个数组或一个伪数组中的所有每一个元素连接成一个字符串并且返回这个字符串。

**语法：**

>```js
>arr.join([separator])
>```

**方法参数：**

- `{string} separator` 字符串形式的分隔符

**当该方法没有参数的时候，默认是以逗号为分隔符。**

使用示例：

```js
const elements = ['first', 'second', 'three', 'four']

console.log(elements.join('-')) //=>"first-second-three-four"

### 当该方法没有参数的时候，默认是以逗号为分隔符。
console.log(elements.join()) //=>"first,second,three,four"

### 当数组中只有一个元素的时候，直接返回字符串类型的该元素。
const a = ['apple']
console.log(a.join('-'))  //=>"apple" 
```



### 十二、concat()方法

`concat()方法` 用于合并两个或多个数组。此方法不会改变原数组，而且会返回一个合并后的新数组。

**语法：**

>```js
>const new_array = old_array.concat(value1[, value2[, ...[, valueN]]])
>```

**方法参数：**

- `{Array} valueN` 要合并的一个或多个新数组元素。

使用示例一：

```js
const array7 = ['q', 'w', 'e']

const array8 = ['a', 's', 'd', 'f', 'g']

console.log(array7.concat(array8)) //=>['q', 'w', 'e', 'a', 's', 'd', 'f', 'g']
```

使用示例二：该方法还可以对数组进行浅拷贝。

```js
const arr3 = arr1.concat([])

console.log(arr3) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr3 === arr1) // => fasle
```



### 十三、slice()方法

`slice()方法` 返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括end）。

==注意：原始数组不会被改变。==

**语法：**

>```js
>arr.slice([begin[, end]])
>```

**方法参数：**

- `{number} begin` 切割的开始位置。
- `{number} end` 切割的结束位置。

使用示例：

```js
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant']

console.log(animals.slice()) //=>['ant', 'bison', 'camel', 'duck', 'elephant']  当slice()没有参数的时候,直接浅拷贝数组中的所有元素.

console.log(animals.slice(2)) //=>["camel", "duck", "elephant"]

console.log(animals.slice(2, 4)) //=>["camel", "duck"]

console.log(animals.slice(1, 5)) //=>["bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2)) //=>["duck", "elephant"]

console.log(animals.slice(2, -1)) //=>["camel", "duck"]
```



### 十四、splice()方法

`splice()方法` 通过删除或替换现有元素或者原地添加新的元素来修改数组,并以数组形式返回被修改的内容。

==注意：此方法会改变原数组。==

**语法：**

>```js
>array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
>```

**方法参数：**

- `{number} start` 删除的开始位置下标。
- `{number} deleteCount` 删除元素的数量。
- `{any} itemN` 要新添加的元素。

```js
const array10 = ['a', 'b', 'c', 'd', 'e', 'f']
// 删除元素
array10.splice(3, 3)
console.log(array10) //=>['a', 'b', 'c']

// 添加元素
array10.splice(2, 0, 'w', 'z') //第二个参数为0,则表示在某个位置开始添加元素.
console.log(array10) //=>['a', 'b', 'w', 'z', 'c', 'd', 'e', 'f']

// 替换元素
array10.splice(1, 2, 'x', 'y')
console.log(array10) //=>['a', 'x', 'y', 'z', 'c']
```



### 十五、keys()方法



### 十六、values()方法



### 十七、entries()方法



### 十八、includes()方法



## 高阶函数方法

### 一、filter高阶函数

`filter()高阶函数` 用于把数组的某些元素过滤掉，然后返回剩下的元素。

作用：`filter()` 把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。

```js
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6); //过滤掉长度小于6的元素，保留长度大于6的元素

console.log(result); //=>["exuberant", "destruction", "present"]
```



### 二、map高阶函数

`map()高阶函数` 创建一个新数组，其结果是该数组中的每个元素调用一次提供的函数后的返回值。

```js
const array1 = [1, 4, 9, 16];

const map1 = array1.map(x => x * 2);

console.log(map1); // => [2, 8, 18, 32]

##把Array的所有数字转为字符串;
const map2 = array1.map(String);
console.log(map2);  // => ["1", "4", "9", "16"] 
```

注意：**`map高阶函数`会返回一个新数组。**



### 三、forEach高阶函数

`forEach()高阶函数`和 `map()高阶函数`功能类似，也是把数组中每个元素依次作用于传入的函数中，**区别在于它不返回新的数组。**

```js
const arr = ['Apple', 'pear', 'orange']

arr.forEach((item, index) => console.log(item, index))

// 结果：Apple 0，pear 1，orange 2
```



### 四、reduce高阶函数

`reduce()高阶函数` 对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值，然后返回出去。

简单理解就是把里面的事情都做好了然后将最终的结果返回。

**==注意：该高阶函数中的回调函数的this执行不能被改变（请看示例2）。==**

示例一：

```js
##利用reduce()求积
const array2 = [1, 2, 3, 4, 5, 6];
const amass = function (x, y) {
  return x * y;
}
const result0 = array2.reduce(amass);
console.log(result0);  //=>720

##reduce操作实现一个string2int()函数：
function string2int(s) {
  s = s.split("");   //切分后变成数组
  s = s.map(x => +x);
  //[1, 3, 1, 5, 1, 6, 1, 6, 1, 6]
  //利用一元运算符+使字符串隐式转换为number类型
  s = s.reduce((x, y) => x * 10 + y);
  return s;
}
console.log(string2int("1315161616"))  //=>1315161616
```



示例二：`reduce()高阶函数` 中的回调函数this执行不能被改变。 

```js
const arr = [1, 2, 3, 4, 5, 6]
arr.reduce(
  function (prev, item, index, array) {
    console.log(this) // => this指向的还是window
    return prev + item
  },
  '0',
  { a: '1' }
)
```



### 五、sort高阶函数

一、`sort()方法` 是一个高阶函数，它对数组中的元素进行排序。**默认排序顺序是将元素转换为字符串，然后比较它们的UTF-16编码大小。**

==注意：sort()方法会改变原数组。==

```js
### 对字符串的排序
const months = ['March', 'Jan', 'Feb', 'Dec']
months.sort()
console.log(months) //=>['Dec', 'Feb', 'Jan', 'March']

### 对数字的排序
const array4 = [21, 4, 2, 5, 123, 2124]
array4.sort()
console.log(array4) //=>[123, 2, 21, 2124, 4, 5]
```

二、它还可以接受一个比较函数来实现自定义排序。

```js
array4.sort((a, b) => a - b)
console.log(array4) //=>[2, 4, 5, 21, 123, 2124] 升序

array4.sort((a, b) => b - a)
console.log(array4) //=>[2124, 123, 21, 5, 4, 2] 降序
```

三、简单案例表示sort()方法的使用：

```js
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic' },
  { name: 'Zeros', value: 37 },
]

// sort by value
items.sort(function (a, b) {
  return a.value - b.value
})

console.table(items)

// sort by name
items.sort(function (a, b) {
  var nameA = a.name.toUpperCase() // ignore upper and lowercase
  var nameB = b.name.toUpperCase() // ignore upper and lowercase
  if (nameA < nameB) {
    return -1
  }
  if (nameA > nameB) {
    return 1
  }

  // names must be equal
  return 0
})
console.table(items)
```



### 六、every高阶函数

`every()高阶函数` 可以判断数组中的所有元素是否符合条件，全符合符合true,否则返回false。

```js
##检查数组中的每个元素的长度是否大于0
const arr = ['threey', 'Rose', 'Luckyly', 'apple']

console.log(arr.every(item => item.length) > 0) //=>结果:true

##检查数组的每个元素是否都是小写
console.log(arr.every(item => item.toLowerCase() === item)) //结果：false
```



### 七、find高阶函数

`find()高阶函数` 可以查找符合条件的一个元素，如果找到了则返回找到的元素，否则返回undefined

```js
const arr = ['threey', 'ROSE', 'typescript']
console.log(arr.find(item => item.toUpperCase() === item)) //结果：ROSE
```



### 八、findIndex高阶函数

`findIndex高阶函数` 和 `find高阶函数` 功能相识,不同是该方法返回的是符合条件的第一个元素索引，找不到则返回-1

```js
const arr = ['Apple', 'pear', 'orange']
console.log(arr.findIndex(item => item.toUpperCase() === item)) //结果:-1
console.log(arr.findIndex(item => item.toLowerCase() === item)) //结果:1
```



### 九、some高阶函数

`some()高阶函数` 可以检查数组中是否至少一个元素符合条件，如果至少有一个元素符合条件则返回true,否则返回false。

**==注意：只要有一个条件符合，后面就不会再进行检测。==**

```js
##检测数组中是否有偶数
const array = [1, 2, 3, 4, 5]
console.log(array.some(item => item % 2 === 0))

##如果some()方法对一个空数组进行检测，则永远返回false。
const array2 = []
console.log(array2.some(item => item % 2 === 0)) //=>false
```

注意：**`some高阶函数` 被调用时不会改变数组。**



## 高阶函数各方法的重写

### 一、forEach高阶函数重写

示例：

```js
// 重写forEach
Array.prototype.forEach = function (fn, optionThis) {
  const arr = this // 保存当前的this指向
  const context = optionThis ? optionThis : window
  const length = arr.length
  let i = 0
  // 判断是否有回到函数
  const isExistCallbackFn = typeof fn === 'function' ? true : false
  for (; i < length; i++) {
    if (isExistCallbackFn) {
      fn.call(context, arr[i], i, arr)
    } else {
      throw new Error('参数必须是function')
    }
  }
}
```



### 二、map高级函数重写

示例：

```js
// 重写map方法
Array.prototype.map = function (fn, optionThis) {
  const arr = this // 保存当前this的指向
  const ctx = optionThis ? optionThis : window
  const length = arr.length
  // map方法还有一个返回值,返回值的内容按用户进行的处理，没有return 默认是undefined
  const res = []
  let i = 0
  for (; i < length; i++) {
    if (typeof fn === 'function') {
      // 存在回调函数
      res.push(fn.call(ctx, arr[i], i, arr))
    } else {
      throw new Error('函数的参数必须是function')
    }
  }
  return res
}
```



### 三、filter高阶函数重写

示例：

```js
// 重写filter方法
Array.prototype.filter = function (fn, optionThis) {
  const arr = this
  const context = optionThis ? optionThis : window
  const length = arr.length
  const res = []
  let i = 0
  // 判断fn是否是回调函数
  if (typeof fn !== 'function') throw new Error('传入的参数必须是function')
  for (; i < length; i++) {
    // 如果fn回调函数返回的是true，则push到res中，否则不做任何处理
    fn.call(context, arr[i], i, arr) ? res.push(arr[i]) : null
  }
  return res
}
```



### 四、some高阶函数重写

示例：

```js
// 重写some方法
Array.prototype.some = function (fn, optionThis) {
  const arr = this
  const length = arr.length
  const context = optionThis ? optionThis : window
  let i = 0
  let res = false // 默认为false
  if (typeof fn !== 'function') throw new Error('传入的第一个参数必须是function')
  for (; i < length; i++) {
    if (fn.call(context, arr[i], i, arr)) {
      res = true
      break
    }
  }
  return res
}
```



### 五、every高阶函数重写

示例：

```js
// 重写every方法
Array.prototype.every = function (fn, optionThis) {
  const arr = this
  const length = arr.length
  const context = optionThis ? optionThis : window
  let i = 0
  let res = true // 默认为true
  if (typeof fn !== 'function') throw new Error('传入的第一个参数必须是function')
  for (; i < length; i++) {
    if (!fn.call(context, arr[i], i, arr)) {
      res = false
      break
    }
  }
  return res
}
```



### 六、reduce高阶函数重写

示例：

```js
// 重写reduce方法
Array.prototype.reduce = function (fn, prev) {
  const arr = this
  const length = arr.length
  let i = 0
  // 判断传递的第一个参数是否是回调函数,如果不是则抛出错误
  if (typeof fn !== 'function') throw new Error('传入的第一个参数不是回调函数')
  // 判断是否设置了初始值,如果设置了,则初始值为设置的那个，否则是数组的第一个
  if (typeof prev === 'undefined') {
    prev = arr[0]
    i = 1
  }
  for (; i < length; i++) {
    prev = fn(prev, arr[i], i, arr)
  }
  return prev
}
```

