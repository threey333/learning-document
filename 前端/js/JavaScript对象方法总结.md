# JavaScript对象方法总结

## 一、常用的方法

### 1.1 Object.assign方法

`Object.assign` 方法的作用是：将一个或多个源对象的**所有可枚举的属性**拷贝到目标对象上，最后返回目标对象。

**语法：**

```js
Object.assign(target, ...sources)
```

**==注意：如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。==**

示例一：合并对象。

```javascript
// 合并对象
const o1 = { a: 1 }
const o2 = { b: 2 }
const o3 = { c: 3 }

const obj = Object.assign(o1, o2, o3)
console.log(obj) // => { a: 1, b: 2, c: 3 }
console.log(o1) // => { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变。
console.log(obj === o1) // => true 说明返回的对象就是目标对象o1

// 如果用ES6+的写法是这样的: 
console.log({...o1,...o2,...o3}) // => { a: 1, b: 2, c: 3 }
```

示例二：合并具有相同属性的对象。

```javascript
// 合并具有相同属性的对象
const o1 = { a: 100, b: 200, c: 300 }
const o2 = { b: 300, c: 50 }
const o3 = { c: 70 }
const obj = Object.assign(o1, o2, o3)
console.log(obj) // => {a: 100, b: 300, c: 70}
```

示例三：`Object.assign` 方法还可以合并 `symbol类型的属性`。

```javascript
// 还可以合并symbol类型的属性
const o1 = { [Symbol('foo')]: 200, a: 1 }
const o2 = { b: 3 }
const obj = Object.assign(o2, o1)
console.log(obj) // => {b: 3, a: 1, Symbol(foo): 200}
```

示例四：源对象 ==原型上的属性== 和 ==不可枚举的属性== 不能被拷贝。

```js
// 源对象原型上的属性和不可枚举的属性不能被拷贝。
const obj = Object.create(
  { name: '该对象已成为新对象的原型对象' },
  {
    a: {
      value: 2, //默认不可枚举
    },
    b: {
      value: 4,
      enumerable: true,
    },
  }
)
console.log(obj) // => {b: 4, a: 2}

const res = Object.assign({}, obj)
console.log(res) // 结果 => {b: 4}
```



### 1.2 Object.keys方法

`Object.keys` 方法返回自身对象所有可枚举属性的键名(不包括symbol类型值和原型上的属性)，这些键名组成一个数组并返回。

**语法：**

```js
Object.keys(obj)
```

示例一：返回自身对象中所有可枚举属性的键值。

```js
// 返回自身对象中所有可枚举的属性。
const obj = {
  a: 1,
  b: 2,
  c: 3,
}
console.log(Object.keys(obj)) // => ['a', 'b', 'c']
```

示例二： `Object.keys` 方法不能返回自身对象中的 `symbol类型属性` 和 `原型上的属性`。

```js
// Object.keys方法不能返回自身对象中的Symbol类型属性和原型上的属性。
const obj = {
  [Symbol('name')]: 'Rose',
  age: 18,
}
obj.__proto__.a = 1
console.log(Object.keys(obj)) // => ['age']
```

示例三：如果 `Object.keys` 方法的参数是一个字符串，则返回该字符串中每一字符的下标位置

```js
const obj = {
  [Symbol('name')]: 'Rose',
  age: 18,
}
obj.__proto__.a = 1
console.log(Object.keys(obj)) // => ['age']

//示例三: 如果Object.keys方法的参数是一个字符串，则返回该字符串中每一字符的下标位置
console.log(Object.keys('foo')) // => ['0', '1', '2']
```



### 1.3 Object.values方法

`Object.values` 方法返回自身对象所有可枚举属性的键值(不包括symbol类型值和原型上的属性)，这些键值组成一个数组并返回。

**语法:**

```js
Object.values(obj)
```

示例一：返回自身对象所有可枚举属性的键值。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
}

console.log(Object.values(obj)) // => [1, 2, 3]
```

示例二：`Object.values` 方法不能获取自身对象中的symbol类型属性的值和原型上属性的值。

```js
// Object.values方法不能获取自身对象中的symbol类型属性的值和原型上属性的值。
const obj = Object.create(
  { bar: 'bar' },
  {
    a: {
      value: 10,
      enumerable: true,
    },
    b: {
      value: 20,
      enumerable: true,
    },
    [Symbol('c')]: {
      value: 30,
    },
  }
)

console.log(obj) // => {a: 10, b: 20, Symbol(c): 30}
console.log(Object.values(obj)) // => [10, 20]
```

示例三：如果 `Object.values` 方法的参数是 `string`、`number`、`boolean` 类型，则会默认转换成一个对象，其中对于字符串的会返回每一个字符组成的数组，其它返回的是一个空数组。

```js
console.log(Object.values('bar')) // => ['b', 'a', 'r']

console.log(Object.values(true)) // => []

console.log(Object.values(10)) // => []
```



### 1.4 Objece.entries方法

`Object.entries` 方法会遍历对象自身所有可遍历属性(不含继承,也不含Symbol值)，结果返回一个键值对数组。

**语法：**

```js
Object.entries(obj)
```

示例一：返回自身对象所有可枚举属性的键值和键名。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
}
console.log(Object.entries(obj)) // => [['a', 1],['b', 2],['c', 3]]
```

示例二：利用 `Object.entries` 方法可以很方便的将该对象转为 `map` 数据结构。

```js
// 对象转map结构
const obj = {
  foo: 'foo',
  bar: 'bar',
}
console.log(new Map(Object.entries(obj))) // => {'foo' => 'foo', 'bar' => 'bar'}
```



### 1.5 Object.getOwnPropertyNames方法

`Object.getOwnPropertyNames` 方法获取一个对象自身所有属性的键名(包括不可枚举属性的键名但不包括symbol类型的键名)，这些键名组成数组并返回。

**语法：**

```js
Object.getOwnPropertyNames(obj)
```

示例一：获取对象自身所有属性的键名。

```js
// 对于数组
const arr = ['a', 'b', 'c']
console.log(Object.getOwnPropertyNames(arr)) // => ['0', '1', '2', 'length']

// 对于对象
const obj = {
  a: 1,
  b: 2,
  c: 3,
}
console.log(Object.getOwnPropertyNames(obj)) // => ['a', 'b', 'c']
```

示例二：`Object.getOwnPropertyNames(obj)` 方法也能获取对象自身不可枚举的属性。

```js
const obj = Object.create(
  { title: '原型对象' },
  {
    a: {
      value: 'a',
      enumerable: false,
    },
    b: {
      value: 'b',
      enumerable: false,
    },
  }
)
console.log(Object.getOwnPropertyNames(obj)) // => ['a','b']
```

示例三：`Object.getOwnPropertyNames(obj)` 方法不会获取原型链上的属性。

```js
// ChildClass构造函数
function ChildClass() {
  this.prop = 5
  this.method = function () {}
}

ChildClass.prototype.show = function () {
  console.log('show')
}
ChildClass.prototype.speak = function () {
  console.log('speak')
}

const obj = new ChildClass()
console.log(obj) // =>  {prop: 5, method: ƒ}
console.log(Object.getOwnPropertyNames(obj)) // => ['prop', 'method']
```



### 1.6 Object.getOwnPropertySymbols方法

`Object.getOwnPropertySymbols(obj)` 方法获取一个对象自身所有 `symbol类型属性` 的键名，这些键名组成一个数组并返回。

**语法：**

```js
Object.getOwnPropertySymbols(obj)
```

示例一：获取对象中所有 `symbol` 类型属性的键值。

```js
const obj = {
  [Symbol('a')]: 1,
  [Symbol.for('b')]: 2,
}

const objectSymbols = Object.getOwnPropertySymbols(obj)
console.log(objectSymbols.length) // => 2
console.log(objectSymbols) // => [Symbol(a), Symbol(b)]
console.log(objectSymbols[0]) // => Symbol(a)
console.log(obj[objectSymbols[0]]) // => 1
</script>
```



### 1.7 Object.prototype.hasOwnProperty方法

`Object.prototype.hasOwnProperty()`方法会返回一个布尔值,表示该对象内所有自身的属性是否具有指定的属性，如果没有返回false,否则返回true。

**语法：**

```js
obj.hasOwnProperty(prop)
```

示例一：使用 `hasOwnProperty` 方法判断属性是否存在。

```js
const obj = {
  [Symbol('name')]: 'aaa',
  foo: 'foo',
  bar: 'bar',
  propOne: undefined,
  propTwo: null,
}
console.log(obj.hasOwnProperty('foo')) // => true
console.log(obj.hasOwnProperty('bar')) // => true
console.log(obj.hasOwnProperty('propOne')) // => true
console.log(obj.hasOwnProperty('propTwo')) // => true

delete obj.propTwo
console.log(obj.hasOwnProperty('propTwo')) // => false
```

示例二：`hasOwnProperty` 方法只会检测对象自身的属性，而不会根据原型链去检测属性。

```js
o = new Object();
o.prop = 'exists';

o.hasOwnProperty('prop');             // => true
o.hasOwnProperty('toString');         // => false
o.hasOwnProperty('hasOwnProperty');   // => false
```

示例三：利用 `hasOwnProperty` 方法可以对 `for...in` 循环进行优化。

```js
// 优化for...in循环
for (let key in object) {
  if (Object.hasOwnProperty.call(object, key)) {
    ....
  }
}
```



### 1.8 Object.getOwnPropertyDescriptors方法

`Object.getOwnPropertyDescriptors(obj)` 用来获取一个对象自身所有属性的描述符。

**语法：**

```js
Object.getOwnPropertyDescriptors(obj)
```

对于属性描述符各字段的解释：

>- `configurable`：可配置的，例如可以删除（默认为true）。
>- `enumerable`：可枚举的（默认为true）。
>- `writable`：可修改的（默认为true）。
>- `value`：属性的值。

示例：获取对象中所有属性的描述符。

```js
const obj = {
  a: 1,
  b: 2,
  c: 3,
}
console.log(Object.getOwnPropertyDescriptors(obj))
/* 
结果:
  => a: {value: 1, writable: true, enumerable: true, configurable: true}
  => b: {value: 2, writable: true, enumerable: true, configurable: true}
  => c: {value: 3, writable: true, enumerable: true, configurable: true}
*/
```



### 1.9 Object.defineProperties方法

`Object.defineProperties` 方法会直接在一个对象上定义新的属性或修改现有属性，并返回该对象。

>**注意：**
>
>- 一：该方法向该对象中添加新属性的时候可对 `configurable`、`enumerable`、`writable` 和 `value`进行配置。
>- 二：添加的新属性的 `enumerable` 默认为false。

**语法：**

```js
Object.defineProperties(obj, props)
```

示例：

```js
const obj = {}
const res = Object.defineProperties(obj, {
  property1: {
    value: true,
    writable: true,
  },
  property2: {
    value: 'Hello',
    writable: false,
  },
})

console.log(obj) // => {property1: true, property2: 'Hello'}
console.log(res) // => {property1: true, property2: 'Hello'}
console.log(obj === res) // => true
```



## 二、不常用但也要了解的方法

### 2.1 Object.is方法

ES5检测两个值是否相等，只有两个运算符：(1).相等运算符( `==` )、(2).严格相等运算符( `===` )。

但是，他们都有缺点：

- 一：相等运算符 `==` 检测的规则是==**值相等，类型不相等**==就属于相等，这样会造成检测不严谨。
- 二：严格相等运算符 `===` 对于 `0`、`-0` 和 `NaN` 比较不了。

```js
// 相等运算符
console.log(1 == '1') // => true

// 严格相等运算符
console.log(+0 === -0);  // => true
console.log(NaN === NaN);  // => false
```

 

为了能解决这些问题，后面ES6就推出了 `Object.is(value1，value2)` 方法进行处理。

`Object.is()` 方法判断两个值是否为同一个值。

**语法：**

```js
Object.is(value1, value2)
```

示例：

```js
console.log(Object.is(+0, -0)); // => false
console.log(Object.is(NaN, NaN)); // => true
```



### 2.2 Object.create方法

`Object.create` 方法会将一个原来的对象变成新对象的原型对象，而新对象则成为实例对象。ES5就是通过使用 `Object.create()方法`实现原型式继承。

**语法：**

```js
Object.create(proto，[propertiesObject])
```

该方法接收两个参数：

1. 第一个参数是 `Object类型`，即该参数是作为新的对象的原型对象。
2. 第二个参数是可选参数，即该参数是可以作为新的对象的自身属性。

示例：

```js
const person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van'],
}
let anotherPerson = Object.create(person, {
  name: {
    value: 'Greg',
  },
})
console.log(anotherPerson) 
// =>
//{name: "Greg"}
//	name: "Greg"
//	[[Prototype]]: Object
//		friends: (3) ["Shelby", "Court", "Van"]
//		name: "Nicholas"
//		[[Prototype]]: Object

console.log(anotherPerson.name) //=> "Greg"
```

