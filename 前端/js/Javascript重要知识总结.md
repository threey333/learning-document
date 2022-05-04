# Javascript重要知识总结

## 一、undefined和null

### 1.1 undefined指的是什么

`undefined`指的是：原始值未初始化。

>什么是原始值？
>
>- 原始值是存储在栈中的**基本数据类型**的字段。
>- 基本数据类型有：`undefined`、`null`、`string`、`boolean`、`number`。

例如：

```js
let a
console.log(a)  //undefined
```

上面的声明了一个变量，用来存放基本数据类型的值，只不过它没有被初始化，所以默认`undefined`。



### 1.2 undefined出现场景

一、已声明未初始值的**基本数据类型变量**：

```js
let a    //a代表的是基本数据类型的变量
console.log(a)  //结果undefined
```

二、访问对象不存在的属性：

```js
const a={}
console.log(a.name)   //结果undefined
```

三、函数没有返回值的情况：

```js
function test(){}
console.log(test())   //结果undefined
```

四、函数有形参，但是实参没有传入值的情况：

```js
function test(params){
  console.log(params)
}
console.log(test())   //结果undefined
```

五、void表达式

```js
console.log(void 1)   //结果undefined
```



### 1.3 undefined相关隐式转化

```js
console.log(Boolean(undefined))    //false
console.log(String(undefined))     //undefined
console.log(Number(undefined))     //NaN
```



### 1.4 null指的是什么

`null`指的是：对象的初始值，即空对象的引用。

```js
const a = null
console.log(a)  //null
```

这样定义变量a，说明变量a是引用类型的变量，即`Object对象`，只不过该对象的引用指向的是null。



### 1.5 null相关隐式转化

```js
console.log(Boolean(null))    //false
console.log(String(null))     //null
console.log(Number(null))     //0
```



## 二、JS的内置对象和宿主对象

### 2.1 JS内置对象分类

- 原始值：`undefined`、`null`、`string`、`number`、`boolean`。
- 引用值：`Object`。
- 基本包装对象：`String`、`Number`、`Boolean`。
- 构造函数：`Object`、`Array`、`Date`、`Function`、`Error`、`RegExp`。
- 使用键的集合对象：`Map`、`Set`、`WeakMap`、`WeakSet`。
- 控制抽象对象：` Promise`、`Generator` 等
- 其它对象：`Math`、`JSON`、`arguments`。



### 2.2 JS宿主对象分类

所有的BOM和DOM都是宿主对象。

- DOM：`document`对象（也是DOM的顶级对象）。
- BOM：`window`对象（也是BOM的顶级对象）、`location`对象、`history`对象、`navigator`对象



## 三、闭包

### 3.1 什么是闭包

#### 3.1.1 各权威书籍对闭包的解释

**一、犀牛书的解释：**

- 函数变量可以保存在函数作用域内，这就是闭包。

**二、高级程序设计的解释：**

- 闭包是指有权访问另一个函数作用域中的变量的函数（函数没导出）。

**三、你不知道的javascript的解释**

- 当函数可以记住并访问所在词法作用域时，就产生闭包，即使函数是在当前词法作用域之外执行。

**从这三本书中总结闭包：**

>**==函数的执行，导致函数被定义。这就是闭包（结论）==**
>
>也就是说，闭包和函数的定义有关



#### 3.1.2 函数的执行，导致函数被定义就是闭包的理解

举个例子吧：

```js
function fn1() {
  var a = 1 
  var fn2 = function () {    //fn1函数的执行导致fn2函数被定义，此时闭包产生。
    console.log(a)
  }
  fn2()
}
fn1()  //执行fn1函数
```

上面的代码示例是不是闭包？答案是闭包。

根据前面的总结的结论：**函数的执行，导致函数被定义。**当`fn1函数`执行的时候，导致`fn2函数`被定义了，此时就产生了闭包。



### 3.2 闭包的几种常见形式

**第一种：函数的返回值是函数**

```js
function foo() {
  const arr = [1, 2, 3, 4, 5, 6, 7, 8]
  return function bar() {
    console.log(arr)
  }
}
const result = foo()
result()    //[1, 2, 3, 4, 5, 6, 7, 8]
```

**第二种：函数的返回值是变量，该变量是函数**

```js
function foo() {
  const arr = [1, 2, 3, 4]
  const n = function bar() {
    console.log(arr)
  }
  return n
}
foo()()    //[1, 2, 3, 4]
```

**第三种：全局变量定义的闭包**

```js
let outer
function foo() {
  const arr = [1, 2, 3, 4, 5, 6]
  outer = function bar() {
    console.log(arr)
  }
}
foo()
outer() 	//[1, 2, 3, 4, 5, 6]
```

**第四种：函数的参数方式：**

```js
const outer = function (fn) {  console.log(fn()) //结果[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]};(function foo() {  const n = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  const inner = function () {    return n  }  outer(inner)})()
```



### 3.3 闭包的应用场景

在认识闭包的应用场景之前，首先了解一些基础概念。

**一、函数作用域**

定义在函数中的`参数`和`变量`在函数外部是不可见的。

**二、块级作用域（私有作用域）**

任何一对`花括号(即“{}”)`中的语句都属于一个块，在这之中的所有变量在代码块外都是不可见的，我们称之为块级作用域。

**三、私有变量**

私有变量包括`函数的参数`，`局部变量`和`函数内部定义的其他函数`。

**四、静态私有变量**

私有变量是每个实例对象独立的，而==静态私有变量是共用的==。

**五、特权方法**

有权访问私有变量的方法称为特权方法。

#### 3.3.1 模仿块级作用域

比如我们可以使用闭包能使下面的代码按照我们预期的进行执行（每隔1s打印 0,1,2,3,4）。

```javascript
for(var i = 0; i < 5; i++) {
  (function(j){    //这里的形参j作为私有变量保存着i的值。
    setTimeout(() => {
      console.log(j);
    }, j * 1000);
  })(i)
}
```

> 我们应该尽量避免往全局作用域中添加变量和函数。通过闭包模拟的块级作用域



#### 3.3.2 私有变量

JavaScript中没有私有成员的概念，所有属性都是公有的。但是有私有变量的概念，**任何在函数中定义的变量，都可以认为是私有变量**，因为在函数的外部不能访问这些变量。

**私有变量包括`函数的参数`，`局部变量`和`函数内部定义的其他函数`。**

```js
function add(a, b) {
  var sum = a + b;
  return sum;
}
```

在add函数内部，a,b,sum这三个变量是私有变量，只能在函数内部才能访问，函数外部是访问不到它们的。

但是通过**闭包**的方式，我们是有权访问它们的。

做法：

>- 通过在函数内部创建一个闭包，闭包可以通过自己的`作用域链`就可以访问这些私有变量。
>- 利用闭包，我们就可以**创建用于访问私有变量的公有方法（也称为特权方法）**。

有两种在对象上创建特权的方法：

第一种：**在构造函数中定义特权方法**

```js
function MyObject() {  
  // 私有变量和私有函数  
  var privateNumber = 10  
  function privateFunction() {    
    return false  
  }  
  // 特权方法  
  this.publicMethod = function () {    
    privateNumber++    
    return privateFunction  
  }  
  this.publicGetNumber = function () {    
    return privateNumber  
  }
}
//测试检验：
const obj1 = new MyObject()
const obj2 = new MyObject()
const foo = obj1.publicMethod()
console.log(foo()) //false
console.log(obj1.publicGetNumber()) //11
console.log(obj2.publicGetNumber()) //10
```

==能够在构造函数中定义特权方法，是因为特权方法作为闭包有权访问在构造函数中定义的所有变量和函数。==

**注意：**

>- **每一个由构造函数实例出来的对象，它通过特权方法访问私有变量和私有函数都是不一样的。因此每次调用构造函数实例化对象都会重新创建特权方法。**
>
>![image-20210813033730355](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021841.png)



第二种：**利用函数形参私有变量配合特权方法，可以隐藏哪些不应该被直接修改的数据**

```js
function Foo(name) {
  // 形参作为私有变量
  this.getName = function () {
    return name
  }
}
const foo1 = new Foo('threey')
const foo2 = new Foo('Ethon')
console.log(foo1.name) //undefined
console.log(foo1.getName()) //threey
console.log(foo2.getName()) //Ethon
```

可以发现`foo1.name`你是访问不到函数形参私有变量的，但是通过特权方法`getName()`可以访问私有变量`name`。

**原因：**

>- 因为该特权方法是在构造函数内部定义的，作为**闭包**可以通过作用域链访问`name`。
>- 私有变量 `name`在 `Foo`的每个实例中都不一样，因此每次调用构造函数都会重新创建该方法。

**总结：**

>- 在构造函数中定义特权方法的缺点就是你必须使用构造函数模式。
>- 使用静态私有变量实现特权可以避免这个问题。



#### 3.3.3 静态私有变量

创建特权方法也可以通过**在私有作用域（块级作用域）中定义私有变量或私有函数函数来实现**。

```js
;(function (params) {  // 这种模式下，userName是静态私有变量  
  var userName = 'yyy'  
  // 这种模式下，methods是静态私有变量  
  function methods() {    
    console.log('私有作用域中的私有函数')    
    return '私有作用域中的私有函数'  
  }  
  var Person = function (value) {    
    userName = value  
  }  
  //下面都是特权方法  
  Person.prototype.getName = function () {    
    console.log(userName)    
    return userName  
  }  
  Person.prototype.setName = function (value) {    
    userName = value  
  }  
  Person.prototype.getMethdos = function () {   
    return methods  
  } 
  params.Person = Person})(window)
//测试检验
const p1 = new Person('蛤蟆龟')

p1.getName() //蛤蟆龟

const result1 = p1.getMethdos()

result1() //私有作用域中的私有函数

const p2 = new Person('喷火龙')p1.getName() //喷火龙

p2.getName() //喷火龙

//从这里就可以知道userName是静态私有属性。
const result2 = p2.getMethdos()

result2() //私有作用域中的私有函数
```

上面代码通过一个匿名函数实现块级作用域，在块级作用域中 变量 `userName` 只能在该作用域中访问，同样的通过**闭包(作用域链)的方式**实现 `getName` 和 `setName` 来访问 `userName`, 而 `getName` 和 `setName` 又是原型对象的方法，所以它们成了 `Person` 实例的共享方法。

> **这种模式下，`userName` 就变成了一个静态的、由所有实例共享的属性（也叫静态私有属性）。在一个实例上调用 `setName()` 会影响所有的实例。**



### 3.4 闭包的一些面试题

```js
function fun(n, o) {
  console.log(0)
  return {
    fun: function (m) {
      return fun(m, n)
    },
  }
}
// 第一种：
var a = fun(0)
a.fun(1)
a.fun(2)
a.fun(3)

// 第二种
var b = fun(0)
.fun(1)
.fun(2)
.fun(3)

// 第三种：
var c = fun(0).fun(2)
c.fun(2)
c.fun(3)
```



### 3.5 总结

当在函数内部定义了其他函数，就创建了闭包。闭包有权访问包含函数内部的所有变量，原理如下：

- 在后台执行环境中，闭包的作用域链包含它自己的作用域链、包含函数的作用域和全局作用域
- 通常，函数的作用域及其所有变量都会在函数执行结束后销毁
- 但是，当函数返回来了一个闭包，这个函数的作用域将一直在内存中保存在闭包不存在为止。

使用闭包可以在JavaScript中模仿块级作用域（JavaScript本身没有块级作用域的概念），要点如下:

- 创建并立即调用一个函数，这样既可以执行其中的代码，又不会在内存中留下对该函数的引用
- 结果就是函数内部的所有变量都会被销毁 -- 除非将某些变量赋值给了包含作用域(即外部作用域)中的变量

闭包还可以用于在对象中创建私有变量，相关概念和要点如下。

- 即使JavaScript中没有正式的私有对象属性的概念，但可以使用闭包来实现公有方法，而通过公有方法可以访问在包含作用域中定义的变量
- 可以使用构造函数模式，原型模式来实现自定义类型的特权方法也可以使用模块模式、增强的模块模式来实现单例的特权方法



## 四、this指向

### 4.1 理解this

- `this指向`是在函数执行的时候进行绑定的，而不是在函数声明的时候进行绑定的。
- **在函数内部，`this指向` 取决于函数被调用的方式，与函数声明的时间和位置无关。**



### 4.2 this关键字绑定规则

`this` 关键字绑定规则有四种：

- **默认绑定：**函数独立调用的时候就是默认绑定。
- **隐式绑定：**函数当作对象的一个方法被调用，这种就是隐式绑定。
- **显示绑定：**通过`call，bind，apply`等方法改变 `this` 的绑定，这种就是显示绑定。
- **new绑定：**当构造函数实例化对象时，会使 `this的指向 ` 绑定在实例化对象上。这种就是new绑定。



### 4.3 默认绑定

`默认绑定` 就是当**函数独立调用**的时候，`this指向`绑定在==全局window==上（非严格模式下）。

- 严格模式下：`this`的默认值是`undefined`。
- 非严格模式下：`this`默认指向`全局对象window`。

```js
const foo = function () {
  console.log(this)
}
foo() //this
```

函数独立调用相当于是window全局对象调用该函数：

```js
console.log(window.foo() === foo()) //true
//都是指向window
```

>默认绑定可以理解为：函数独立调用模式。



### 4.4 隐式绑定

#### 4.4.1 隐式绑定

一、`隐式绑定` 就是**函数当作对象的一个方法被调用**的时候，`this指向`绑定在==调用该方法的对象==上。

```js
const obj = {
  a: 42,
  bar: function () {
    console.log(this) // 指向的是obj
    console.log(this.a) // 42
  },
}
// obj.bar()
obj['bar']()
```

>隐式绑定可以理解为：方法调用模式。

>通过 `. 运算符` 或者是 `[]运算符 ` 都是属于 方法调用模式。



#### 4.4.2 隐式绑定失败例子

隐式绑定失败的例子：

- 隐式丢失。
- 参数赋值。

**一、隐式丢失：**

对象外部的一个变量保存着对象内一个方法地址，当该变量执行的时候，相当于==函数独立调用==；此时`this指向`绑定在全局window上。

```js
const foo = function () {
  console.log(this)
}
const obj = {
  a: 'Ethon',
  foo: foo,
}
const res = obj.foo  // ===>相当于const res = foo

obj.foo() //obj.
res() //window
```

**二、参数赋值：**

当一个函数的实参传递的是一个对象方法地址，形参接收后执行相当于==函数独立调用==；此时`this指向`绑定在全局window上。

```js
const foo = function () {
  console.log(this)
}
const baz = function (fn) {
  // 参数是回调函数
  fn()
}
const obj = {
  id: 22,
  foo: foo,
}
baz(obj.foo) //window
```



### 4.5 显示绑定

`显示绑定`就是**通过使用 `call()`、`bind()`和 `apply()` 方法**显示把函数内部的`this指向`绑定在指定的对象上。

#### 4.5.1 call方法

`call方法`可以调用一个函数，并且可以指定这个函数内部的 `this`指向绑定在哪个对象上。

>`call()`的第一个参数就是函数内部`this`指向要绑定的新对象。

示例：

```js
//不使用call方法
const foo = function (a, b) {
  console.log(this)
}
foo() //函数内部this指向绑定在全局window上，相当于函数独立调用。

//使用call方法
const foo = function (a, b) {
  console.log(this)
}
foo.call([1, 2, 3]) //[1,2,3]   函数内部this指向绑定在call方法指定的对象。
```



#### 4.5.2 apply方法

`apply方法`可以调用一个函数，并且可以指定这个函数内部的 `this指向`绑定在哪个对象上。

>`apply()`的第一个参数就是函数内部`this`指向要绑定的新对象。

示例:

```js
//不使用apply方法
const bar = function (a, b) {
  console.log(this)
}
bar() //window

//使用apply方法
const bar = function (a, b) {
  console.log(this)
}
bar.apply({ name: 'rose' }) //{name: "rose"}  apply方法改变了函数内部的this指向。
```



#### 4.5.3 call方法和apply方法的区别

`call方法`和`apply方法`的区别：

- `call方法`有多个参数，除了第一个参数是改变函数内部`this`指向外，从第二个参数开始，对应的就是函数中的形参。
- `apply方法`只有两个参数，除了第一个参数是改变函数内部的`this`指向外，第二个参数就是数组或者伪数组。

>简单来说就是：`apply方法` 第二个参数使用的是参数数组，`call方法`从第二个参数开始使用的是参数列表。

示例：

```js
//call方法从第二个参数开始就是参数列表。
const fn1 = function (a, b, c, d) {
  console.log(this) //{}
  console.log('a => ' + a) //10
  console.log('b => ' + b) //20
  console.log('c => ' + c) //30
  console.log('d => ' + d) //40
}
fn1.call({}, 10, 20, 30, 40)

//apply方法第二个参数是伪数组或数组
const fn2 = function (e, f, g, h) {
  console.log(this) //{}
  console.log('e => ' + e) //1
  console.log('f => ' + f) //2
  console.log('g => ' + g) //3
  console.log('h => ' + h) //4
}
fn2.apply({}, [1, 2, 3, 4])
```



#### 4.5.5 call方法和apply方法用途总结

**用途总结：**

- 改变函数内部`this指向`。
- 借用其它对象的方法。
- 调用函数。

针对“借用其它对象的方法”这一用途进行说明：

```js
const Person1 = function () {
  this.name = '油炸皮卡丘'
  this.id = 101
}
const Person2 = function () {
  this.getname = function () {
    console.log(this.name)
    console.log(this.id)
  }
  Person1.call(this)  //通过call方法借用了Person1构造函数来实例化对象的属性。
}
const person = new Person2()
person.getname() // 油炸皮卡丘 101
```



#### 4.5.6 bind方法

`bind() ` 创建一个新的函数, 在 `bind()` 被调用的时候，这个新函数的 `this指向` 指定为`bind()` 的第一个参数，而剩余参数则将作为新函数的参数，供调用时使用。

示例：

```js
const moduleA = {
  name: 'threey',
  age: 22,
  getName: function () {
    console.log('this指向：', this)
    console.log(this.name)
  },
  getAge: function () {
     console.log('this指向：', this)
    console.log(this.age)
  },
}

// 未使用bind方法绑定函数的情况
moduleA.getName() 
// 结果:
// => {name: 'threey', age: 22, getName: ƒ, getAge: ƒ} 
// => threey

moduleA.getAge() 
// 结果:
// => {name: 'threey', age: 22, getName: ƒ, getAge: ƒ} 
// => 22

// 使用bind方法绑定函数，返回一个新函数并执行，查看情况
const newGetName = moduleA.getName.bind({
  name: 'Eson',
})
const newGetAge = moduleA.getAge.bind({
  age: 30,
})
newGetName() 
// 结果:
// => {name: 'Eson'}
// => 'Eson',

newGetAge() 
// 结果:
// => {name: 'Eson'}
// => 30
```



### 4.6 new绑定

`new绑定`就是使用 `new关键字` 初始化一个构造函数时，能使其构造函数内部的`this指向`绑定在实例化对象上。

示例：

```js
function Person(name, age) {
  this.name = name
  this.age = age
  console.log(this) //指向woman1这个构造函数实例对象上。
}
const woman1 = new Person('ethon', 20)
```



### 4.7 各种函数的this指向汇总

#### 4.7.1 构造函数中的this指向

构造函数中的 `this指向` 绑定在==构造函数的实例对象==。

```js
function Person(name, age) {
  this.name = name
  this.age = age
  console.log(this) //指向woman1这个构造函数实例对象上。
}
const woman1 = new Person('ethon', 20)  //对象实例化。
```



#### 4.7.2 DOM绑定事件的回调函数中this指向

DOM绑定事件的回调函数中的 `this指向` 绑定在==目标DOM元素对象==。

```html
<div class="container">
  <button id="btn">点击一下</button>
</div>
```

```js
情况一：
const btn = document.querySelector('button')
btn.onclick = function () {
  console.log(this)  //指向的是目标DOM元素对象,即button
}

情况二：
const btn = document.querySelector('button')
const obj = {
  show: function () {
    console.log(this)  //还是指向目标DOM元素对象，即button
  },
}
btn.onclick = obj.show
```



#### 4.7.3 addEventListener绑定函数的this指向

addEventListener绑定函数的 `this指向` 绑定在==目标DOM元素对象==。

```html
<div class="container">
  <button id="btn">点击一下</button>
</div>
```

 ```js
 情况一：
 const btn = document.querySelector('#btn')
 btn.addEventListener('click', function () {
   console.log(this)  // this指向的是目标DOM元素对象，即button
 })
 
 情况二：
 const btn = document.querySelector('#btn')
 const data = {
   show: function () {
     console.log(this)  //this指向的是目标DOM元素对象
   },
 }
 btn.addEventListener('click', data.show)
 ```



#### 4.7.4 定时器的回调函数this指向

定时器回调函数的 `this指向`绑定在全局对象window上。 

```js
setTimeout(function () {
  console.log(this)  //this指向window
}, 1000)
```



#### 4.7.5 闭包函数中的this指向

闭包函数中的`this指向`绑定在全局对象window上。相当于函数独立调用。

```js
//情况一：
const foo = function () {
  console.log(this) //this指向obj
  const bar = function () {
    console.log(this) //闭包函数内部this指向window对象。
  }
  bar()
}
const obj = {
  show: foo,
}
obj.show()

//情况二：
const bar = function () {
  console.log(this) //this指向obj
  return function baz() {
    console.log(this) //闭包函数内部的this指向全局对象window
  }
}
const obj = {
  show: bar,
}
obj.show()()
```



#### 4.7.6 箭头函数的this指向(重点)

箭头函数是没有自己的 `this指向`，其会继承离得最近的父函数执行上下文环境的 `this指向`。 

示例：

```js
const foo = function () {
  console.log(this) //obj
  const bar = () => {
    console.log(this.title) //箭头函数拿到title属性值了
    //其继承了父函数执行上下文环境的this指向。
  }
  bar()
}
const obj = {
  title: '箭头函数拿到title属性值了',
  foo,
}
obj.foo()
```

分析：由于`bar()`是箭头函数，其是没有自己的this指向，但会继承父函数执行上下文环境的 `this指向`；所有它拿到了obj对象的ttile属性值。

**箭头函数总结：**

- `this关键字`的四种绑定规则在箭头函数中不起作用。
- 箭头函数没有自己的`this指向`，其会继承离得最近的父函数执行上下文环境的 `this指向`。



## 五、高阶函数和回调函数

### 5.1 什么是高阶函数

`高阶函数`：函数的参数是函数，或者函数的返回值是函数。

**示例：**

```js
示例1：
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
arr.map(item => item * 2)

示例2：
function sum(arr, callback) {
  return callback(arr)
}
const res = sum(arr, function (params) {
  let result = 0
  for (const item of arr) {
    result += item
  }
  return result
})

示例3：
 new Promise(function (res, rej) {})
```

示例一：`map函数` 中的参数是函数，那么`map函数` 就是个高阶函数

示例二：`sum函数` 中的参数有一个是函数，那么`sum函数`就是个高阶函数。

示例三：`promise构造函数` 中的参数也是函数，那么 `promise构造函数`就是个高阶函数。



### 5.2 高阶函数的用处

因为函数的参数是函数或者函数的返回值函数，那么该函数就是 `高阶函数` 。

- 用处一：对于**参数是函数的情况**，可以理解为==业务可以抽离为函数==。
- 用处二：对于**返回值是函数的情况**，可以理解为==函数是功能的延续==。

#### 5.2.1 用处一、业务可以抽离为函数

用ajax发送请求举例说明，加深对该用处的理解：

```js
const getInfoMessage = function (userId, callback) {
  $.ajax({
    type: 'get',
    url: 'https://api.apiopen.top/getJoke',
    data: userId,
    dataType: 'json',
    success: callback,
  })
}
getInfoMessage('123', function (data) {
  console.log(data)
  //结果：result: (20) [{…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}, {…}]
})
```

在这个例子中，ajax发送网络请求，当请求成功后返回数据（也就是触发了请求成功后的异步回调函数）。那么成功后代码逻辑是可以抽离出来成为函数，然后该函数中处理业务。



#### 5.2.2 用处二、函数是功能的延续

因为返回值是函数（实际上是利用了闭包原理），所以该返回值保留了该函数的功能，==**重复利用程度高**==。

**示例：**

```js
//返回值不是函数,这样会造成很多重复的逻辑代码（复用程度低）
const isString = function (obj) {
  return Object.prototype.toString.call(obj) === '[object String]'
}
const isArray = function (obj) {
  return Object.prototype.toString.call(obj) === '[object Array]'
}
const isNumber = function (obj) {
  return Object.prototype.toString.call(obj) === '[object Number]'
}


//当使用一个函数在它们的基础之上进行包装，返回值返回该函数的功能，这样能减少重复的代码，提高代码的重复利用。
const isType = function (type) {
  // 返回一个匿名函数
  return function (obj) {
    return Object.prototype.toString.call(obj) === `[object ${type}]`
  }
}
const isString = isType('String')
const isArray = isType('Array')
const isNumber = isType('Number')

// 利用高阶函数实现功能的延续。
// 判断是否是字符串
console.log(isString('你好啊')) //true
// 判断是否是数组
console.log(isArray('你好啊')) //false
console.log(isArray([1, 2, 3])) //true
// 判断是否是数字
console.log(isNumber(1)) //true
```

**这个用处在 `react的高阶组件中用的多`，所以这里要理解。**



### 5.3 什么是回调函数

根据概念：**参数是函数，那么作为该参数的函数是个回调函数。**

回调函数又跟 `事件轮询` 有关，因为`事件轮询`的核心就是回调函数。

**示例：**

```js
示例1：
arr.sort((a, b) => b - a)
//(a,b)=>b-a函数是个回调函数。

示例2：
setTimeout(() => {
  console.log('我是异步任务的回调函数')
}, 1000) 
//定时器中的参数是函数,所以()=>{console.log('我是异步任务的回调函数')}是个回调函数

示例3：
$.ajax({
  type: 'get',
  url: 'https://api.apiopen.top/getJoke',
  data: userId,
  dataType: 'json',
  success: callback,
})
//ajax发送网络请求是个异步任务，里面成功的时候有个回调函数callback.
```

示例一：(a,b)=>b-a函数是个回调函数。

示例二：定时器中的参数是函数,所以()=>{console.log('我是异步任务的回调函数')}是个回调函数。

示例三：ajax异步任务中的callback是个回调函数。



### 5.4 回调函数和异步任务之间的关系

#### 5.4.1 异步任务一定有回调函数

在此之前呢，我一直弄不清除回调函数和异步任务之间的关系，直到去读阮一峰大神的技术文章才深刻明白之间的关系。

阮一峰大神在技术文章中这么说到：异步任务必须指定回调函数，==**当主线程开始执行异步任务时，就是执行对应的回调函数**==。

![image-20210903231649070](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210903231700.png)



#### 5.4.2 回调函数的分类

回调函数又分为 `同步回调函数` 和 `异步回调函数` 。

- 同步任务中的回调函数就是 `同步回调函数`。
- 异步任务中的回调函数就是 `异步回调函数`。

**示例：**

```js
示例一：
arr.sort((a, b) => b - a)

示例二：
setTimeout(() => {
  console.log('我是异步任务的回调函数')
}, 1000)
```

- 示例一解释：`sort函数` 是同步任务，它会优先在js引擎的主线程中执行。而它里面的回调函数故而是 `同步回调函数`。
- 示例二解释：`setTimeout`是个异步任务，异步任务会开启异步线程执行，等执行完后会返回回调函数，而这个回调函数故而是 `异步回调函数`。

**总结：**

>**回调函数不一定是异步任务,但是异步任务一定有回调函数。**





## 六、认识浏览器的线程

### 6.1 浏览器是多线程的

浏览器是多线程的，它们在内核的控制下互相配合以保持同步。

浏览器的线程有：

- 一、`GUI渲染线程`：负责渲染页面，比如解析DOM、BOM、CSS。

  **它和JS引擎线程是互斥的，即JS线程工作的时候，GUI渲染线程就会停下工作，等JS引擎线程执行完任务后再执行。**

- 二、`JS引擎线程`：单线程工作，负责解析执行javascript脚本代码。**我们常说的javascript是单线程的，就是因为浏览器环境中只有一个执行javascript脚本代码的线程在工作。**

- 三、`事件触发线程`：当事件符合条件被触发时，该线程会返回一个==对应事件的回调函数==并添加到消息队列中，等待 `JS引擎主线程` 来处理。

- 四、`定时器触发线程`：因为 `JS引擎线程` 自身并没有调度时间的能力，只能靠浏览器开启 `定时器触发线程`来记时间，当时间到了后会返回一个回调函数到消息队列中，然后等待 `JS引擎线程` 来处理该回调任务。

- 五、`http网络请求线程`：http网络请求会开启一个请求线程，等请求完成并有结果后会返回一个回调函数到消息队列中，等待 `JS引擎线程`来处理。

- 六、`EventLoop(事件循环)处理线程`：简单来说就是**处理轮询消息队列（消息队列有宏任务消息队列和微任务消息队列）这个后面会讲。**

### 6.2 JS引擎线程是单线程的

浏览器环境中只有一个执行javaScript脚本代码的线程在工作。

**单线程最大的特点就是：同一个时间只能执行一个任务，当执行完这个任务后才能执行后面的任务。**



### 6.3 认知总结

**总结：**

>- GUI渲染线程、JS引擎线程、事件触发线程是浏览器的常驻线程。
>- **事件触发线程、定时器触发线程、http网络请求线程可以理解为`异步线程`（后面讲到异步任务会再讲解）。**
>- 事件循环线程中有两个消息队列：一个是`宏任务消息队列`、另一个是`微任务消息队列`。

只有对浏览器的多个线程了解后，后面的宏任务、微任务、事件轮询等待才会有深刻的理解。



## 七、同步任务和异步任务

javaScript脚本代码中有两种任务：

- 同步任务
- 异步任务

==同步任务可以理解为同步函数，异步任务可以理解为异步函数。==

### 7.1 同步任务

同步任务指的是：**在JS引擎主线程中排队执行的任务，只有前一个任务执行完才会执行后一个任务。**

###  7.2 异步任务

异步任务指的是：**它不会在主线程的执行栈中执行，而是会开启异步线程来执行。**当该任务执行完后会返回一个异步回调函数到消息队列中，然后等待主线程处理执行。

**异步任务代码示例：**

```js
//2.1 定时器异步任务
setTimeout(() => {
  // 异步回调函数
  console.log(
    '一，异步任务开启异步线程，在异步线程执行完后返回一个异步回调函数到消息队列中'
  )
}, 10)

// 2.2 DOM绑定事件异步任务
const h1 = document.querySelector('h1')
h1.addEventListener('click', () => {
  console.log('DOM绑定事件异步回调函数')
})

// 2.3 http网络请求(异步任务)
const XMR = new XMLHttpRequest()
XMR.open('get', 'https://api.apiopen.top/getJoke')
XMR.send()
XMR.onload = function () {
  const res = JSON.parse(XMR.response)
  console.log(res)
}
```

注意：

>异步任务又可以分为 `宏任务` 和 `微任务`。



### 7.3 同步任务和异步任务的执行流程

![image-20210905020449232](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210905020458.png)



## 八、宏任务和微任务

### 8.1 宏任务、微任务

**宏任务有**：==注意：存在表示的是该环境能实现该功能==

| #                     | 浏览器环境 | Node环境 |
| --------------------- | :--------: | :------: |
| I/O流                 |    存在    |   存在   |
| setTimeout            |    存在    |   存在   |
| setTnterval           |    存在    |   存在   |
| setImmediate          |   不存在   |   存在   |
| requestAnimationFrame |    存在    |  不存在  |
| ajax请求              |    存在    |   存在   |
| UI渲染                |    存在    |  不存在  |
| 整段 script脚本       |    存在    |  不存在  |
| DOM绑定事件回到函数   |    存在    |  不存在  |

**微任务有：**

| #                                            | 浏览器环境 | Node环境 |
| -------------------------------------------- | :--------: | :------: |
| process.nextTick                             |   不存在   |   存在   |
| MutationObserver                             |    存在    |  不存在  |
| Promise.then、Promise.catch、Promise.finally |    存在    |   存在   |
| Async函数中的Await                           |    存在    |   存在   |



### 8.2 事件循环与宏任务、微任务（重点）

#### 8.2.1 宏任务和微任务的执行顺序

宏任务和微任务的执行顺序在网上有两种说法：

- 第一种是：先执行宏任务，后执行微任务。
- 第二种是：先执行微任务，后执行宏任务。

**说明：**

> - 第一种认为是先宏任务后微任务的，是把 `整体script脚本` 视为了宏任务。
> - 第二种认为是先微任务后宏任务的，是忽略了 `整体script脚本` 这一宏任务，进而认为 **JS引擎主线程的执行完同步任务后就开始先执行微任务，然后不断地事件循环。**

**加深理解：**

- 引入微任务的初衷是为了解决异步回调函数的问题。而最终采用的方案是：**==将异步回调函数放到当前宏任务的末尾。==**

- 每一轮"事件循环"都可以认为是 ==宏任务一微任务==。

**经典宏任务和微任务执行顺序代码：**

```js
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```

代码说明：

1. `console.log('one')` 是同步任务，所以先在执行栈中执行。
2. `Promise.then()` 是微任务，所以当执行栈中的同步任务执行完后，会检查微任务队列中的回调函数，然后将其压入到执行栈中执行。
3. `setTimeout(fn,0)` 是在下一轮”事件循环“开始执行。
4. 最后打印结果就是：one two three。



#### 8.2.2 Promise的链式 then()执行顺序

在正式开始之前，我们先来看以下这段代码：

```js
new Promise((r) => {
  r();
})
  .then(() => console.log(1))
  .then(() => console.log(2))
  .then(() => console.log(3))

new Promise((r) => {
  r();
})
  .then(() => console.log(4))
  .then(() => console.log(5))
  .then(() => console.log(6))
```

答案：

```js
1
4
2
5
3
6
```

`Promise链式then()`的执行顺序是，**只有前一个==then微任务执行完==，下一个==then微任务才能进微任务队列中等待执行==**。



#### 8.2.2 消息队列含有宏任务队列和微任务队列

`事件轮询处理线程`负责处理消息队列中的事件函数，当主线程的执行栈为空，就会告诉 `事件轮询处理线程` 说我要拿消息队列中的事件回调函数，然后将其压入执行栈中执行。

**注意：**

>- 当执行栈中为空的时候，事件轮询处理线程会优先检擦 `微任务的消息队列` 中是否事件回调函数再等待，如果有就将其压入到JS主线程中执行。如果没有则当前事件循环结束，进行新的一轮事件循环处理。
>- **处理完微任务的消息队列后，说明当前一轮”事件循环“也处理完毕。**然后进行新的一轮”事件循环“处理。



#### 8.2.3 事件轮询流程（重点）

`JS引擎主线程` 从任务队列中读取事件，这个过程是循环不断的。这个运行机制称为 `事件循环Event Loop`。

**事件轮询示意图：**

![image-20210905015135515](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210907110534.png)



**浏览器环境下EventLoop执行流程：**

1. 一开始 `整段script脚本`作为第一个宏任务执行。
2. ==执行过程中同步代码直接执行，异步任务开启异步线程执行，执行完后的结果以回调函数的形式返回==；
3. 宏任务回调函数放入到宏任务队列中；微任务回调函数放入微任务队列中。
4. 当第一个宏任务执行完毕出栈后（就是script脚本中的同步代码都执行完），会检查微任务队列，如果有任务则依次压入执行栈中执行，直到微任务队列为空。
5. 微任务在执行的过程中也是==同步代码直接执行，异步代码则开启异步线程执行，执行完后的结果以回调函数的形式返回；==
6. `GUI渲染线程`开始执行工作。
7. 检查是否有 `Web worker任务` ，有则执行。
8. 新的一轮”事件循环“开始，取宏任务队列中首个新的宏任务放入到主线程中执行，过程回到==步骤2==。
9. 后面依次循环，直到宏任务和微任务队列都为空。

**注意：**

>- 像`Promise.then()等微任务`，==只有前一个的微任务执行完，才轮到下一个then微任务进入微任务队列中。==
>- 像`await命令的微任务`，它能起到等待的效果(即，`await代码`后面的代码都要等它有结果后才能执行)，并且能跳出当前微任务上下文环境转而去执行其它的同步代码。

**代码简单演示事件循环执行流程：**

```js
setTimeout(() => console.log(4), 10)  //异步宏任务

new Promise(resolve => {
  resolve('异步回调函数中的微任务')
}).then(res => {
  console.log(res)
})

new Promise(resolve => {
  resolve()
  console.log(1)
})
  .then(() => {
  console.log(3)
  console.log(5)
  setTimeout(() => {
    console.log('定时器任务')
  }, 10)
})
  .then(() => {
  console.log(6)
  console.log(7)
})
console.log(2)

//结果：1、2、异步回调函数中的微任务、3、5、6、7、4、定时器任务
```

- 步骤1：第一轮---先是整段`script脚本`作为第一个宏任务执行。
- 步骤2：执行的过程中，同步代码直接在主线程的执行栈中执行出结果（此时已打印出 `1` 、 `2` ）。异步代码则开启异步线程，在该线程中执行完后返回一个异步回调函数。
- 步骤3：如果该异步回调函数是微任务回调函数，则放入到微任务队列中，否则放入到宏任务队列中。
- 步骤4：第一个宏任务执行完后，会检查微任务队列中是否有任务，检查发现有任务；那么依次压入执行栈中执行，此时打印 `异步回调函数中的微任务`、`3`、`5`、`6`、`7`。
- 在执行微任务过程中，也会含有异步代码，那么该异步代码同样也是会开启异步线程来执行，执行完毕后返回一个回调函数。步骤回到 `步骤3`。
- 第二轮：执行完微任务后，会开启新的一轮“事件循环”，取宏任务队列中首个新的宏任务压人到执行栈中执行，回到 `步骤2`开启不段循环。打印 `4`、`定时器任务`
- 最后宏任务队列和微任务队列都为空，事件循环结束。



#### 8.2.4 事件轮询经典面试题

**第一道：**

```js
Promise.resolve().then(()=>{
  console.log('Promise1')  
  setTimeout(()=>{
    console.log('setTimeout2')
  },0)
});
setTimeout(()=>{
  console.log('setTimeout1')
  Promise.resolve().then(()=>{
    console.log('Promise2')    
  })
},0);
setTimeout(()=>{
  console.log('setTimeout3')
  Promise.resolve().then(()=>{
    console.log('Promise3')    
  })
},0);
console.log('start');

//结果：start、Promise1、setTimeout1、Promise2、setTimeout2
```

**第二道：**

```js
async function async1() {
  console.log('async1 start')
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2')
}
console.log('script start')
setTimeout(function () {
  console.log('settimeout')
}, 0)
async1()
new Promise(function (resolve) {
  console.log('promise1')
  resolve()
}).then(function () {
  console.log('promise2')
})
console.log('script end')

//结果：script start、async1 start、async2、promise1、script end (script宏任务中的同步代码执行完了)
//轮到微任务：async1 end、promise2
// 新的一轮事件循环（从宏任务队列中取搜个新的宏任务到执行栈中执行）： settimeout
```

**第三道：**

```js
const fn1 = () => {
  console.log('A')
  return 'B'
}
const fn2 = async () => {
  console.log('C')
  return Promise.resolve('D')
}
const result = async () => {
  console.log('start....')
  let r1 = await fn1()
  console.log(r1)
  let r2 = await fn2()
  console.log(r2)
}

result()
console.log('Z')

new Promise((resolve, reject) => {
  console.log('E')
  resolve('F')
})
  .then(res => {
  console.log(res)
  return Promise.resolve('G')
})
  .then(res => {
  console.log(res)
})
//答案：start....、A、Z、E、B、C、F、D、G
```

==注意！！==

解这道题目需要知道这几个知识点：

>- 知识点一：`await异步微任务` 有起到等待的效果，意思就是**该行`await命令`的微任务执行了，后面的代码语句才会执行，否则会跳出当前函数（也就是执行上下文环境）转而先去执行其它同步代码。**
>
> 示例：
>
> ![image-20210909230012277](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909230017.png)
>
>- 知识点二：`await命令`后面如果跟的是 `async函数并且返回的是Promise.resolve('XXX')`。例如下图所示：
>
> ![image-20210909223051102](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909223103.png)
>
> 那么该 `await命令异步微任务 `则要**等两轮时序的异步微任务才执行。**
>
> ![image-20210909223419881](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909223425.png)
>
> **验证：**
>
> ```js
> const fn1 = () => {
>   console.log('A')
>   return 'B'
> }
>
> const fn2 = async () => {
>   console.log('第一轮微任务', 'C')
>   return Promise.resolve('D')
> }
>
> const result = async () => {
>   console.log('start....')
>   let r1 = await fn1()
>   console.log('第一轮微任务', r1)
>   let r2 = await fn2()
>   console.log(r2)
> }
> result()
> console.log('Z')
> new Promise((resolve, reject) => {
>   console.log('E')
>   resolve('F')
> })
>   .then(res => {
>   console.log('第一轮微任务', res)
> })
>   .then(res => {
>   console.log('第二轮微任务G')
> })
>   .then(res => {
>   console.log('第三轮微任务V')
> })
>   .then(res => {
>   console.log('第四轮微任务P')
> })
>
> new Promise((resolve, reject) => {
>   console.log('1')
>   resolve('2')
> })
>   .then(res => {
>   console.log('第一轮微任务', res)
> })
>   .then(res => {
>   console.log('第二轮微任务3')
> })
>   .then(res => {
>   console.log('第三轮微任务4')
> })
>   .then(res => {
>   console.log('第四轮微任务5')
> })
> ```
>
> ![image-20210909224757989](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909224802.png)
>
>- 知识点三：`await命令`后面如果跟的是 `1、不是对象`、`2、普通Promise实例`，那么按正常顺序执行微任务。
>
> 1. 不是对象：
>
>    ![image-20210909231006506](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909231013.png)
>
> 2. 普通Promise实例：
>
>    ![image-20210909231239040](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909231248.png)
>
>    ![image-20210909231422679](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021923.png)
>
>- 知识点四：`await命令` **现在语义上等价于 `Promise.resolve(XXX)`**，而不是 `new Promise(resolve=>resolve(XXX))`。



**第四道：**

```js
function testSometing() {
  console.log('执行testSomething')
  return 'testSomething'
}

async function testAsync() {
  console.log('执行testAsync')
  return Promise.resolve('hello async')
}

async function test() {
  console.log('test start...')
  const v1 = await testSometing() 
  console.log(v1)
  const v2 = await testAsync() 
  console.log(v2)
}

test()

var promise = new Promise(resolve => {
  console.log('promise start..')
  resolve('promise')
}) //关键点2
promise.then(val => console.log(val))

console.log('test end...')

//结果：test start...、执行testSomething、promise start..、test end...(第一次宏任务中的同步代码) 
//：testSomething、执行testAsync、promise、hello async、(第一次微任务中的同步代码)
```

分析：

>1. 当上下文执行环境的指针指向 `test()` ，开始将 `test()`任务压入执行栈中，也就是将 `test()` 函数上下文环境压入执行栈中，然后指针进入该函数上下文环境，从上到下扫描，逐行解析代码。
>
>2. 解析`test()`函数上下文环境第一行代码，打印 `test start`。解析第二行代码，发现有 `testSomething()函数`，将其函数上下文环境压入执行栈中执行。
>
>3. 解析`testSomething()函数`第一行代码，打印 `执行testSomething`，当执行到第二行的时候返回testSomething；该函数上下文环境移出执行栈。
>
>4. 指针回到 `const v1 = await testSomething()`，因为await是异步微任务，此时开启异步线程执行该异步任务，结果返回一个异步回调微任务到微任务队列中等待执行。
>
>5. 因为 `await命令`能起到停歇作用，以至于该行下面的代码要等待其有结果才能往下执行。所以指针可以跳出 `test()函数上下文环境`转而去执行其它同步代码。
>
>6. 指针来到 `console.log('promise start')`，打印该同步代码结果，指针跳下一行，执行`resolve(’promise')`，改变Promise实例的状态为成功。发现有`then微任务`，开启异步线程执行该微任务，结果返回到微任务队中等待执行。此时微任务队列中已经有两个微任务再等待执行了。
>
>7. 执行最后一个同步代码：打印`test end...`。
>
>8. 第一个script脚本宏任务中的同步代码都执行完毕了，开始执行微任务。
>
>9. 指针回到`test()函数上下文环境`的const v1 = await testSomething()，执行该异步微任务。打印`testSomething`，指针下移，进入到`testAsync()函数上下文环境`，打印`执行testAsync`。返回一个`Promise.resolve('hello async')`给await微任务。
>
>   该微任务进入到微任务队列中等待执行。
>
>10. 执行第二个微任务，打印`promise`。
>
>11. 最后发现只剩下微任务队列中有任务等待执行。执行最后一个微任务，打印`hello async`。
>
>12. 最终所有任务执行完毕，事件循环结束。



## 九、八种数据类型及相互转换

### 9.1 八种数据类型

JavaScript一共有**8种数据类型**，其中7种是原始数据类型(即基本数据类型)，1种是引用值类型。

>-  原始数据类型：undefined、null、number、string、boolean、symbol(ES6)、bigint(ES10)
>- 引用值数据类型：object。



### 9.2 其它数据类型转换成数值类型

方法一：利用 `Number函数` ，将参数转换为数值类型的数据，如果转换不成功则返回NaN.

```js
console.log(Number('123')) //123
console.log(Number(true)) //1
console.log(Number(false)) //0
console.log(Number(undefined)) //NaN
console.log(Number(null)) //0
console.log(Number('')) //0
console.log(Number('hello world')) //NaN
console.log(Number({ id: 123 })) //NaN
```

**总结：**

>- 字符串如果是数字字符串，则会转换为数值类型的数字，否则NaN。
>- 空字符串会转换为数字0。
>- 布尔值会转换为1或0。
>- null会转换为0，undefined会转换为NaN。
>- 引用类型的数据也会转换为NaN。



方法二：利用 `+一元运算符` 转换为数值类型。

```js
console.log(+'123') //123
console.log(+true) //1
console.log(+false) //0
console.log(+undefined) //NaN
console.log(+null) //0
console.log(+'') //0
console.log(+'hello world') //NaN
console.log(+{ id: 12 }) //NaN
```

**总结：跟Number()函数功能一样。**



方法三：利用 `parseInt()` 和 `parseFloat()` 方法。

```js
##parseInt:
console.log(parseInt('123')) //123
console.log(parseInt(true)) //NaN
console.log(parseInt(false)) //NaN
console.log(parseInt(null)) //NaN
console.log(parseInt(undefined)) //NaN
console.log(parseInt('hello world')) //NaN
console.log(parseInt('')) //NaN
console.log(parseInt({ id: 1 })) //NaN

##parseFloat:
console.log(parseFloat('123.12')) //123.12
console.log(parseFloat(true)) //NaN
console.log(parseFloat(false)) //NaN
console.log(parseFloat(null)) //NaN
console.log(parseFloat(undefined)) //NaN
console.log(parseFloat('hello world')) //NaN
console.log(parseFloat('')) //NaN
console.log(parseFloat({ id: 1 })) //NaN
```

**总结：**

>- 字符串中出现了非数字都会转换为NaN。
>- parseFloat 比 parseInt会多识别一个小数点后面。



### 9.3 其它数据类型转换成字符串类型

方法一：利用 `String()函数` ，将其它数据类型转换为字符串类型。

```js
console.log(String(123)) //"123"
console.log(String(true)) //"true"
console.log(String(false)) //"false"
console.log(String(undefined)) //"undefined"
console.log(String(null)) //"null"
console.log(String([1, 2, 3, 4])) //"1,2,3,4"
```



方法二：利用 `toSring()方法` 将其它数据类型转换为字符串类型。

```js
console.log((123).toString()) //"123"
console.log([1, 2, 3, 4, 5, 6].toString()) //"1,2,3,4,5,6"
console.log(new Date().toString()) //Thu Oct 28 2021 19:32:28 GMT+0800 (中国标准时间)
console.log((123 + '1').toString()) // "1231"
console.log(true.toString()) //"true"
console.log(false.toString()) //"false"
```



**两方法总结及细微发现：**

>将数组转字符串可以使用 `String()方法` 和 `toString()方法`。



### 9.4 其它数据类型转换成boolean类型

隐式转换成Boolean类型：`null`、`undefined`、`空字符串`、`0`、`NaN`会转换成false。

显式转换成Boolean类型：

```javaScript
console.log(Boolean(null)) //false
console.log(Boolean(undefined)) //false
console.log(Boolean('')) //false
console.log(Boolean(0)) //false
console.log(Boolean(NaN)) //false
console.log(Boolean({})) //true
```



## 十、数据类型检测办法总结

### 10.1 typeof检测

`typeof`是直接在计算机底层基于二进制进行检测的。

一、对象在计算机存储中，都是以 `000`为开头的二进制进行存储，而 `null` 也是，所以 `typeof` 对 `null` 的检测结果就是一个object对象。

二、它对 `普通对象`、`数组对象`、`正则对象`、`日期对象` 的检测结果都是”object“。

三、`typeof` 检测返回的结果都是以字符串的形式。

```js
##对基本数据类型的检测
console.log(typeof 123) // =>"number"
console.log(typeof 'hello world') //=>"string"
console.log(typeof true) // =>"boolean"
console.log(typeof false) //=>"boolean"
console.log(typeof undefined) //=>"undefined"
console.log(typeof null) //=>"object"

##对引用类型的检测
console.log(typeof {}) //=>"object"
console.log(typeof new Array(10)) //=>"object"
console.log(typeof /^$/i) // =>"object"
console.log(typeof new Date()) // =>"object"
console.log(typeof function () {}) //  =>"function"
```

**总结：**

>- `typeof` 对**基本数据类型**的检测还是很不错的（除了 `null` ）。
>- `typeof` 对**引用数据类型**的检测不太友好，因为结果大部分是 `object`，无法进一步知道其到底是哪种类型的对象。 



### 10.2 instanceof检测

`instanceof` 是检测当前实例是否属于这个类。

#### 10.2.1 底层机制

**检测的底层机制：**

==**只要当前类出现在该实例的原型链上，那么检测结果返回true，否则返回false。**==

示例：

```js
const arr = []
console.log(arr instanceof Array) //=>true
console.log(arr instanceof RegExp) //=>false
console.log(arr instanceof Object) //=>true
```

从示例结果中你也会发现，`instanceof` 检测机制也没有那么完美，比如：

![image-20211029024147032](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211029024219.png)

arr既属于`Array类`，也属于`Object类`。也就是说，`instanceof`用来数据类型检测也不太行。

**当然了，`instanceof` 的出现并不是专门用来检测数据类型的，而是用来检测该实例是否属于这个类。**



#### 10.2.2 致命问题

 同时它有二个致命的问题，那就是：

- 一、不能检测基本数据类型的实例所属哪个类。
- 二、对象的原型可以肆意的进行更改，所以检测出来的结果是不准的。

示例：

```js
##原型指向进行了更改
function Fn() {
  this.a = 100
}
Fn.prototype = Object.create(Array.prototype) //改变原型指向。
const f = new Fn()

console.log(f, f instanceof Array) //=>true
//f对象实例本来就不属于Array这个类，单由于其显式原型的指向进行过更改，更改为指向数组的原型，以至于结果变成了false。


##不能检测基本数据类型的实例所属哪个类
console.log(1 instanceof Number) //=>false
console.log('hello world' instanceof String) //=>false
console.log(true instanceof Boolean) //=>false
//从结果中可以发现，基本数据类型的检测都返回false，因为不能对基本数据类型进行检测。
```



#### 10.2.3 手动封装一个instanceof方法

既然 `instanceof` 不能对基本数据类型进行检测，那么我们自己来封装一个方法，这样既可以检测引用数据类型，也可以检测基本数据类型。

封装方法：

```js
/**
*@param {String|Object} example 要检测的实例
*@param {Function} classFunc 类
*/
function _instanceof(example, classFunc) {
  let _proto_ = Object.getPrototypeOf(example) //相当于=>example.__proto__,只不过是为了兼容其它浏览器而已。
  let calssFuncPrototype = classFunc.prototype
  // 开始进行检测
  while (true) {
    if (_proto_ === null) {
      // 说明已经到了Object.prototype.__proto__=>null
      return false
    }
    if (_proto_ === calssFuncPrototype) {
      // 查找过程中发现有，则证明实例是这个类的实例。
      return true
    }
    // 如果某个隐式原型找不到，那么继续往上找。
    _proto_ = Object.getPrototypeOf(_proto_)
  }
}
```

检测结果：

```js
console.log(_instanceof(1, Number)) //true
console.log(_instanceof('hello world', String)) //true
console.log(_instanceof(1, String)) //false
```



### 10.3 constructor检测

`constructor` 检测的结果是=>**返回创建实例对象的构造函数的引用。**

```js
const n = 1
const obj = {}
const arr = []
console.dir(n.constructor) // => Number()
console.dir(obj.constructor) // => Object()
console.dir(arr.constructor) // => Array()
```

以至于 `constructor` 也是可以用来充当数据类型检测点的方法。

```js
console.log(n.constructor === Number) // =>true
console.log(arr.constructor === Array) // =>true
console.log(obj.constructor === Object) //=>true
console.log(obj.constructor === Array) //=>false
console.log(obj.constructor === RegExp) //=>false
```

从结果中可以发现，使用 `constructor` 数据类型检测比 `instanceof` 好用，因为它不仅可以检测引用数据类型，还可以检测基本数据类型。

**consturctor的缺点：**

但是它也有一个致命的缺点，那就是，它返回构造函数的引用能随便进行修改，以至于检测出来的结果不一定准确。

```js
const arr = []
console.log('未修改返回的引用:', arr.constructor === Array) //=>true
arr.constructor = Number
console.log('修改返回的引用后:', arr.constructor === Array) //false

//从结果中你可以发现，对某个实例对象的constructor进行修改后，用它进行数据类型检测结果就不一样了。
```



### 10.4 Object.prototype.toString.call()检测

`Object.prototype.toString.call([value])` **标准检测数据类型的办法**。

#### 10.4.1 作用

**作用：会返回当前实例所属类的信息。**

```js
##所有数据类型检测出来的结果：
console.log(Object.prototype.toString.call(1)) //=>[object Number]
console.log(Object.prototype.toString.call(11n)) //=>[object BigInt]
console.log(Object.prototype.toString.call(NaN)) //=>[object Number]
console.log(Object.prototype.toString.call('hello world')) //=>[object String]
console.log(Object.prototype.toString.call(true)) //=>[object Boolean]
console.log(Object.prototype.toString.call(null)) //=>[object Null]
console.log(Object.prototype.toString.call(undefined)) //=>[object Undefined]
console.log(Object.prototype.toString.call(function () {})) //=>[object Function]
console.log(Object.prototype.toString.call(new Array())) //=>[object Array]
console.log(Object.prototype.toString.call({})) //=>[object Object]
console.log(Object.prototype.toString.call(/^$/i)) //=>[object RegExp]
console.log(Object.prototype.toString.call(new Date())) //=>[object Date]
console.log(Object.prototype.toString.call(Symbol('1'))) //=>[object Symbol]
```

你会发现，`Object.prototype.toString.call([value])` 能将所有数据的类型都检测出来。

对所有数据类型检测的结果：

>[Object Number/BigInt/Number/String/Boolean/Null/Undefined/Function/Array/Object/RegExp/Date/Symbol]



#### 10.4.2 封装一个实用的数据类型检测方法

因为 `typeof `对引用的数据类型和null检测结果不太友好，那么我们可以结合`Object.prototype.toString.call([value])`方法，封装一个实用的数据类型检测方法。

**封装一个实用的数据类型检测方法：**

```js
/* 类型检测万能方法 */
const _isType = function (obj) {
  const class2type = {},
        toString = class2type.toString
  const reg = /^\[object (.+)\]$/
  // 如果obj是n或undefined，则返回null或undefined
  if (obj === null || undefined) {
    return obj
  }
  // 如果是基本数据类型，则使用typeof来检测,否则使用Object.prototype.toString.call()
  return typeof obj === 'object' || typeof obj === 'function' ? reg.exec(toString.call(obj))[1] : typeof obj
}
```



## 十一、循环遍历方法性能的比较





## 十二、原型与原型链

### 12.1 认识原型prototype

创建的每一个函数，**解析器都会向函数中添加一个属性`prototype`属性。**

```js
function Fn() {
  this.name = 'yyy',
  this.age = 18,
  this.id = 123
  //相当于
  //this.prototype=Fn.prototype
}
console.dir(Fn) 
```

结果：

![image-20211101015827385](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211101015829.png)

`prototype属性` 对应着一个对象，这个对象我们称为原型对象。   **理解：（prototype属性的值是一个原型对象的地址）**

注意：

>- 如果函数作为普通函数执行，那么 `prototype属性` 没有任何作用。
>- 如果函数作为构造函数执行，那么 `prototype属性`指向的就是**该构造函数的显式原型对象。**

代码示例：

```js
function Fn() {
  this.name = 'yyy',
    this.age = 18, 
    this.id = 123
}
const fn = new Fn()
console.log(Fn.prototype.constructor === Fn) //=>true
```



### 12.2 显式原型和隐式原型

**一、显式原型：**

每一个构造函数都有一个 `prototype属性`，这个属性可以理解为显式原型属性。**它的指向是该构造函数的原型对象。**

```js
##用constructor判断prototype属性指向的地方
function Fn() {
}
const fn = new Fn()
console.log(Fn.prototype.constructor === Fn)  //=>true
```

**二、隐式原型：**

每一个实例对象上都有一个 `__proto__属性` ，这个属性称为隐式原型属性。**它的指向也是该构造函数的原型对象。**

```js
##隐式原型检测
function Fn() {

}
const fn = new Fn()

console.log(fn.__proto__ == Fn.prototype)  //=>true
```

**三、显式原型和隐式原型的关系：**

==对象的隐式原型等于构造函数的显式原型。==

```js
console.log(fn.__proto__ == Fn.prototype)  //=>true
```

**四、图解说明它们之间的关系：**

![image-20211101014242489](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211101014302.png)



### 12.3 原型链

#### 12.3.1 原型链的理解

`原型链` 就相当于我们学数据结构时的链表结构一样，一个属性指向的是一个对象的地址。

下面这个图就相当于一个原型链：

![image-20211101014242489](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211101014302.png)



#### 12.3.2 原型链的用处

认识原型链用处之前我们先看一个问题：当我们访问一个对象的属性时，它的内部过程是在干嘛？

问题解答：

>1. 先在该对象的自身开始寻找属性，找到就返回。
>2. 如果找不到，那么它会沿着 `隐式原型__proto__` 这条链向上进行查找，找道就返回。
>3. 如果还是找不到，继续沿着 `隐式原型__proto__` 这条链向上进行查找，找到就返回。
>4. 如果最终还是没找到，就是返回 `undefined`。

**也正是原型链的存在，我们才可以利用原型进行功能的扩展，同时也可以节约内存空间。**



#### 12.3.3 所有函数都是Function.prototype的实例

所有函数都是 `Function.prototype` 的实例，即都是 `Function构造函数的原型对象`的实例（包括`Function`它自己）。 

```js
##检测Foo普通构造函数
function Function() {}
var Foo = new Function()
console.log(Foo.__proto__ == Function.prototype) //=>true

##检测Object构造函数
function Function() {}
var Object = new Function()
console.log(Object.__proto__ === Function.prototype) //=>true
console.log(Object.__proto__.__proto__ === Function.prototype.__proto__) //=>true

##检测Function构造函数
console.log(Function.__proto__ === Function.prototype) //=>true
```

参考图：

![原型链](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211101025956.png)

#### 12.3.4 Object的原型对象是原型链的尽头

```js
console.log(Object.prototype.__proto__)  //=>null
```



### 12.4 原型链整体示意图

原型链整体示意图：

![原型链](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211101025956.png)



## 十三、继承

JavaScript继承模式有很多种，这里我们先概括常用的继承模式。

### 13.1 原型链继承

一、通过强大的原型链来实现继承，那么该继承模式就是原型链继承。

```js
##原型链继承示例
function Bar() {
  this.name = 'Eson'
}
function Foo() {
  this.name = 'Rose'
}
//继承Bar
Foo.prototype = new Bar() //改变Foo构造函数的原型对象

const foo = new Foo() 

console.log(foo)

//继承后的foo
//Foo {name: "Rose"}
//	name: "Rose"
//	[[Prototype]]: Bar
//		name: "Eson"
//		[[Prototype]]: Object
//			constructor: ƒ Bar()
//			[[Prototype]]: Object
```

二、**原型链继承的问题：引用值会被共享。**

```js
##原型链继承的问题：引用值会被共享
function SuperType() {
  this.colors = ['red', 'blue', 'green']
}
function SubType() {}

// 继承 SuperType
SubType.prototype = new SuperType()
let instance1 = new SubType()

instance1.colors.push('black')

console.log(instance1.colors) //=> "red,blue,green,black"

let instance2 = new SubType()

console.log(instance2.colors) //=> "red,blue,green,black"
```



### 13.2 借用构造函数继承

**一、套路做法：**

>1. 定义父类型构造函数
>2. 定义子类型构造函数
>
>3. 在子类型构造函数中调用父类型构造函数

**二、关键**

**在子类型构造函数中通过利用call()（或apply()）方法调用父类型构造函数**

示例：

```js
// Foo父类
function Foo() {
  this.colors = ['red', 'yellow', 'green']
}
Foo.prototype.show = function () {
  console.log(this.colors)
}

// Bar子类
function Bar() {
  // 借用构造函数继承父类属性
  Foo.call(this)
}

// 父类的实例对象
const foo = new Foo()
console.log(foo) //=>colors: (3) ["red", "yellow", "green"]

//子类的实例对象
const bar1 = new Bar()
const bar2 = new Bar()
bar2.colors.push('pink')
console.log(bar1) //=>colors: (3) ["red", "yellow", "green"]
console.log(bar2) //=>colors: (4) ["red", "yellow", "green", "pink"]
```

从示例结果中可以发现，==**使用借用构造函数继承解决了引用值会被共享的问题**==。

**三、借用构造函数继承的优点和缺点：**

- 优点：可以在子类构造函数向父类构造函数传递参数。
- 缺点：父类原型上定义的方法不能被共享。

```js
###优点:
function SuperType(name) {
  this.name = name
}
function SubType(name) {
  // 继承 SuperType 并传参
  SuperType.call(this, name)
  // 实例属性
  this.age = 29
}
const instance = new SubType('Ethon')

console.log(instance) //=>SubType {name: "Ethon", age: 29}

###缺点
function SuperType(name) {
  this.name = name
}
SuperType.prototype.show = function () {
  console.log(this.name)
}
function SubType(name) {
  // 继承 SuperType 并传参
  SuperType.call(this, name)
  // 实例属性
  this.age = 29
}

//父类的实例对象，该实例对象上有show()方法
const superType = new SuperType('Rose')
// 父类的实例对象的原型上有show方法
superType.show() //=>Rose

//子类的实例对象，因为是使用借用构造函数继承，以至于父类的原型对象上的方法不能共享。
const instance = new SubType('Ethon')

// 子类的实例对象上的原型上没有继承的父类原型上的show方法
console.log(instance.show) //=>undefined 说明instance实例对象没有show方法
```



### 13.3 组合继承(常用)

组合继承也称为伪经典继承。

组合继承是结合了==原型链继承和借用构造函数继承这两种继承的优点实现的。==

**基本思路：**

- 因为 `实例属性` 是每个对象自身，所以通过**借用构造函数继承**。
- `原型上的属性和方法` 是所以实例对象公有的，所以通过**原型链继承**。

```js
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'blue', 'green']
}
SuperType.prototype.sayName = function () {
  console.log(this.name)
}

// 子类
function SubType(name, age) {
  // 继承属性
  SuperType.call(this, name)
  this.age = age
}

// 继承方法
SubType.prototype = new SuperType()
SubType.prototype.sayAge = function () {
  console.log(this.age)
}
// 实例对象instance1
let instance1 = new SubType('Nicholas', 29)
instance1.colors.push('black')
console.log(instance1.colors) // "red,blue,green,black"
instance1.sayName() // "Nicholas";
instance1.sayAge() // 29

// 实例对象insatnce2
let instance2 = new SubType('Greg', 27)
console.log(instance2.colors) // "red,blue,green"
instance2.sayName() // "Greg";
instance2.sayAge() // 27
```

**组合继承问题：**父类构造函数始终会被调用两次，一次在是创建子类原型时调用，另一次是在子类构造函数中调用。

```js
function SuperType(name) { 
  this.name = name; 
  this.colors = ["red", "blue", "green"]; 
} 
SuperType.prototype.sayName = function() { 
  console.log(this.name); 
}; 
function SubType(name, age){ 
  SuperType.call(this, name); // 第二次调用 SuperType() 
  this.age = age; 
} 
SubType.prototype = new SuperType(); // 第一次调用 SuperType() 
SubType.prototype.constructor = SubType; 
SubType.prototype.sayAge = function() { 
  console.log(this.age); 
};
```

**总结说明：**

>- 组合继承弥补了原型链继承和借用构造函数继承的不总，也是JavaScript中使用最多的继承模式。
>- 组合继承还保留了instanceof操作符和isPrototypeOf()方法。



### 13.4 复制继承

复制继承的基本思路是：将`父对象的原型属性`和`方法`逐一复制给子对象。

**复制继承的基本思路：**

```js
for(let item in 父对象){
  if(父对象.hasOwnProperty(item)){
    this[item]=父对象[item];
  }
}
//此功能重复，用函数封装起来复用
```



### 13.5 原型式继承

原型式继承适合这种情况：**你有一个对象，想在它的基础上再创建一个新的对象。**

**结果：**

>原对象成为了新对象的原型对象，新对象就成了实例对象。

ES5通过使用 `Object.create()方法`实现原型式继承。

该方法接收两个参数：

1. 第一个参数是 `Object类型`，即该参数是作为新的对象的原型对象。
2. 第二个参数是可选参数，即该参数是可以作为新的对象的自身属性。

```javascript
// Object.create()方法使用示例
const person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van'],
}
const anotherPerson = Object.create(person)
console.log(anotherPerson)  

//结果：
// {}
//		[[Prototype]]: Object
//			friends: (3) ["Shelby", "Court", "Van"]
//			name: "Nicholas"
//			[[Prototype]]: Object


// 原对象上的属性会是所有新对象共享的属性
const person = {
  name: 'Nicholas',
  friends: ['Shelby', 'Court', 'Van'],
}
const anotherPerson = Object.create(person)

anotherPerson.friends.push('Rob')

const yetAnotherPerson = Object.create(person)

yetAnotherPerson.friends.push('Barbie')

console.log(person.friends) // => "Shelby,Court,Van,Rob,Barbie"


/ Object.create()方法的第二个参数示例
let person = {
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



### 13.6 寄生组合继承

**寄生组合继承也称为经典继承。**

这种继承模式解决了组合继承中父类构造函数被调用两次问题。

```js
//父类
function Super(name) {
  ;(this.a = [1, 2, 3, 4]), (this.title = name)
}
Super.prototype.say = function () {
  console.log(222)
}

// 子类
function Sub(name) {
  Super.call(this, name)
}
// Sub.prototype = new Super('aaa')
// 上面Sub.prototype=new Super('aaa') 替换使用原型式继承，避免父类构造函数被调用两次
Sub.prototype = Object.create(Super.prototype)

// 兼容浏览器写法，因为有些浏览器没有Object.create()这个方法
if (!Object.create) {
  Object.create = function (proto) {
    function F() {}
    F.prototype = proto
    return new F()
  }
}

const sub1 = new Sub('bbb')
sub1.say() //=>222
```



## 十四、原型与继承的关系

检测原型与实例的关系可以通过两种办法：

- 一、使用 `instanceof操作符` 检测。
- 二、使用 `isPrototypeOf()方法` 检测。

### 14.1 使用instanceof检测

`instanceof操作符` 检测机制是：**只要当前类出现在该实例的原型链上，那么instanceof返回true，否则返回false。**

```js
const arr = []
console.log(arr instanceof Array) //=>true
console.log(arr instanceof RegExp) //=>false
console.log(arr instanceof Object) //=>true
```



### 14.2 使用isPrototypeOf()方法检测

`isPrototypeOf()方法` 检测机制是：**只要当前这个原型对象出现在该实例的原型链上，那么isPrototypeOf()方法返回true，否则返回false。**

```js
##使用isPrototypeOf()方法检测
function Bar() {
  this.name = 'Eson'
}
function Foo() {
  this.name = 'Rose'
}
Foo.prototype = new Bar() //改变Foo构造函数的原型对象

const foo = new Foo()

console.log(Bar.prototype.isPrototypeOf(foo)) //=>true
console.log(Foo.prototype.isPrototypeOf(foo)) //=>true
console.log(Object.prototype.isPrototypeOf(foo)) //=>true
```



## 十五、预编译总结

### 15.1 全局预编译

全局预编译执行过程总结：

- 第一步：在浏览器环境中，将window确定为全局执行上下文。
- 第二步：寻找 `var声明的变量`，然后将其提升到全局执行上下文的顶部。
- 第三步：寻找 `function声明的函数变量`，然后将其提升到全局执行上下文的顶部。如果此时function声明的函数变量与var声明的变量相同，则会将var声明的变量覆盖。
- 第四步：开始逐行执行代码。

```js
function Foo() {}
console.log(Foo) //=>Foo() {}
var Foo = 2
```



### 15.2 函数局部预编译

函数执行上下文预编译执行过程总结：

- 第一步：先寻找 `var声明的变量`，然后将其提升到函数执行上下文顶部。
- 第二步：函数的形参和实参赋值。
- 第三步：寻找 `function声明的函数变量`，然后也将其提升到函数执行上下文顶部。如果此时function声明的函数变量与var声明的变量相同，则会将var声明的变量覆盖。
- 第四步：开始逐行执行代码。

```js
function a(a) {
  console.log(a) //=>a() {}
  var a = 4
  function a() {}
}
a(2)
```



## 十六、防抖节流

### 16.1 防抖

防抖就是通过 `setTimeout` 的方式，在一定的时间间隔内，将多次触发变成一次触发。

#### 16.1.1 原生实现

```html
<input type="text" />
<input type="submit" id="input" />
```

```js
### 防抖示例
const btn = document.querySelector('#input')
btn.addEventListener(
  'click',
  debounce(function (e) {
    console.log(22, e)
    console.log(this)
  }, 1000),
  false
) //第三个参数为false标识禁止冒泡

//debounce防抖函数
function debounce(fn, delay) {
  let timer = null //一开始定时器任务为空
  return function (...arg) {
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      fn.apply(this, arg)
    }, delay)
  }
}
```

上面的写法存在一个问题，问题就是：当点击发送网络请求的时候，如果该网络请求很慢，此时你还延时点击，这样只会造成更慢。

那么有没有办法解决这类问题呢？就是每次在该延时范围内第一次点击都不受影响，后面在延时范围内的多次点击就会受到影响。

答案是：有的！如下例子写法：

```js
### 防抖函数第一次点击不受影响
function debounce(fn, delay) {
  let timer = null //一开始定时器任务为空
  return function (...arg) {
    let firstClick = !timer // 就是定时器任务为空的时候
    if (firstClick) fn.apply(this, arg)
    if (timer) clearTimeout(timer)
    timer = setTimeout(() => {
      timer = null
    }, delay)
  }
}
```



#### 16.1.2 防抖函数完整版

完整版防抖函数具有的功能:

1. 将一段时间内多次频繁执行的操作变为一次。
2. 具备立即执行的效果(即第三个参数为true的时候，防抖函数能立即执行，而无需再等定时器再执行)。
3. 在定时器指定时间执行之前可取消该操作。
4. 能够返回防抖的内部函数的结果。

```js
// 防抖函数完整版
const debounce = function (fn, delay, immediate = false) {
  // console.log(typeof fn)
  if (typeof fn !== 'function') throw new TypeError('fn is not a function!')
  if (typeof delay !== 'number') delay = 300 // 如果delay的类型不是数字，则给它默认300毫米时间间隔
  if (typeof delay === 'boolean') immediate = delay // 如果第二个参数是布尔类型，则说明该防抖函数需要立即执行
  if (typeof immediate !== 'boolean') throw new TypeError('the third param must be of Boolean type')
  let timer = null
  let result
  const operate = function (...args) {
    if (timer) clearTimeout(timer)
    if (immediate) {
      const firstClick = !timer // 当没有定时器的时候为true,那么立即执行
      firstClick ? result = fn.apply(this, args) : null
      timer = setTimeout(() => {
        timer = null
      }, delay)
    } else {
      timer = setTimeout(() => {
        result = fn.apply(this, args)
      }, delay)
    }
    return result
  }
  // 取消操作
  operate.task = function () {
    clearTimeout(timer)
    timer = null
  }
  return operate
}
```



#### 16.1.3 应用场景

防抖的应用场景有：

- `scroll` 事件滚动触发。
- 搜索框进行搜索的时候。
- 点击按钮进行提交的时候。
- 表单验证。
- 浏览器窗口缩放，即 `resize` 事件。



### 16.2 节流

节流就是减少一段时间内的触发频率。

#### 16.2.1 原生实现

```html
<input type="text" />
<input type="submit" id="input" />
```

```js
### 节流
btn.addEventListener(
  'click',
  throttle(function (e) {
    console.log(22, e)
    console.log(this)
  }, 1000),
  false
)

function throttle(fn, delay) {
  let begin = 0
  return function (...arg) {
    let cur = new Date().getTime()
    if (cur - begin > delay) {
      fn.apply(this, arg)
      begin = cur
    }
  }
}
```



#### 16.2.2 应用场景

节流的应用场景：

- DOM元素拖拽。
- 射击游戏。
- 计算鼠标移动的距离。
- 监听 `scroll` 滚动事件。



## 十七、相等运算符的比较规则总结

我们先回顾一下 `相等运算符==` 是怎么进行比较的？哦，是**==值相等，类型不相等==**就是 `true`，否则就是 `false`。

**总结：**相等运算符的比较规则无非就是下面这几种：

- 一：对象跟字符串相比较的时候（即`对象` == `字符串`）,对象调用 `toString()` 方法变成字符串，然后再进行比较。
- 二：`null` 和 `undefined` 进行比较（即 `null` == `undefined`），它们是相等的。
- 三：`NaN` 和 `NaN` 进行比较，它们是不相等的。
- 四：剩下的，都是转为数字进行比较。

示例：

```js
/*1.对象跟字符串进行比较*/
console.log({} == '[object Object]') // => true

/*2.null和undefined进行比较*/
console.log(null == undefined) // => true

/*3.NaN和NaN进行比较*/
console.log(NaN == NaN) // => true

/*4.其它的都是转化为数字再进行比较*/
console.log([10] == 10) // => true
console.log(true == 1) // => true
```



**一道经典面试题目：**

```markdown
var a =?
if(a==1 && a==2 && a==3) {
	console.log('成立')
}
```

解决的办法有两种：其中一种就是重写 `toString()` 方法来处理这道面试题。

```js
/*重写toString*/
var a = {
  i: 0,
  toString: function () {
    return ++this.i
  },
}
if (a == 1 && a == 2 && a == 3) {
  console.log('成立')
}
```



## 十八、深拷贝与浅拷贝

### 18.1 浅拷贝

浅拷贝就是对基本数据类型的拷贝是没问题的，但对引用类型的拷贝则只是拷贝它们的引用。

示例：

```js
/* 浅拷贝 */
const obj = {
  0: 'math',
  1: 'chinese',
  2: 'english',
  score: {
    math: 99,
    chinese: 100,
    english: 19,
  }, // 普通对象
  reg: /\d+/gim, // 正则对象
  time: new Date(),
  friends: ['tom', 'Rose'],
  say: function () {
    console.log('good good study')
  },
  tag: Symbol('TAG'),
  [Symbol('title')]: 'object',
}

const obj2 = {}
for (const key in obj) {
  if (Object.hasOwnProperty.call(obj, key)) {
    obj2[key] = obj[key]
  }
}

// 改变其中一个引用类型中的值
obj.friends[0] = 'threey'
console.log(obj2.friends[0]) // => threey
```

当我们改引用类型中的一个值时，浅拷贝出来的新对象的引用类型值也会发生改变。**所以浅拷贝对引用数据类型仅仅只是拷贝了引用地址。**

#### 18.1.1 面对对数组进行浅拷贝的方法总结

**方法1：利用ES6的扩展运算符。**

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
const arr2 = [...arr]

console.log(arr2) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr2 === arr1) // => false
```

**方法2：利用数组的concat()方法。**

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0] 
const arr3 = arr1.concat([])

console.log(arr3) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr3 === arr1) // => fasle
```

**方法3：利用数组的slice()方法。**

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0] 
const arr4 = arr1.slice()

console.log(arr4) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr4 === arr1) // => false
```

**方法4：利用数组的form()方法。**

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0] 
const arr6 = Array.from(arr1)

console.log(arr6) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr6 === arr1) // => false
```

**方法5：自己写一个循环遍历数组中的每一个基本类型属性。**

```js
const arr = [1,2,3,4,5,6,7,8,9,0]
const arr5 = []
for (let [key, value] of arr1.entries()) {
  arr5[key] = value
}
console.log(arr5) // => [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
console.log(arr5 === arr1) // false
```



#### 18.1.2 面对对象进行浅拷贝的方法总结

**方法1：使用ES6对象的扩展运算符。**

```js
const obj = {
  0: 'math',
  1: 'chinese',
  2: 'english',
  score: {
    math: 99,
    chinese: 100,
    english: 19,
  }, // 普通对象
  reg: /\d+/gim, // 正则对象
  time: new Date(),
  friends: ['tom', 'Rose'],
  say: function () {
    console.log('good good study')
  },
  tag: Symbol('TAG'),
  [Symbol('title')]: 'object',
}

const obj1 = {
  ...obj,
}

console.log(obj)
console.log(obj1 === obj) // => false
```

**方法2：使用Object.assign方法。**

```js
const obj = {
  0: 'math',
  1: 'chinese',
  2: 'english',
  score: {
    math: 99,
    chinese: 100,
    english: 19,
  }, // 普通对象
  reg: /\d+/gim, // 正则对象
  time: new Date(),
  friends: ['tom', 'Rose'],
  say: function () {
    console.log('good good study')
  },
  tag: Symbol('TAG'),
  [Symbol('title')]: 'object',
}

const obj1 = {
  ...obj,
}

console.log(obj)
console.log(obj1 === obj) // => false

// 2.2 使用Object.assign方法
const obj2 = Object.assign({}, obj1)
console.log(obj2 === obj) // => false
```

**方法3：自己写一个循环浅拷贝对象属性值**

```js
const obj3 = {}
for (const key in obj) {
  if (Object.hasOwnProperty.call(obj, key)) {
    const element = obj[key]
    obj3[key] = element
  }
}
console.log(obj3)
console.log(obj === obj3) // => false
```

**==注意：for...in循环只会遍历自身对象和继承的可枚举属性，不会含有Symbol类型的属性。==**

![image-20220413000338159](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20220413000338159.png)



#### 18.1.3 手写兼容各种情况的浅拷贝方法





### 18.2 深拷贝

