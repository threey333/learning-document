# JavaScript的常用字符串方法总结

### 一、charAt()方法

`charAt()方法` 返回的指定位置的字符。

**语法：**

>```js
>str.charAt(index)
>```

**方法参数：**

- `{number} index` 字符串下标位置。

使用示例：

```js
console.log(somethings.charAt(2)) //=> l
console.log(somethings.charAt(1)) //=> e
```



### 二、concat()方法

`concat()方法 ` 是用来将一个或多个字符串和原来的字符串进行合并拼接，形成一个新的字符串并返回。

**语法：**

>```
>str.concat(str2, [, ...strN])
>```

**方法参数：**

- `{String} str2` 字符串。

使用示例：

```js
### 示例1
const title = '今天11月5号啦,'

const message = '吃饭了没'

const strRes = title.concat(message)

console.log(strRes) //=>”今天11月5号啦,吃饭了没“

### 示例2
const fruit = ['apple', ' ', 'pear']

const my = 'i like eat '

const res = my.concat(...fruit) //=> i like eat apple pear

console.log(res)
```



### 三、startsWith()方法

`startsWith()方法`  检测指定的字符串参数是否在该字符串的开头，如果在则返回true，否则返回false。

**语法：**

 >```js
 >str.startsWith(searchString[, position])
 >```

**方法参数：**

- `{string} searchString` 要检测的字符串参数。
- `{number} position` 检测的开始位置。

使用示例：

```js
const str1 = 'Saturday night plans'
console.log(str1.startsWith('Sat')) //=>true
console.log(str1.startsWith('End')) //=>false
console.log(str1.startsWith('Sat', 3)) //=>false
```



### 四、endsWith()方法

`endsWith()方法` 检测字符串参数是否在原字符串的尾部，如果在则返回true，否则返回false。

**语法：**

>```js
>str.endsWith(searchString[, position])
>```

**方法参数：**

- `{String} searchString` 要检测的字符串参数。
- `{number} position` 检测的开始位置。

使用示例：

```js
const str1 = 'Saturday night plans'
console.log(str1.endsWith('plans')) //=>true
console.log(str1.endsWith('an')) //=>false
console.log(str1.endsWith('ans', 18)) //=>false
console.log(str1.endsWith('ans', 20)) //=>true
```



### 五、indexOf()方法

`indexOf()方法` 是用来查找指定的字符是否在该字符串中，如果在则返回该字符第一次出现的下标位置，否则返回-1。

**语法：**

>```js
>str.indexOf(searchValue [, fromIndex])
>```

**方法参数：**

- `{String} searchValue` 要查找的字符。
- `{number} fromIndex` 数字表示开始查找的位置。可以是任意整数，默认值为0。

使用示例：

```js
const str = 'anndy'

const str1 = 'asdninyz'

console.log(str.indexOf('d')) //=>3

console.log(str.indexOf('n')) //=>1

// 该方法还支持第二个参数，表示从什么位置开始进行查找
console.log(str.indexOf('n', 2)) //=>2
```



### 六、lastIndexOf()方法

`lastIndexOf()方法` 也用来查找指定的字符是否在该字符串中，不同的是它是从后往前面进行查找。如果在则返回一个出现的下标位置，否则返回-1。**（即从右往左找）**。

**语法：**

>```js
>str.lastIndexOf(searchValue[, fromIndex])
>```

**方法参数：**

- `{String} searchValue` 要查找的字符。
- `{number} fromIndex`  数字表示开始查找的位置。如果 `fromIndex >= str.length`，则会搜索整个字符串，如果是 `fromIndex < 0`，则等同于 `fromIndex === 0`。 

使用示例：

```js
const str = 'anndy'

const str1 = 'asdninyz'

console.log(str.lastIndexOf('e')) //=>-1 说明不在

console.log(str.lastIndexOf('n')) //=>2

// 该方法也支持第二个参数,表示从什么位置开始进行查找
console.log(str1.lastIndexOf('n', 4)) //=>3

console.log(str1.lastIndexOf('n', 6)) //=>5
```



### 七、includes()方法

`includes()方法` 用于判断一个字符串是否在另一个字符串之中，如果存在则返回true，否则返回false。

**语法：**

>```js
>str.includes(searchString[, position])
>```

**方法参数：**

- `{string} searchString` 要查找的字符串。
- `{number} position` 开始查找的位置。

使用示例：

```js
const string = 'blue whale'

console.log(string.includes('blue')) //=>true

// 该方法支持第二个参数，表示从上面位置开始进行查找。
console.log(string.includes('whale', 4)) //=>true

console.log(string.includes('whale', 6)) //=>false
```



### 八、slice()方法

`slice()方法` 是用来切割字符串的，切割完后返回一个新的字符串，并且不会改动原字符串。切割的位置总结就是**左闭右开**。

**语法：**

>```
>str.slice(beginIndex[, endIndex])
>```

**方法参数：**

- `{number} beginIndex` 切割字符串的开始位置。
- `{number} endIndex` 切割字符串的结束位置。

使用示例：

```js
const str = 'The quick brown fox jumps over the lazy dog.'

console.log(str.slice(31)) //=> "the lazy dog."

console.log(str.slice(4, 19)) //=> "quick brown fox"

console.log(str.slice(-4)) //=> "dog."

console.log(str.slice(-9, -5)) // => "lazy"
```



### 九、substring()方法

`substring()方法` 返回一个字符串在开始索引和结束索引之间的子集，用法跟 `slice()方法` 相识。它们之间的区别在于 `substring()方法`的参数不能接收负值（即负值都会按0来处理）。

**语法：**

>```js
>str.substring(indexStart[, indexEnd])
>```

**方法参数：**

- `{number} indexStart` 开始的下标位置。
- `{number} indexEnd` 结束的下标位置。

使用示例：

```js
const str = 'The quick brown fox jumps over the lazy dog.'

console.log(str.substring(0, 9)) //=>"The quick"

console.log(str.substring(1, -3)) // =>"T" 相当于str.substring(0, 1)
```



### 十、repeat()方法

`repeat方法 `返回一个新字符串，表示将原字符串重复n次。

**语法：**

>```js
>str.repeat(count)
>```

**方法参数：**

- `{number} count` 重复的次数，且不能是负值。

使用示例：

```js
### 重复3次
console.log('abc'.repeat(3)) //=> "abcabcabc"

### 重复2次
console.log('hello'.repeat(2)) //=>"hellohello"
```



### 十一、toString()方法

`toString()方法` 返回指定对象的字符串形式。

**语法:**

>```js
>str.toString()
>```

使用示例：

```js
### 返回指定对象的字符串形式。
const res = new String('hello world')

console.log(res) //=>String {"hello world"}

console.log(res.toString()) //=>"hello world"
```



### 十二、valueOf()方法

`valueOf()方法` 返回String对象的原始值。

**语法：**

>```js
>str.valueOf()
>```

使用示例：

```js
### 返回String对象的原始值。
const stringObj = new String('foo')

console.log(stringObj) // => "String { "foo" }"

console.log(stringObj.valueOf()) // => "foo"
```



### 十三、toString()方法和valueOf()方法的区别

`toString()方法` 和 `valueOf()方法` 的区别：

- 区别一：`valueOf()方法 `偏向于运算，`toString()方法` 偏向于显示。
- 区别二：在进行强制转换字符串时，优先调用的是 `toString()方法` ；进行强制转换数字时，优先调用的是 `valueOf()方法`。
- 区别三：使用运算操作符的时候，`valueOf()`的优先级高于`toString()`。



### 十四、toLowerCase()方法

`toLowerCase()方法` 会将调用该方法的字符串值转为小写形式，并返回。

**注意：**如果调用该方法的值不是字符串类型会被强制转换。

**语法：**

>```js
>str.toLowerCase()
>```

使用示例：

```js
### 字符串全部转小写
console.log('中文简体 zh-CN || zh-Hans'.toLowerCase());
// 中文简体 zh-cn || zh-hans

​console.log( "ALPHABET".toLowerCase() );
// "alphabet"
```



### 十五、toUpperCase()方法

`toUpperCase()方法` 将调用改方法的字符串全部转为大写格式并返回。

**注意：**如果调用该方法的值不是字符串类型会被强制转换。

**语法：**

>```js
>str.toLocaleUpperCase()
>```

使用示例：

```js
### 字符串全部转大写
console.log('apples'.toUpperCase()) //=>APPLES

console.log('helloworld'.toUpperCase()) //=>HELLOWORLD
```



### 十六、split()方法

