# ECMAScript6前部分知识

学习地址：https://es6.ruanyifeng.com/#docs/intro

## 一、let命令和const命令

**ES6之所以引入`let和const关键字`，是为了让大家养成良好的编程习惯，变量一定要在声明之后使用，否则就报错。**

### 1.1 let命令

let命令的基本语法：`let xx`

它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的**代码块内**有效。

示例：

```javascript
{
    let i = 1;
    var a = 2;
}
console.log(a); //2
console.log(i); //i is not defined
```



`for`循环的计数器，就很合适使用`let`命令。

```javascript
for (let i = 0; i < 10; i++) {
    // ...
}

console.log(i);
// ReferenceError: i is not defined
```

上面代码中，计数器`i`只在`for`循环体内有效，在循环体外引用就会报错。



>for循环中let和var的区别

==使用var：==

```javascript
/* 使用var关键字声明变量 */
var a = [];
for (var i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    }
}
console.log(a[7]());  //10
```

上面代码中，变量`i`是`var`命令声明的，在全局范围内都有效，所以全局只有一个变量`i`。每一次循环，变量`i`的值都会发生改变，而循环内被赋给数组`a`的函数内部的`console.log(i)`，里面的`i`指向的就是全局的`i`。也就是说，所有数组`a`的成员里面的`i`，指向的都是同一个`i`，导致运行时输出的是最后一轮的`i`的值，也就是 10。



==使用let：==

**声明的变量仅在块级作用域内有效**

```javascript
/* for循环中使用let */
var a = [];
for (let i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    }
}
console.log(a[7]());  //7
```

上面代码中，变量`i`是`let`声明的，当前的`i`只在本轮循环有效，所以每一次循环的`i`其实都是一个新的变量，所以最后输出的是`6`。你可能会问，如果每一轮循环的变量`i`都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量`i`时，就在上一轮循环的基础上进行计算。



`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域

![image-20210422140720883](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422140720883.png)



>let声明不支持变量提升

示例：

![image-20210422144706359](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422144706359.png)

>let不允许重复声明

**情景1,var和let声明的变量同名：报错**

![image-20210422150046037](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422150046037.png)

**情景2，let重复声明同一个变量 :报错**

![image-20210422150111264](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422150111264.png)



>let和const存在暂时性死区

只要块级作用域内存在`let`命令和`const`命令，它所声明的变量就“绑定”（binding）这个区域，**不会受外部变量的影响，也不会影响外部变量。**



ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。**凡是在声明之前就使用这些变量，就会报错。**

示例：

![image-20210422154841150](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422154841150.png)

==总结==：在代码块内，使用`let`命令和`const`命令声明变量之前，该变量都是不可用的。



### 1.2 块级作用域

#### 1.2.1 为什么要使用块级作用域

ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。

场景1：内层变量可能会覆盖外层变量

![image-20210422161047211](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422161047211.png)

场景2：用来计数的循环变量泄露为全局变量

![image-20210422161202454](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422161202454.png)

#### 1.2.2 ES6的块级作用域

**一、`let`实际上为 JavaScript 新增了块级作用域。**

```js
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```

上面的函数有两个代码块，都声明了变量`n`，运行后输出 5。这表示外层代码块不受内层代码块的影响。

如果两次都使用`var`定义变量`n`，最后输出的值才是 10,如下示例所示：

```js
function f1() {
  var n = 5;
  if (true) {
    var n = 10;
  }
  console.log(n); // 10
}
```

**二、内层作用域可以定义外层作用域的同名变量。**

```js
{
  let insane = 'Hello World';
  {let insane = 'Hello World'}
};
```

**三、块级作用域也叫私有作用域**

在`let`没给javascript添加块级作用域之前，我们使用匿名立即执行函数是可以创建一个私有作用域的。

因为私有作用域能保护内部变量不被外部访问，作用效果跟块级作用域一样。**所以，匿名立即执行函数也是可以产生块级作用域的。**

==但如今let提供的块级作用域的出现，使得匿名立即执行函数表达式（匿名 IIFE）不再成为必要了。==

```js
// IIFE 写法
(function () {
  var tmp = ...;
  ...
})();

// 可以用块级作用域写法替代上面匿名立即执行函数的写法
{
  let tmp = ...;
  ...
}
```



#### 1.2.3 块级作用域与函数声明

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。

ES6 规定，**块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用**。

考虑到环境导致的行为差异太大，应该==避免在块级作用域内声明函数==。如果确实需要，也应该写成函数表达式，而不是函数声明语句。

![image-20210422170554452](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422170554452.png)



### 1.3 const命令

**const声明的变量代表是常量**

基本用法：**`const`声明变量是必须初始化**，并且在代码的整个运行过程中不能修改初始值，否则会报错。

示例：

![image-20210422171240479](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422171240479.png)



`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。

![image-20210422171501707](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422171501707.png)

`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

`const`声明的常量，也与`let`一样不可重复声明。

![image-20210422171615700](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422171615700.png)

>本质

`const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动

​		对于基本数据类型，等同于常量，因为基本数据类型变量存放在栈内存中，其中存放的是值。

​		但对于引用数据类型变量，栈内存中存放的是地址，堆内置中存放的是引用类型的值。而`const`只能保证栈内存中的地址是固定的，至于它指向的数据结构是不是可变的，就完全不能控制了。因此，**将一个对象声明为常量必须非常小心。**

**实际用途**：可以给`数组`或者`对象`用const关键字声明，保证它们的地址是不变的。

示例：

![image-20210422172728916](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422172728916.png)

另一个例子：

![image-20210422172831969](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422172831969.png)

上面代码中，常量`a`是一个数组，这个数组本身是可写的，但是如果将另一个数组赋值给`a`，就会报错。



### 1.4 ES6 声明变量的六种方法

ES5 只有两种声明变量的方法：`var`命令和`function`命令

ES6增加了`let`、`const`、`import`、`class`。



### 1.5 顶层对象的属性

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。

**ES5 之中，顶层对象的属性与全局变量是等价的。**

示例：

![image-20210422173829418](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422173829418.png)

![image-20210422173900448](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422173900448.png)

这一例子无不说明：**ES5 之中，顶层对象的属性与全局变量是等价的。**



- ES6 为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；

- 另一方面规定，**`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性**。
- 也就是说，**从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。**

![image-20210422174402265](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210422174402265.png)



### 1.6 关于let和const在实际开发中的使用说明

1. 在实际开发和生产中，如果是**小程序，uniapp或者一些脚手架中的**，可以大胆的去使用let和const
2. 但是如果你是在web开发，建议大家还是使用var，因为在一些低版本的浏览器还是不支持let和const



## 二、变量的解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

变量的解构赋值本质上就是=="模式匹配"==，只要等号两边的模式相同，左边的变量就会被赋予对应的值。

```javascript
let [a,b,c] = [1,2,3];
console.log(a,b,c)；
//结果：1 2 3
```

### 2.1 变量的解构赋值规则

规则：等号右边的值==不是数值或对象==，就会将其转为对象（除了undefined,null），然后再进行解构赋值。

### 2.2 数组的解构赋值

#### #数组解构赋值的示例：

示例1：

```javascript
let [, twich, third] = ["foo", "bar", "baz"];
console.log(third);  //baz
console.log(twich);  //bar
```

示例2：数组解构赋值中有==剩余运算符“...”==

```javascript
let [head, ...end] = [1, 2, 3, 4, 5, 6];
console.log(head);  //1
console.log(end);   //[2, 3, 4, 5, 6]
```

示例3：等号右边值少于左边匹配模式数量c

```javascript
let [x, y, w, ...z] = ['a'];
console.log(x)   //a
console.log(y)   //unefined
console.log(w);  //undefined
console.log(z)   //[]
```

示例4：如果解构不成功，变量的值就等于undefined

```javascript
let [yyy] = [];
let [zzz, zxy] = [1];
console.log(yyy)       //undefined
console.log(zzz, zxy)  //1 undefined
```

示例5：不完全解构，即等号左边的模式，只匹配了一部分等号右边的数组。这种情况下，解构依然可以成功

```javascript
let [num1, num2] = [1, 2, 3];
console.log(num1); //1
console.log(num2); //2

let [num3, [num4], num6] = [1, [2, 3], 4];
console.table([num3, num4, num6]);   //1 2 4
```

示例6：如果等号的右边不具备iterator接口(即不可遍历的数据结构)，就会报错。

```javascript
let [foo] = 1;      //Number不具备iterator接口
let [foo] = false;  //Boolean不具备Iterator接口
let [foo] = NaN;    
let [foo] = undefined;
let [foo] = null;
let [foo] = {};     //纯Object不具备iterator接口
//上面都会报错
```

示例7：对于 Set 结构，也可以使用数组的解构赋值

```javascript
let [num1, num2, num3] = new Set(['a', 'b', 'c']);
console.log(num1, num2, num3);  
//a b c
```

#### #数组解构赋值的总结

==只要某种数据结构具有 Iterator 接口，都可以采用**数组形式的解构赋值**。==

#### #数组解构赋值的默认值





### 2.3 对象的解构赋值

#### #与数组解构赋值的区别

- 对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定。
- 对象的属性没有次序，变量必须与属性同名，才能取到正确的值

#### #对象的解构赋值内部机制

内部机制：先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。

冒号前面是`匹配模式`，后面是`被赋值的变量`。

```javascript
let { foo: bar } = { foo: 'aaa', bar: 'bbb' };
console.log(bar); 
//结果：“aaa”
//说明对象的解构赋值是 :后者
```

上面代码中，foo是`匹配的模式`，bar才是变量。真正被赋值的是变量bar，而不是模式foo。

#### #对象的解构赋值示例：

示例1：

```javascript
let { name, age } = { name: 'yyy', age: 22 }
console.log(name, age);  
//结果:yyy 22
```

示例2：如果变量没有对应的属性名，会导致取不到值，最后等于undefined

```javascript
let { a, b, c } = { a: 1, b: 2, d: 3 }
console.log(c); //undefined
//变量c匹配不到值，只能等于undefined。
```

示例3：将现有对象的方法解构赋值，赋值到某个变量

```javascript
const { log } = console;
log('hello');      //hello
console.dir(log);  //显示的是打印函数log()
```

​		上面代码解释：console是一个对象，里面有许多的方法，根据匹配模式，console对象里面有log方法，所以解构赋值成功。以至于log变量就是一个log打印方法。

------

示例4：解构嵌套赋值

```javascript
let obj = {};
let arr = [];
({ foo: obj.number, foo: obj.name, boo: arr[arr.length], boo: arr[arr.length] } = { foo: 123, boo: false })
console.log(obj); //{ number: 123, name: 123 }
console.log(arr); //[false, false]
```

示例5：如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。 

```javascript
let { yyy: { yyx } } = { yyx: "nihao" };
console.log(yyx);  //报错
//因为yyy这时等于undefined，再取子属性就会报错。
```

### 2.4 数值和对象解构赋值默认值

#### #数值解构赋值的默认值

> 解构赋值允许指定默认值

示例1：

```javascript
let [a = true] = [false];
console.log(a);  //false
```

示例2：

```javascript
let [b, c = 'yyy'] = ['zyx'];
console.log(b, c);  //zyx yyy
```

示例3：

ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。（**即变量声明没有赋值就是undefined**）

```javascript
let [d, e = 'hello'] = ['hahaha', undefined];
console.table(d, e);  
//结果：hahaha hello

let [f, g, h, hh] = [undefined, null, NaN, '1'];
console.log(f, g, h, hh); 
//结果： undefined null NaN "1"
```

示例4：

默认值可以引用解构赋值的其他变量，但该变量必须已经声明。

```javascript
let [x = 1, y = x] = [];
console.log(x, y);   //1 1
let [x = 1, y = x] = [2];
console.log(x, y);   // 2 2
let [x = 1, y = x] = [3, 2]
console.log(x, y);   //3 2
let [x = y, y = 1] = [];
console.log(x, y);   //报错
```



#### #对象解构赋值的默认值

>对象解构赋值可以指定默认值

示例1

```javascript
var { x = 3 } = {};
console.log(x);  //3
```

示例2：

```javascript
var { x, y = 3 } = { x: 1, y: 4 };
console.log(x, y)  //1 4
var { x: y = 20 } = { x: 30 };
console.log(y);  //30
var { x: y = 20 } = {}
console.log(y);  //20
```

示例3：

默认值生效的条件是，对象的属性值严格等于undefined。

```javascript
var { x = 3 } = { x: undefined }
console.log(x);                   //3
var { x = 3 } = { x: null };     //null
```

示例4：

由于数组本质也是对象，因此可以**对数组进行对象解构赋值**

```javascript
let arr = [1, 2, 3];
let { 0: first, [arr.length - 1]: end } = arr;
console.log(first);  //1
console.log(end)    //3
//方括号这种写法，属于“属性名表达式
```



### 2.5 字符串的解构赋值

字符串也可以解构赋值，这是因为此时，字符串被转换成了一个类似数组的对象(即伪数组)

```javascript
const [a, b, c, d, e] = "hello";
console.table(a, b, c, d, e); //h e l l o
```

伪数组有一个length属性，因此还可以对这个属性进行解构赋值。

```javascript
let { length: len } = "hello";
console.log(len);  //5
let { 1: y } = "hello"
console.log(y);	   //e
```



### 2.6 数值和布尔值的解构赋值

#### #数值的解构赋值示例

示例1：数值会先转为对象，而数值对象里面有toString方法，从而解构赋值成功。

```javascript
let { toString: s } = 123;
console.log(s === Number.prototype.toString);  //true
console.log(s);    //ƒ toString()
```

#### #布尔值的解构赋值示例

示例：

```javascript
let { toString: a } = true;
console.log(a === Boolean.prototype.toString);  //true
```

布尔值的包装对象有toString属性，因此变量a能够复制成功



#### #undefined和null不能解构赋值

由于undefined和null无法转为对象，所以对它们进行解构赋值都会报错。

```javascript
let { __proto__: x } = undefined;  		//TypeError
let { __proto__: y } = unll;       		//TypeError
```



### 2.7 函数参数的解构赋值

#### #函数参数的解构赋值示例

示例：

```javascript
function add([a, b]) {
    return a + b;
}
var i = add([1, 2]);
console.log(i);  //3
```

#### #函数参数的解构赋值使用默认值

示例1：

```javascript
function move([a = 1, b = 2]) {
    return a + b;
}
var i = move([]);
console.log(i); //3
```

示例2：

函数参数是一个对象，对这个对象进行解构，如果成功则得到实参的值，如果失败则等于默认值

```javascript
//采用对象的解构赋值默认值
function move1({ x = 0, y = 0 } = {}) {
    return [x, y];
}
console.log(move1({ x: 3 }));   //[3, 0]
console.log(move1({ x: 4, y: 8 }));     //[4,8]
console.log(move1({}));  //[0, 0]
console.log(move1());   //[0,0]
```

上面代码的理解：

![image-20210513195851148](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210513195851148.png)



示例3：

```javascript
//为函数的参数指定默认值。
function moves({ x, y } = { x: 0, y: 0 }) {
    return [x, y];
}
console.log(moves({ x: 3, y: 8 }));  //[3, 8]
console.log(moves({ x: 3 }))    //[3, undefined]
console.log(moves({}))   //[undefined, undefined]
console.log(moves())   //[0, 0]
```

上面代码解释：

![image-20210513200111675](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210513200111675.png)

示例4：

```javascript
function fn1({ x, y = 4 }) {
    console.log(x, y)
}
fn1({})  //undefined 4
fn1({ x: 10 })  //10 4
fn1({ x: 4, y: 6 })  //4 6
fn1()  //报错
```



### 2.8 圆括号问题

ES6 的规则是，只要有可能导致解构的歧义，就不得使用圆括号。因此，**建议只要有可能，就不要在模式中放置圆括号。**

#### #不能使用圆括号的情况

1. 变量声明语句

   ```javascript
   let [(a, b, c, d)] =[1, 2, 3, 4];  报错
   let [(a, (b))]=[1, 2];  报错
   let({ x: c }) = { x: 1 };  报错
   let {(x: c)} = { };  报错
   let {(x): c} = { };  报错
   ```

2. 函数参数

   ```javascript
   function fn([(a)]) {
       return a + 1;
   }  报错
   
   function fn([x, (y)]) {
       return x + y;
   }   报错
   ```

3. 赋值语句的模式

   ```javascript
   ({ p: a }) = { p: 42 };
   ([a]) = [5];  报错
   ```

   

### 2.9 变量的解构赋值应用

#### #交换变量

```javascript
let x = 1;
let y = 2;
[x, y] = [y, x];
console.log('变化后的x和y：' + 'x:' + x + ',y:' + y);
//结果：变化后x和y的值：x:2,y:1
```

#### 函数返回多个值

函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
function fn1() {
    return [1, 2, 3];
}
var [a, b, c] = fn1();
console.log(`${a},${b},${c}`)  //1,2,3

//函数返回一个对象值
function fn2() {
    return {
        a: 'first',
        b: 'second',
        c: 'third'
    };
}
var { a, b, c } = fn2();
console.log(`${a},${b},${c}`)  //first,second,third
```

#### #函数参数的定义

解构赋值可以方便地将一组参数与变量名对应起来

```javascript
//参数是一组有次序的值
function f([x, y, z]) {
    console.log(x, y, z);  //2 4 6
}
console.log(f([2, 4, 6]));
//参数是一组无序的值
function f1({ x, y, z }) {
    console.log(x, y, z)
}
console.log(f1({ x: 1, z: 5, y: 3 }));  //1 3 5
```

#### #提取JSON数据

**解构赋值对提取 JSON 对象中的数据，尤其有用。 （重要）**

```javascript
let jsonData = {
    name: 'yyy',
    id: 2181600,
    address: 'guangdongzhonshan',
    data: [170, 52]
}
var { name, id, address, data } = jsonData;
console.log(name,id,address,data);
//结果：yyy 2181600 guangdongzhonshan (2) [170, 52]

//利用对象传播操作符
var { name, id, ...jsonData1 } = jsonData;
console.log(name, id, jsonData1);
//结果：yyy 2181600 {address: "guangdongzhonshan", data: Array(2)}
```

利用这个特点可以快速提取JSON数据的值。

#### #函数参数的默认值

```javascript
var func=function (url, {
    async = true,
    beforeSend = function () { },
    cache = true,
    complete = function () { },
    crossDomain = false,
    global = true,
    // ... more config
} = {}) {
    // ... do stuff(做事情)
};
```

指定参数的默认值，就避免了在函数体内部再写var foo = config.foo || 'default foo';这样的语句。



#### #遍历Map结构

任何部署了 `Iterator 接口`的对象，都可以用`for...of循环遍历`。

**配合变量的解构赋值**，获取键名和键值就非常方便。

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'niaho');
console.log(map); 
//Map(2) {"first" => "hello", "second" => "niaho"}

for (let item of map) {
    console.log(item);
    //打印结果：["first", "hello"]
    //打印结果：["second", "niaho"]
}
//或者
for (let [key, value] of map) {
    console.log(`${key} is ${value}`);
    //first is hello
    //second is niaho
}
//如果只想获取键名，或者只想获取键值，可以写成下面这样。
// 获取键名
for (let [key] of map) {
    // ...
}

// 获取键值
for (let [, value] of map) {
    // ...
}
```



#### #输入模块的指定方法

 加载模块时，往往需要指定输入哪些方法。解构赋值使得输入语句非常清晰。

```javascript
const { SourceMapConsumer, SourceNode } = require("source-map");
```



## 三、字符串扩展

### 3.1 字符的Unicode表示法

字符的Unicode表示法

```javascript
console.log('\u0061');  //a
/* 但是，这种表示法只限于码点在\u0000~\uFFFF之间的字符。超出这个范围的字符，必须用两个双字节的形式表示。 */

console.log("\uD842\uDFB7")  //𠮷
//ES6改进，要将码点放入大括号，就能正确解读该字符。
console.log('\u{0061}'); //a
console.log('\u{1F680}' === '\uD83D\uDE80');  //true
```

### 3.2 字符串的iterator接口

ES6 为字符串添加了 `遍历器接口` ,使得字符串可以被`for...of循环`遍历。

```javascript
for (let i of 'yang') {
    console.log(i);
    //y
    //a
    //n
    //g
}
```

### 3.3 模板字符串

模板字符串（template string）是增强版的字符串，用==反引号（`）==标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在**字符串中嵌入变量**。

1. 传统模板字符串

   ```javascript
   var name = 'Ethon';
   var age = 22;
   var address = '江西财经大学现代经济管理学院';
   console.log('你好这里是' + address + '，我是'+ name +'今年' + age + '岁了');
   //你好这里是江西财经大学现代经济管理学院,我是Ethon今年22岁了
   ```

2. ES6模板字符串

   ```javascript
   var name = 'Ethon';
   var age = 22;
   console.log(`你好这里是${address},我是${name},今年${age}岁了`)
   //你好这里是江西财经大学现代经济管理学院,我是Ethon今年22岁了
   ```



如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。

```javascript
console.log(`\`Yo\` World!`);  //`Yo` World!
```

大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

```javascript
var x = 1;
var y = 2;
console.log(`${x}+${y}=${x + y}`);  //1+2=3
let obj = { name: 'yyy', id: 2181600 };
var { name, id } = obj;
console.log(name + id)  //yyy2181600
console.log(`${obj}哈哈`) //[object Object]哈哈
```

模板字符串之中还能调用函数。

```javascript
function fn() {
    return "Hello World";
}

console.log(`foo ${fn()} bar`)  //foo Hello World bar
```

 如果模板字符串中的变量没有声明，将报错。

```javascript
let msg = `hello,${place}`;
console.log(msg);   //ReferenceError
```

如果大括号内部是一个字符串，将会原样输出。

```javascript
console.log(`${'world'}`);   //world
```



## 四、字符串的新增方法

### 4.1 includes()方法

传统确定一个字符是否在一个字符串中，只有IndexOf()方法,如果存在返回索引下标，不存在则返回-1

示例：

```javascript
var string = 'asadiqwlnapanlkdnq';
var arr = [];
var num = 0;
console.log(string.indexOf('a', 1));  //从下标1开始查找。
//返回所有符合条件的下标
var temp = string.indexOf('a');
while (temp !== -1) {
    arr.push(temp);
    temp = string.indexOf('a', temp + 1);
}
console.log(arr);  //[0, 2, 9, 11]
```

> ES6中添加了includes()新方法，可以找到字符串中是否包含某个参数字符串。

找到返回true,找不到返回false。

```javascript
var string = 'asadiqwlnapanlkdnq';
console.log(string.includes('qw')); //true
console.log(string.includes('zzx')); //flse
```

`includes()方法`支持第二个参数，表示从什么地方开始搜索。

### 4.2 startsWith()方法

startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。存在就返回true，不存在返回false。

```javascript
var string = 'asadiqwlnapanlkdnq';
console.log(string.startsWith('a')); //true
console.log(string.startsWith('z')); //false
```

`startsWith()方法`支持第二个参数，表示从什么地方开始搜索。

### 4.3 endsWith()方法

endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。存在就返回true，不存在则返回false。

```javascript
var string = 'asadiqwlnapanlkdnq';
console.log(string.endsWith('q'));   //true
console.log(string.endsWith('l'));  //false
```

`endsWith()方法`支持第二个参数，表示从什么地方开始搜索。



### 4.4 repeat()方法

`repeat方法`返回一个新字符串，表示将原字符串重复n次。

示例:

```javascript
var i = 'pyrwoaini'.repeat('3')
console.log(i)  
//结果：pyrwoainipyrwoainipyrwoaini

var y = 'hello'.repeat('2');
console.log(y)  
//结果：hellohello
```

如果`repeat方法`的参数是负数或者Infinity，会报错。

```javascript
'jdk'.repeat(Infinity);//报错
'jdk'.repeat(-1);      //报错
```

 如果`repeat方法`的参数是字符串，则会先转换成数字。

```javascript
console.log('haha'.repeat('i'));    
//结果：''
```

参数NaN、0到-1之间的小数、0到1之间的小数，都等同于0

```javascript
console.log('hahah'.repeat(-0.5));  	//''
console.log('hahah'.repeat(0.9));   	//''
console.log('abc'.repeat(NaN));     	//''
```



## 五、数值的扩展

### 5.1 二进制和八进制

在C语言中：

二进制用`0b`或`0B`作为开头(不区分大小写)，

八进制用`0`作为开头(注意不是字母O)，

十六进制用`0X`或`0x`作为开头(不区分大小写)。



在ES6，提供了二进制和八进制数值的新写法

二进制：`0b`或`0B`作为前缀。

八进制：`0o`或`0O`作为前缀。

示例1：

```javascript
console.log(0b11);  //3
console.log(0o11);  //9
```

示例2：

在ES5中的严格模式中，八进制不允许使用`0`作为前缀表示，必须使用`0o`表示

```javascript
//非严格模式
(function () {
    console.log(0o11 === 011);  //true
})();

//严格模式
(function () {
    'use strict'
    console.log(0o11 === 011); //未捕获的语法错误：严格模式下不允许使用八进制文字。
})()
```

示例3：

将`0b和0o前缀的字符串数值`转为十进制，要使用Number方法

```javascript
const num1 = Number('0b11')  //3
const num2 = Number('0o11')  //9
console.log(num1, num2);  //3 9
```



### 5.2 Number.isFinite()方法和Number.isNaN()方法

ES6 在Number对象上，新提供了`Number.isFinite()`和`Number.isNaN()`两个方法。

相当于传统的isFinite()和isNaN()方法，这两个新方法==只对数值有效==，传统方法会先把非数值转为数值再进行判断

#### 5.2.1 Number.isFinite()

Number.isFinite()用来检查一个数值是否为有限的（finite），即不是Infinity。如果是返回true，不是返回false。

示例：

```javascript
Number.isFinite(15); // true
Number.isFinite(0.8); // true
Number.isFinite(NaN); // false
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite('foo'); // false
Number.isFinite('15'); // false
Number.isFinite(true); // false
//如果参数类型不是数值，Number.isFinite一律返回false。
```

#### 5.2.2 Number.isNaN()

Number.isNaN()用来检查一个值是否为NaN。如果是NaN则返回true，不是则返回false。

示例：

```javascript
Number.isNaN(NaN) // true
Number.isNaN(15) // false
Number.isNaN('15') // false
Number.isNaN(true) // false
Number.isNaN(9 / NaN) // true
Number.isNaN('true' / 0) // true
Number.isNaN('true' / 'true') // true
//如果参数类型不是NaN，Number.isNaN一律返回false
```



### 5.3 Number.parseInt()和Number.parseFloat()

ES6 将全局方法parseInt()和parseFloat()，移植到Number对象上面，行为完全保持不变。

这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

```javascript
//ES5写法
console.log(parseInt('12.22'));  //12
console.log(parseFloat('103.32#'));  //103.32
//ES6写法
console.log(Number.parseInt('123.222'));  //1213
console.log(Number.parseFloat('10.2233#')); //10.2233
```

> ES5写法和ES6写法不同，本质是一样的

示例：

```javascript
Number.parseInt === parseInt // true
Number.parseFloat === parseFloat // true
```



## 六、函数的扩展

### 6.1 函数参数的默认值

在ES6之前，不能直接在函数的参数指定默认值，只能采用另外变通的方法。（如下）

```javascript
function log(x, y) {
    if (y === '') {
        y = ' ';
    }
    y = y || 'morning';
    console.log(x, y);
}
log('good'); 
//结果：good morning

log('good', 'morning');  
//结果：good morning

log('good', ''); 
//结果：“good  ”
```

#### 1 ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面

示例1：

```javascript
function log(x, y = 'World') {
    console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello
```

可以看到，ES6 的写法比 ES5 简洁许多，而且非常自然。

示例2：ES6函数参数默认值

```javascript
function Point(x = 0, y = 0) {
    this.x = x;
    this.y = y;
}
const p = new Point();
console.log(p);  //{x: 0, y: 0}>
```



#### 2 参数变量是默认声明的，所以不能用let和const再次声明

示例：

```javascript
function foo(x = 0) {
    let x = 1;  //Identifier 'x' has already been declared
    const x = 3;  //Identifier 'x' has already been declared
}
```

#### 3 使用参数默认值时，不能有同名参数

示例

```javascript
function foo(x, x, y) {
    //不使用参数默认值时，同名参数存在也不会报错
}
function foo1(x, x = 4, y) {
    //使用参数默认值时，参数不能同名
}
```

#### 4 函数参数默认值与解构赋值使用

参数默认值可以与解构赋值的默认值，结合起来使用。

示例1：

```javascript
// 这里只使用了对象的解构赋值默认值
function fn1({ x, y = 4 }) {
    console.log(x, y)
}
fn1({})  //undefined 4
fn1({ x: 10 })  //10 4
fn1({ x: 4, y: 6 })  //4 6
fn1();  // fn1() 报错

//为了避免调用函数没提供参数发生报错，可以使用函数参数的默认值
function fn2({ x, y = 6 } = {}) {
    console.log(x, y);
}
fn2()  //undefined 6
```

示例2：

> 函数参数采用对象的解构赋值默认值和采用函数参数的默认值区别

```javascript
// 写法1   有对象的解构赋值默认值和函数参数的默认值
function m1({ x = 0, y = 0 } = {}) {
    console.log([x, y]);
}
//写法2   没有对象的解构赋值默认值，只有函数参数的默认值
function m2({ x, y } = { x: 0, y: 0 }) {
    console.log([x, y]);
}
//看看两个函数的调用结果
//x和y没有参数的情况
m1(); //[0, 0]
m2(); //[0, 0]
//x和y都有值的情况
m1({ x: 10, y: 8 });  //[10, 8]
m2({ x: 4, y: 6 });   //[4, 6]
//x有值，y没有值
m1({ x: 1 });   //[1,0]
m2({ x: 1 });   //[1, undefined]
//x没有值，y有值
m1({ y: 3 });  //[0, 3]
m2({ y: 3 });  //[undefined, 3]
//x和y都没有值的情况
m1({});  //[0, 0]
m2({});  //[undefined, undefined]

//没有匹配项跟没有值的情况也是一样
m1({ z: 8 });  //[0, 0]
m2({ z: 6 });  //[undefined, undefined]
```

#### 5 函数参数的默认值的位置

函数参数的默认值都要放在最后面，这是隐式规定

```javascript
function m3(x, y, z = 8) {
    console.log(x, y, z);
}
m3();  //undefined undefined 8
```



#### 6 函数的length属性

函数对象的length属性的含义是，该函数预期传入的参数个数，如果某个参数有默认值，则预期传入的参数就不包括这个。

```javascript
function fn4(a, b, c, d = 3) {
    console.log(1);
}
//用console.dir可以很好的打印出函数对象所含有的属性
console.dir(fn4);
/*  arguments: null
            caller: null
            length: 3
            name: "fn4" */
console.log(fn4.length);  //3说明有3个没有默认值的参数
```

rest参数也不会计入length属性。

```javascript
function fn5(...args) {
    console.log(1);
}
console.dir(fn5);  //length为0  ==>说明rest参数不会计入到length属性中。
```



### 6.2 函数参数有默认值时的作用域

1. 一旦设置了默认值参数，在函数调用时，参数就会形成一个单独的作用域 .等调用结束，这个作用域就会消失。
2. 不设置参数默认值时，不会产生单独作用域。

示例1：

```javascript
let x = 1;
function f(x, y = x) {
    console.log(y);
}
f(2); //2
```

示例2：

函数f调用时，参数y = x形成一个单独的作用域。

这个作用域里面，变量x本身没有定义，所以指向外层的全局变量x。

```javascript
#作用域链，因为y是函数的f()的私有变量，当它被赋值x的时候，x在该作用域内内中没有，所以往作用域链上找，发现找到了，最好是1
let x = 1;
function f(y = x) {
  let x = 2;
  console.log(y);
}
f() // 1
```

如果全局变量i不存在，函数调用时也没有传递参数的话就会报错

```javascript
function fn1(y = i) {
    let i = 3;
    console.log(y);
}
fn1()  //报错
```



### 6.3 rest参数

ES6 引入 `rest 参数（形式为...变量名）`，用于获取函数的多余参数，这样就不需要使用arguments对象了 

示例：

```javascript
function render(...args) {
    console.log(args);//["塔莫", "寒冰", "松江"]
    //因为rest参数返回的是数组，所以可以使用遍历器进行for of循环遍历数据
    for (let item of args) {
        console.log(`${item}`);
        //塔莫
        //寒冰
        //松江
    }
}
render('塔莫', '寒冰', '松江');
```

**rest参数返回的是数组，这样的好处是可以利用数组的方法对参数进行操作**



如果不用rest参数而用以前的arguments，返回的是伪数组，这样还需要我们手动转数组，这样才能使用数组的方法.

```javascript
function newArr() {
    //     console.log(arguments);//伪数值
    let newArrOne = Array.from(arguments);
    newArrOne.sort((a, b) => {
        return b - a;
    })
    console.log(newArrOne); //[3, 2, 1]

}
newArr(1, 2, 3);
```



>rest参数要放在最后面，不然会报错

示例：

```javascript
// 报错
function f(a, ...b, c) {
    // ...
}
```

函数的length属性，不包括rest参数

```javascript
(function (a) { }).length;  // 1
(function (...a) { }).length; // 0
(function (a, ...b) { }).length;  // 1
```



### 6.4 严格模式

从 ES5 开始，函数内部可以设定为严格模式。(一般配合沙箱模式使用)

```javascript
function fn(a, b) {
    'use strict';
    console.log(1);
}
```

​		ES2016 做了一点修改，规定只要==函数参数使用了默认值、解构赋值、或者扩展运算符==，那么函数内部就不能显式设定为严格模式，否则会报错。

​		 这样规定的原因是，函数内部的严格模式，**要同时适用于函数体和函数参数**。

```javascript
1.函数参数有默认值设定严格模式---报错
function fn1(a, b = a) {
    'use strict';
}


2.函数使用了解构赋值设定严格模式---报错
function fn2({ a, b }) {
    'use strict';
    console.log(2);
}


3.函数使用了扩展运算符设定严格模式---报错
function fn3(...a) {
    'use strict';
    console.log(1);
}
```



>严格模式的用法

1. 在全局模式下设定严格模式。
2. 配合沙箱模式使用

配合沙箱模式下使用案例：

```javascript
const doSomething = (function (a, b, c) {
    'use strict';
    console.log(a, b, c);
    return function (value = 42) {
        return value
    };
})(1, 2, 3);
console.log(doSomething());
```



### 6.5 箭头函数

ES6允许使用`"箭头"(=>)定义函数`。

```javascript
var f = v => v;
// 等同于
var f = function (v) {
    return v;
};
```

### 6.6 箭头函数中的一些用法

>1.当参数只有一个时，可以不使用圆括号

示例：

```javascript
const fa = a => { return a };
console.log(fa(12));  //12
```

总结：**参数有一个的时候，可以使用圆括号也可以不使用圆括号。**

>2.当没有参数的时候，要使用圆括号

示例：

```js
const fn=()=>{
  return 1+2+3;
}
```



>3.参数有多个的时候，要使用圆括号把参数括起来

示例：

```javascript
const fc = (a, b, c) => {
    let result = a + b + c;
    return result;
}
console.log(fc(10, 20, 30));  //60
```

>3.代码块大于一行，则要使用花括号括起来并且使用return返回值。

示例：

```javascript
const fd = (a, b, c) => {
    let result = a + b + c;
    return result;
}
console.log(fd(10, 220, 30));  //60
```

>4.如果箭头函数直接放回一个对象，必须在对象外面加上括号，否则报错

示例：

```javascript
let getTempItem = id => ({ id: id, name: "Temp" });
console.log(getTempItem(2181600));
//结果：{id: 2181600, name: "Temp"}	
```

>5.如果箭头函数只有一行语句，且不需要返回值，可以采用如下写法

示例：

```javascript
let fn = () => void doesNotReturn();
```

>6.箭头函数的一个用处是简化回调函数

示例：

```javascript
正常写法
let i = [1, 2, 3];
const newArray1 = i.map(function (x) {
    return x + x;
})
console.log(newArray1); //[2, 4, 6]

ES6写法
let i = [1, 2, 3];
const newArray2 = i.map((x) => x * x)
console.log(newArray2); //[1, 4, 9]
```



### 6.7 箭头函数的注意点

箭头函数注意点：

1. **箭头函数没有自己的this指向，其会继承离得最近的父函数上下文环境的this指向。**
2. 不可以当作构造函数的实例对象，也就是说不能使用new命令，否则会抛出一个错误
3. 箭头函数不存在`arguments对象`，改用rest参数。
4. 不可以使用yield命令，因此箭头函数不能用作Generator函数(生成器函数)。
5. 默认绑定、隐式绑定、显示绑定和new绑定这四种绑定规则对于箭头函数不适用。（因为`箭头函数没有自己的this指向`）

==第一点尤其要注意。==



>一、箭头函数的this指向

示例：

```javascript
function foo() {
    setTimeout(() => {
        console.log('id:', this.id);
    }, 100);
}
var id = 21;
foo.call({ id: 42 }); //id: 42
```

上面代码解释：箭头函数没有自己的this指向，会继承最近的父作用域，这里foo函数通过call改变了this指向，所以箭头函数中的this就是{id:42}对象。

>二、箭头函数没有自己的this，所以不能用call()、apply()、bind()这些方法改变this指向。

示例：

```javascript
(function () {
    return [
        (() => this.x).bind({ x: 'inner' })()
    ];
}).call({ x: 'outer' });  //最后this指向了{x:'outer'}
// 结果：['outer']
```



>三、在箭头函数中，`arguments`、`super`、`new.target`这三个变量是不存在的。



### 6.8 箭头函数不适合场合

>第一个场合是定义对象的方法,并且该方法内部包括this，不适合使用箭头函数

示例：

```javascript
const cat = {
    height: 160,
    jumps: () => {
        console.log(this);  //箭头函数中对象的方法this指向了window
        this.height--;      //因为this指向window,所以这里的this.height是undefined
    }
}
console.log(cat.jumps());
```

>第二个场合是需要动态this的时候，也不应该使用箭头函数

示例：

```javascript
<button id="press">点击</button>
const button = document.getElementById('press');
button.addEventListener('click', () => {
    this.classList.toggle('on');  //这里的this指向了window,而不是button元素对象
});
```



## 七、数组的扩展

### 7.1 扩展运算符

扩展运算符是`三个点(...)`。它好比rest参数的逆运算，**将一个数组转为用逗号分隔的参数序列**。

==扩展运算符(...)会调用默认的Iterator接口==

示例：

```javascript
console.log(...[1, 2, 3]); //1 2 3
console.log(1, 2, ...['小明', '小天', '牛堡'], 6);//1 2 "小明" "小天" "牛堡" 6
console.log([...document.querySelectorAll('div')]);
//[div, div, div]
```

>1.扩展运算符主要用于函数调用

示例：

```javascript
function push(array, ...items) {
    array.push(...items);
}
function add(x, y) {
    return x + y;
}
const number = [4, 38];
add(...number);
//结果：42
```

>2.扩展运算符的注意点

1.扩展运算符与正常的函数参数可以结合使用。

```javascript
function fn1(a, b, c, d, e) {
    console.table(a, b, c, d, e); //-1 -2 1 2 8
}
fn1(-1, -2, ...[1, 2], 8)
```

2.扩展运算符后面还可以放置表达式

```javascript
let x = 10;
const arr = [...(x > 0 ? ['a'] : []), '小明'];
console.log(arr); //["a", "小明"]
```

3.如果扩展运算符后面跟的是空数组，则不产生任何效果。

```javascript
console.log([...[], 1, 2, 3,]);  //[1, 2, 3]
```



### 7.2 扩展运算符代替函数的apply方法

由于扩展运算符可以展开数组，所以不再需要apply方法，将**数组转为函数的参数**了

ES5：利用方法借用模式

```javascript
function fn1(x, y, z) {
    console.log(x, y, z);
}
const args = [1, 4, 8];
fn1.apply(null, args);
//结果：1 4 8
```

ES6：利用扩展运算符将数组拆分为以逗号分隔的参数序列，然后作为函数参数

```javascript
function fn1(x, y, z) {
    console.log(x, y, z);
}
const args = [1, 4, 8];
fn1(...args);
//结果：1 4 8
```



### 7.3 扩展运算符的应用

#### 1. 复制数组

 数组是复合的数据类型，直接赋值的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组

```javascript
const a1 = [1, 2, 3];
const a2 = a1;
console.log(a1 === a2);  //true  说明他们指向的地址是一样的
//这样造成的结果就是，当改变原来的数组，新数组也会受影响
a1[1] = 4;
console.log(a2);  //[1, 4, 3]  因为他们指向的地址都是一样的，所以新数组也被改变了
```

> 1.传统复制一个数组的方法：利用数组中的`concat()`方法变通实现复制数组

```javascript
const a3 = [3, 4, 5];
const a4 = a3.concat();
console.log(a4 === a3);   //false    
//明新数组指向的地址跟原数组不一样。  这样修改原数组也不会影响新数组
```

> 2.ES6利用扩展运算符可以实现复制一个数组

示例：

```javascript
const a3 = [3, 4, 5];
const a5 = [...a3];
console.log(a5, a5 === a3);  //[3, 4, 5] false	
```



#### 2. 合并数组

扩展运算符提供了数组合并的方法。

>ES6数组合并

示例：

```javascript
const arr1 = ['小明', '麻辣香锅', '小狗'];
const arr2 = ['啊p'];
const arr3 = ['大师兄', '二师兄'];
const newArr = [...arr1, ...arr2, ...arr3];
console.log(newArr);
//结果：["小明", "麻辣香锅", "小狗", "啊p", "大师兄", "二师兄"]
```

>传统数组合并的做法

示例：

```javascript
const arr1 = ['小明', '麻辣香锅', '小狗'];
const arr2 = ['啊p'];
const arr3 = ['大师兄', '二师兄'];
const newArr1 = arr1.concat(arr2, arr3);
console.log(newArr1);  
//结果：["小明", "麻辣香锅", "小狗", "啊p", "大师兄", "二师兄"]
```



>两种方法的注意说明

这两种方法都是浅拷贝，使用的时候需要注意。

如果是`基本数据类型的变量`，那拷贝就是变量所存的值，如果是`引用变量`，那拷贝的就是它里面的地址



#### 3. 与解构赋值结合使用

示例：

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```



#### 4. 将具有遍历器的对象转为真正的数组

原生具有iterator接口的数据结构：`Array`、`String`、`Map`、`Set`、`arguments`、`NodeList`。

示例：

```javascript
//String
console.log([...'hello']);
//结果：["h", "e", "l", "l", "o"]

//NodeList
const nodeList = document.querySelectorAll('div');
const array = [...nodeList];
console.log(array);  //[div, div, div]

//Map结构
let map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
]);

let arr = [...map.keys()]; 
//结果： [1, 2, 3]

//Set结构
let set = new Set(['Ethon', 'threey', 'Jonk']);
let Array = [...set];
console.log(Array);
//结果：["Ethon", "threey", "Jonk"]
```

2. 对于那些没有部署 Iterator 接口的类似数组的对象，`扩展运算符`就无法将其转为真正的数组，会报错。

示例：

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
let arr5 = [...arrayLike];
//Uncaught TypeError: arrayLike is not iterable
```

3. 生成器函数运行后会返回一个遍历器对象，因此也可以使用`扩展运算符`转为数组。

```javascript
const go = function* () {
    yield 1;
    yield 2;
    yield 3;
};

[...go()] 
//结果： [1, 2, 3]
```



### 7.4 Array.from()方法

Array.from方法用于==将两类对象转为真正的数组==：`类似数组的对象（array-like object）`和`可遍历（iterable）的对象`（包括 ES6 新增的数据结构 Set 和 Map）。

#### 1. 伪数组转为真数组

```javascript
const arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};
//ES6用Array.from()方法
let arr2 = Array.from(arrayLike);
console.log(arr2);  
//结果：["a", "b", "c"]

//ES5的写法
let arr1 = Array.prototype.slice.call(arrayLike);
console.log(arr1);  //["a", "b", "c"]
```



#### 2. 将具有iterator接口的数据结构转为数组

示例：

```javascript
//String:
const newArray1 = Array.from('hello');
console.log(newArray1); 
// ["h", "e", "l", "l", "o"]

//Set对象:
const namesSet = new Set(['a', 'b', 'c']);
console.log(Array.from(namesSet)); 
// ["a", "b", "c"]

//Map对象
const namesMap = new Map([['Adam', 'asd'], ['BOb', 'qwe'], ['Tom', 1]]);
console.log(Array.from(namesMap)); 
//[Array(2), Array(2), Array(2)]

//如果参数是一真正的数组，Array.from会返回一个一模一样的新数组
Array.from([1, 2, 3])  //[1, 2, 3]
```



#### 3. 将具有length属性的对象转为数组

任何有length属性的对象，都可以通过Array.from方法转为数组，而此时扩展运算符就无法转换。

示例：

```javascript
const newArray2 = Array.from({ length: 3 });
console.log(newArray2);  // [undefined, undefined, undefined]
console.log(...newArray2);
```



#### 4. Array.from()方法的第二个参数

Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。

```javascript
Array.from(arrayLike, x => x * x);
// 等同于
Array.from(arrayLike).map(x => x * x);

Array.from([1, 2, 3], (x) => x * x)
// [1, 4, 9]

function typesOf() {
    console.log(Array.from(arguments, value => typeof value));
}
typesOf(null, [], NaN)
//["object", "object", "number"]
```



### 7.5 数组实例的方法

#### 7.5.1 entries()，keys()和values()

1. ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。 它们都返回一个遍历器对象,可以用`for..of循环`进行遍历。

- values()是对键值的遍历
- keys()是对键名的遍历
- entries()是对键值对的遍历

示例：

```javascript
//keys()方法
for (let index of ['a', 'b'].keys()) {
    console.log(index);
}
// 0
// 1

//values()方法
for (let elem of ['a', 'b'].values()) {
    console.log(elem);
}
// 'a'
// 'b'

//entries()方法
for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
}
// 0 "a"
// 1 "b"
```



因为`entries()`，`keys()`和`values()`这三个方法返回的都是==**遍历器对象**==，除了可以用`for...of`进行遍历，还可以用遍历器对象的`next()方法`。

```javascript
let letter = ['a', 'b', 'c'];
let entries = letter.entries();
console.log(entries.next().value);  //[0, "a"]
console.log(entries.next().value);  //[1, "b"]
console.log(entries.next().value);  //[2, "c"]
```



#### 7.5.2Array.prototype.includes方法

2.`Array.prototype.includes方法`**返回的是一个布尔值**，表示某个数组中是否含有给定的值，与字符串中的includes方法类似。

示例：

```javascript
[1, 2, 3].includes(2);    // true
[1, 2, 3].includes(4);    // false
[1, 2, NaN].includes(NaN); // true
```

includes方法第二个参数是表示从什么位置开始搜索。如果是负数表示从后往前开始搜索。如果值大于数组长度，则会重置从0开始

```javascript
console.log([1, 2, 3].includes(2, 2)); //false
console.log([1, 2, 3].includes(3, -4));  //true //-4大于了数组长度3,则会从零开始
```

把该功能封装起来，兼容不同的浏览器。

```javascript
const contains = (() =>
	Array.prototype.includes? (arr, value) => arr.includes(value): (arr, value) => arr.some(el => el === value))();
contains(['foo', 'bar'], 'baz'); // => false
```



## 八、对象的扩展

### 8.1 对象属性的简洁表示法

ES6允许在大括号里面，**直接写入变量和函数，作为对象的属性和方法**，这样书写更加简洁。

```javascript
const foo = 'nihao';
const foo2 = { foo };
console.log(foo2); //{foo: "nihao"}
//变量foo直接写在大括号里面，这时，属性名就是变量名，属性值就是变量值

//上面的写法相当于：
const foo2 = { foo: foo };  //第一个foo是匹配模式，找到foo变量，把该变量的值赋值给foo。
console.log(foo2); //{foo: "nihao"}
```

#### 1. 属性简洁表示法示例

示例1：

```javascript
function f(x, y) {
    console.log({ x, y });
    return { x, y }
}
f(1, 3);  //{x: 1, y: 3}
//上面等同于：
function f(x, y) {
    console.log({ x: x, y: y });
    return { x: x, y: y };
}
f(3, 4)   //{x: 3, y: 4}
```

示例2：属性的简洁表示法-----方法简写

```javascript
const o = {
    method() {
        return "hello!";
    }
};
//等同于
const o = {
    method: function () {
        return "hello";
    }
};
```

实际案例：

```javascript
let birth = "2021/01/01";
const Person = {
    name: '小杨',
    birth,
    hello() { console.log('我的名字是' + this.name); }
};
Person.hello();  //我的名字是小杨
```

示例3：

属性的简洁表示法用在函数返回值会非常方便

```javascript
function getBirth() {
    const x = 1;
    const y = 19;
    console.log({ x, y });
    return { x, y }
}
getBirth();  
//结果：{x: 1, y: 19}
```

示例4：

==CommonJS模块导出一组变量的时候，就非常适合使用简洁表示法==

```javascript
let ms = {};
function getItem(key) {
    return key in ms ? ns[key] : null;
}
function setItem(key, value) {
    ms[key] = value;
}
function clear() {
    ms = null;
}
module.exports = { getItem, setItem, clear };
//上面的导出相当于
module.exports = {
    getItem: getItem,
    setItem: setItem,
    clear: clear
};
```

示例5:

==属性的赋值器(setter)和取值器(getter)，事实上也是采用这种写法.==

```javascript
const cart = {
    _wheels: 4,
    get wheels() {
        return this._wheels;
    },
    set wheels(value) {
        if (value < this._wheels) {
            //抛出错误
            throw new Error('数值太小了！');
        }
        this._wheels = value;
    }
}
console.log(cart._wheels);  //4
console.log(cart.wheels);  //4
```

#### 2. 注意的地方

简写的对象不能用作构造函数，会报错

```javascript
const Obj = {
    //f==>简写了  ==>相当于f:function(){}
    f() {
        this.foo = 'bar';
    }
};
new Obj.f()  //报错
```



### 8.2 属性名表达式

JavaScript 定义对象的属性，有两种方法。

方法一：直接使用标识符作为属性名 ====>`属性名标识符`。

```javascript
obj.name='threey';
```

方法二：使用表达式作为属性名 ====>`属性名表达式`

```
obj['xxx']=123;
```



在ES5中如果直接使用字面量定义对象，只能**使用标识符定义对象属性**。

```javascript
var obj = {
    foo: true,
    abc: 123,
};
```

在ES6中可以在字面量定义对象的时候**使用表达式定义对象的属性**

```javascript
let propKey = 'adress';
const obj1 = {
    [propKey]: true,
    ['nihaoa']: 'hah',
}
console.log(obj1);  //{adress: true, nihaoa: "hah"}
```

使用`属性名表达式`定义对象属性名示例：

```javascript
let sayWords = 'last word';
const obj2 = {
    'first word': 'hello',
    [sayWords]: 'hahaha'
};
console.log(obj2['first word']);  //hello
console.log(obj2[sayWords]);   //hahaha
```



>属性名表达式不能和属性的简洁表示法同时使用，否则会报错

示例：

```javascript
// 报错
const foo = 'bar';
const bar = 'abc';
// const baz = { [foo] };  //报错

//正确
const lbw = 'nihao';
const lob = { lbw };
const ppd = { [lbw]: 'hahah' }
console.log(lob);  //{lbw: "nihao"}
console.log(ppd);  //{nihao: "hahah"}
```



>属性名表达式如果是一个对象

注意，属性名表达式如果是一个对象，默认情况下会自动将对象转为`字符串[object Object]`，这一点要特别小心。

```javascript
const keyA = { a: 1 };
const keyB = { b: 2 };

const myObject = {
    [keyA]: 'valueA',
    [keyB]: 'valueB'
};

console.log(myObject);  // Object {[object Object]: "valueB"}
```



### 8.3 属性的可枚举性和遍历

#### 1. 可枚举性

对象的每个属性都有一个描述对象（Descriptor），用来控制该属性的行为。

ES5的`Object.getOwnPropertyDescriptor方法`会返回某个对象属性的描述对象(descriptor)

示例：

```javascript
let obj = { foo: 123, name: 'yyy' };
const result = Object.getOwnPropertyDescriptor(obj, 'foo')
console.log(result);
//结果：
//{
    //value: 123,
    //writable: true,
    //enumerable: true,
    //configurable: true
//}   
```

描述对象的==enumerable属性，称为“可枚举性”==，如果该属性为false，就表示某些操作会忽略当前属性。

目前有四个操作会忽略`enumerable`为`false`的属性：

- for...in循环：只遍历自身对象和继承的可枚举的属性。(不含Symbol属性)
- Object.keys()：返回自身对象的所有可枚举的属性的键名。返回的是一个数组。(不含Symbol属性)
- JSON.stringify()：只串行化对象自身的可枚举属性。(不含Symbol属性)
- Object.assign()：忽略`enumerable`为`false`的属性，只拷贝对象自身的可枚举的属性。(包含Symbol数据类型属性)

这四个操作之中，前三个是 ES5 就有的，最后一个`Object.assign()`是 ES6 新增的

==上面四个方法只有`for...in`会返回继承的属性，其他三个方法都会忽略继承的属性，只处理对象自身的属性。==



#### 2. 属性的遍历

> 1.ES6 一共有 5 种方法可以遍历对象的属性。

1. **for...in**：`for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）。
2. **Object.keys(obj)**：`Object.keys`**返回一个数组**，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。
3. **Object.getOwnPropertyNames(obj)**：
4. **Object.getOwnPropertySymbols(obj)**
5. **Reflect.ownKeys(obj)**：`Reflect.ownKeys`**返回一个数组**，包含**对象自身的（不含继承的）所有键名**，不管键名是 Symbol 或字符串，也不管是否可枚举。

==总结：我们尽量不要用for...in循环，而是用Object.keys()方法遍历。==



>2.这5个方法都遵守的属性遍历次序规则

- 首先遍历所有数值键，按照数值升序排列。
- 其次遍历所有字符串键，按照加入时间升序排列。
- 最后遍历所有 Symbol 键，按照加入时间升序排列。

示例：

```javascript
Reflect.ownKeys({ [Symbol()]:0, b:0, 10:0, 2:0, a:0 })
// ['2', '10', 'b', 'a', Symbol()]
```



### 8.4 对象的扩展运算符

对象的扩展运算符(...)用于取出参数对象的`所有可遍历的属性`，拷贝到当前对象之中。

==扩展运算符(...)会调用默认的Iterator接口==

示例：

```javascript
let obj1 = {
    a: 3,
    b: 5
};
let obj2 = { ...obj1 };
console.log(obj1 === obj2);  //false
console.log(obj2);  //{a: 3, b: 5}
```



由于数组是特殊的对象，所以**对象的扩展运算符也可以用于数组**

```javascript
let foo = { ...['a', 'b', 'c'] };
console.log(foo);  //{0: "a", 1: "b", 2: "c"}
```



如果扩展运算符后面是一个空对象，则没有任何效果。

```javascript
console.log({ ...{}, a: 1 }); //{a: 1}
```



如果扩展运算符后面不是对象，则会自动将其转为对象

```
let obj3 = { ...1 };
console.log(obj3);  //{}
// 等同于 {...Object(1)}
```



如果对象的扩展运算符后面是`字符串`，它会自动转成一个`类似数组的对象`，因此返回的不是空对象

```javascript
let obj4 = { ...'number' };
console.log(obj4);
//{0: "n", 1: "u", 2: "m", 3: "b", 4: "e", 5: "r"}
```



**对象的扩展运算符等同于使用`Object.assign()方法`。**

`Object.assign()方法`将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```javascript
let obj1 = {
    a: 3,
    b: 5
};
let aClone = { ...obj1 };
let aClonee = Object.assign({}, obj1);
console.log(aClone);  //{a: 3, b: 5}
console.log(aClonee); //{a: 3, b: 5}
```



数组的扩展运算符可以用于合并数组，同理，==对象的扩展运算符可以用于合并两个对象==。

示例：

```javascript
const a = {
    name: 'jiangxi',
    adress: 'guangdon',
}
const b = {
    money: 10000,
    id: 0909,
}
let ab = { ...a, ...b };
console.log(ab);
//{name: "jiangxi", adress: "guangdon", money: 10000, id: 909}
```



如果用户自定义的对象属性放在对象扩展运算符后面，则扩展运算符内部的同名属性会被覆盖。

```javascript
const a = {
    name: 'jiangxi',
    adress: 'guangdon',
}
const b = {
    money: 10000,
    id: 0909,
}
var aWithOverrides = { ...a, name: 1, adress: 3 };
console.log(aWithOverrides);  //{name: 1, adress: 3}
//等同于
var aWithOverrides = { ...a, ...{ name: 2, adress: 4 } };
console.log(aWithOverrides);  //{name: 2, adress: 4}
//等同于
var name = 'yyy', adress = 'shanghai', aWithOverrides = { ...a, name, adress }
console.log(aWithOverrides);
//{name: "yyy", adress: "shanghai"}
//利用这一点，我们可以轻易对现有对象的部分属性进行修改
const newObject = {
    ...a,
    name: 'yyylllooo'
}
console.log(newObject);
//{name: "yyylllooo", adress: "guangdon"}
//是否相同？
console.log(newObject === a);  //false
```



如果把自定义属性放在扩展运算符前面，就变成设置新对象的默认属性值

```javascript
const a = {
    name: 'jiangxi',
    adress: 'guangdon',
}
var newObj = { name: 'Ethon', adress: 'shenghua8', ...a }
console.log(newObj);
//{name: "jiangxi", adress: "guangdon"}
// 等同于
var newObj = Object.assign({}, { name: 1, adress: 2 }, a);
console.log(newObj);
//{name: "jiangxi", adress: "guangdon"}
// 等同于
var newObj = Object.assign({ name: 1, adress: 2 }, a);
console.log(newObj);
//{name: "jiangxi", adress: "guangdon"}
```



与数组的扩展运算符一样，对象的扩展运算符后面可以跟表达式。

```javascript
var x = 6;
const newObj2 = {
    ...(x > 1 ? { a: 1 } : {}),
    b: 10,
};
console.log(newObj2);  //{a: 1, b: 10}
```



## 九、对象的新增方法

### 9.1 Object.is()方法

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



> 在ES6中推出了Object.is()方法来解决这类问题。

```javascript
console.log(Object.is(+0, -0)); // false
console.log(Object.is(NaN, NaN)); //true
```



### 9.2 Object.assign()方法

Object.assign()方法**用于对象的合并**，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```javascript
const target1 = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target1, source1, source2);
console.log(target1); // {a:1, b:2, c:3}
```



Object.assign()方法的第一个参数是目标对象，后面的参数都是源对象。

==注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。==

```javascript
const target2 = { a: 1, b: 1 };

const source3 = { b: 2, c: 2 };
const source4 = { c: 4 };

Object.assign(target2, source3, source4);
console.log(target2); // {a:1, b:2, c:4}
```



如果只有一个参数，Object.assign()会直接返回该参数

```javascript
const obj = { name: 'ROSE' };
const obj2 = Object.assign(obj);
console.log(obj === obj2);  //true
```



用Object.assign()方法，如果参数不是对象，则会先转为对象，然后返回。

```javascript
const obj3 = typeof Object.assign(1);
console.log(obj3);  //object
```



由于undefined和null无法转成对象，所以如果它们作为第一个参数，就会报错

```javascript
Object.assign(undefined); //报错
Object.assign(null);      //报错
```



如果undefined和null作为第二个参数，不会报错，但会跳过此类属性

```javascript
const obj4 = {
    name: 'Ethon',
    age: 30,
}
console.log(Object.assign(obj4, undefined) === obj4);
//true   说明跟原对象相同，也就是说源属性undefined跳过了
console.log(Object.assign(obj4, null) === obj4);
//true   说明跟原对象相同，也就是说源属性null跳过了
```



对于字符串、数组、布尔值这些类型，==只有字符串作为源对象起作用==，其它不起作用（会跳过该属性的赋值）

```javascript
const v1 = 'Rose';   ---string
const v2 = true;     ---boolean
const v3 = 20;       ---number
const obj5 = Object.assign({}, v1, v2, v3);
console.log(obj5);
//{0: "R", 1: "o", 2: "s", 3: "e"}
//为什么字符串可以，数字类型和布尔类型不行？这是因为只有字符串的包装对象，会产生可枚举属性。
console.log(Object(v1));
// 0: "R"
// 1: "o"
// 2: "s"
// 3: "e"
// length: 4
```



**Object.assign()拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性，也不拷贝不可枚举的属性(enumerable:false);）**

```javascript
const newObject = Object.assign({ name: 'Ethon' }, Object.defineProperty({}, 'adress', {
    value: 'shenghuaweiji8',
    enumerable: false
}
                                                                        ))
console.log(newObject);
//{name: "Ethon"}
// 如果enumerable是true,则可以拷贝
```



> 属性名为Symbol值的属性，也会被Object.assign()拷贝

示例：

```javascript
Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
// { a: 'b', Symbol(c): 'd' }
```



### 9.3 Object.assign()方法的注意点

#### 1. 浅拷贝

`Object.assign()方法`实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```javascript
const obj1 = { a: { b: 2 } };
const obj2 = Object.assign({}, obj1);
obj1.a.b = 10;
console.log(obj1);  //a: {b: 10}
console.log(obj2);  //a: {b: 10}
```

上面代码中，源对象obj1的a属性的值是一个对象，Object.assign()拷贝得到的是这个对象的引用。这个对象的任何变化，都会反映到目标对象上面。

#### 2. 同名属性的替换

```javascript
const target = { a: { b: 'haha', c: "nihao" } };
const source = { a: { b: "Ethon" } };
const result = Object.assign(target, source);
console.log(result);  //a: {b: "Ethon"}
```

对于这种嵌套的对象，一旦遇到同名属性，Object.assign()的处理方法是替换，而不是添加。

#### 3. 可以用来处理数组

 Object.assign()可以用来处理数组，但是会==把数组视为对象==。

```javascript
Object.assign([1, 2, 3], [4, 5]);
//[4，2，5]
```

上面代码中，**Object.assign()把数组视为属性名为 0、1、2 的对象**，因此源数组的 0 号属性值4覆盖了目标数组的 0 号属性1。

#### 4. 取值函数的处理

Object.assign()只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

示例：

```javascript
const source = {
    get foo() { return 1 }
};
const target1 = {};

Object.assign(target1, source1);
//结果： { foo: 1 }
```

上面代码中，source对象的foo属性是一个取值函数，Object.assign()不会复制这个取值函数，只会拿到值以后，将这个值复制过去。



### 9.4 Object.assign()方法常用用途

#### 1. 为对象添加属性

```javascript
class Point {
    constructor(x, y) {
        Object.assign(this, { x, y });
    }
}
var p = new Point(10, 20);
console.log(p);
//结果：Point {x: 10, y: 20}
```

上面方法通过Object.assign()方法，将x属性和y属性添加到Point类的对象实例。

#### 2.为对象添加方法

```javascript
Object.assign(SomeClass.prototype, {
    someMethod(arg1, arg2) {
	....
    },
    anotherMethod() {
	....
    }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
	.....
};
SomeClass.prototype.anotherMethod = function () {
	.....
};
上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用assign()方法添加到SomeClass.prototype之中。
```

#### 3. 克隆对象

```javascript
function clone(origin) {
    return Object.assign({}, roigin);
}
```

这种方法的克隆，只能克隆原始对象自身的值，不能克隆它继承的值



#### 4. 合并多个对象

```javascript
const obj1 = {
    name: 'Ethon',
    age: 20
}
const obj2 = {
    money: 10000,
    adress: 'huangpu'
}
const merge = (target, ...sources) => Object.assign(target, ...sources);
console.log(merge(obj1, obj2));
//{name: "Ethon", age: 20, money: 10000, adress: "huangpu"}

//如果合并为一个新对象则用空对象进行合并
const merge1 = (...sources) => Object.assign({}, ...sources);
console.log(merge1(obj1, obj2));
//{name: "Ethon", age: 20, money: 10000, adress: "huangpu"}
```



#### 5. 为属性指定默认值

```javascript
const DEFAULTS = {
    logLevel: 0,
    outputFormat: 'html'
};

function processContent(options) {
    options = Object.assign({}, DEFAULTS, options);
    console.log(options);
    // ...
}
processContent({ url: 'www.baidu.com' })
```



### 9.5 Object.keys()

Object.keys()方法返回**自身对象所有可枚举的属性的键名**（不含继承也不含Symbol属性），==返回的是一个数组。==

```javascript
var obj = {
    name: 'Ethon',
    age: 100
};
console.log(Object.keys(obj));  
//结果：["name", "age"]
```



### 9.6 Object.values()

Object.values()方法是ES2017引进来的，它可以返回`所有可枚举属性的键值(不含继承，也不含Symbol值)`。它返回的是一个数组。

```javascript
const obj1 = {
    [Symbol()]: 123,
    name: 'ROSE',
    age: 100
};
console.log(Object.values(obj1));
//["ROSE", 100] 
```



如果Object.values()方法的参数是一个字符串，则会返回各个字符组成的数组。

```javascript
const result = Object.values('java');
console.log(result);
//["j", "a", "v", "a"]
```



如果参数不是对象，Object.values会将其转为对象，其中number、boolean会转为基本包装对象，Object.values对这些对象会返回空数组

```javascript
Object.values(42) // []
Object.values(true) // []
```



### 9.7 Object.entries()

`Object.entries()` 方法会遍历对象自身所有可遍历属性（不含继承,也不含Symbol值）,它==返回的是键值对数组。==

```javascript
const obj = { foo: 'bar', baz: 42 };
console.log(Object.entries(obj));
//[ ["foo", "bar"], ["baz", 42] ]

//忽略Symbol值
Object.entries({ [Symbol()]: 123, foo: 'abc' });
// [ [ 'foo', 'abc' ] ]
```

除了返回值不一样，该方法的行为与Object.values基本一致。



>Objecet.entries()的基本用途

Object.entries的基本用途是==遍历对象的属性==。

```javascript
let obj2 = { one: 1, two: 2 };
for (let [k, v] of Object.entries(obj2)) {
    console.log(
        `${JSON.stringify(k)}: ${JSON.stringify(v)}`
    );
}
// "one": 1
// "two": 2
```

Object.entries方法的另一个用处是，将对象转为真正的Map结构。

```javascript
const obj3 = { foo: 'bar', baz: 42 };
const map = new Map(Object.entries(obj3));
map // Map { foo: "bar", baz: 42 }
```



## 十、Symbol

### 10.1 Symbol概述

ES6 引入了一种新的`原始数据类型Symbol`，表示独一无二的值。

`Symbol()函数` 会返回**symbol**类型的值，该类型具有==静态属性==和==静态方法==。

```javascript
let s = Symbol();
console.log(typeof s);  //symbol
```

对象的属性名由原来的**只有字符串类型现在变成了有两种数据类型， 一个是String类型，另一个是Symbol类型**

```javascript
const money = Symbol('money');
const obj = {
    name: 'Ethon',
    age: 20,
    adress: 'guangdon',
    [money]: 10000
}
console.log(obj);
//{name: "Ethon", age: 20, adress: "guangdon", Symbol(money): 10000}
```



> 凡是属性名属于Symbol类型，都是独一无二的，可以保证不会与其他属性名产生冲突。

示例：

```javascript
let name = Symbol();
console.log(typeof name);
//结果：symbol
```

上面变量name就是一个独一无二的值。typeof运算符的结果表明变量name是Symbol数据类型，而不是字符串之类的其他类型。



`Symbol函数`前不能使用new命令，否则会报错。因为==**生成的Symbol是一个原始类型的值，不是对象**==

```javascript
let age = new Symbol();  //报错
```

由于Symbol值不是对象，所以不能添加属性。，它是一种类似于字符串的数据类型。



**Symbol函数可以用一个字符串作为参数，表示对Symbol实例的描述**，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```javascript
let s1 = Symbol('name');
let s2 = Symbol('adress');
console.log(s1);  //Symbol(name)
console.log(s2);  //Symbol(adress)
console.log(typeof s1, typeof s2);  //symbol symbol
console.log(s1.toString());  //“Symbol(name)” 字符串类型
console.log(s2.toString());  //”Symbol(adress)“ 字符串类型
```



如果`Symbol函数`的参数是一个对象，则会调用该对象的toString方法，将其转为字符串，然后才生成一个Symbol值。

```javascript
const obj2 = {
    toString() {
        return 'abcd';
    }
};
const result = Symbol(obj2);
console.log(result);  //Symbol(abcd)
```



`Symbol函数`的参数只是表示==对当前Symbol值的描述==，即相同参数的`Symbol函数`的返回值是不相等的，即独一无二。

```javascript
const a = Symbol();
const b = Symbol();
console.log(a === b);   //false

//有参数的情况
const c = Symbol('obj');
const d = Symbol('obj');
console.log(c === d);  //false
```



Symbol值不能与其他类型进行运算，会报错

```javascript
let sym = Symbol('i love you');
"ROSE" + sym;  //报错
`ROSE${sym}`;  //报错
```



Symbol值可以转为`字符串`、`布尔值`，但不能转为`数值`

```javascript
let yyy = Symbol('hello Ethon');
console.log(String(yyy));     //Symbol(hello Ethon)
console.log(yyy.toString());  //Symbol(hello Ethon)
console.log(Boolean(yyy));   //true
console.log(Number(yyy));  //报错   转数值会报错
```



### 10.2 Symbol. prototype. description

ES2019提供了一个实例属性方法`description`,用来返回Symbol的描述

```javascript
const a1 = Symbol('hahah');
console.log(a1.description);  //hahah
```



### 10.3 作为对象属性名的Symbol

前面也说了==Symbol是独一无二的值,==所以每一个Symbol值都是不相等的,借此可以使用Symbol类型来表示对象的属性名,这样可以保证不会出现同名的属性。

==Symbol值作为对象属性名时，该属性是公开属性，不是私有属性。==

示例：

```javascript
const myName = Symbol();

//第一种写法
const a = {
    [myName]: 'Ethon'
}
console.log(a);  //{Symbol(): "Ethon"}

//第二种写法
const a = {};
a[myName] = 'Ethon';
console.log(a);    //{Symbol(): "Ethon"}

//第三种写法
const a = {};
Object.defineProperty(a, myName, { value: 'Ethon' });

// 以上三种写法结果情况都一样（如下）
console.log(a[myName]);  //Ethon
```



>作为对象属性名的Symbol注意点：

==**Symbol值作为对象属性名时,不能用点运算符**.因为点运算符后面总是跟着字符串,这样会把Symbol当作字符串属性名而不是Symbol值属性名这个要时刻小心.==

示例：

```javascript
//用点运算符时是被当作字符串类型的属性名。
const mySymbol = Symbol();
const obj3 = {};
obj3.mySymbol = 'Hello';
console.log(typeof obj3.mySymbol);  
//string
console.log(obj3[mySymbol]);  //undefined
console.log(obj3['mySymbol']);   //"Hello"

//如果要用Symbol值作为对象属性名，则要用方括号
const obj4 = {};
obj4[mySymbol] = 'threey'
console.log(obj4[mySymbol]);  
//结果：threey
```



在对象的内部，使用Symbol值定义属性时，==**Symbol值必须放在方括号之中**==。

```javascript
let s = Symbol('function1');
let obj4 = {
    [s]: function (arg) {
        console.log(arg);
    }
};
obj4[s](2181600) //2181600
```

Symbol值与属性的简洁表示法一起用

```javascript
let s = Symbol('function1');
let obj5 = {
    [s](arg) {
        console.log(arg);
    }
};
obj5[s](1600)   //1600
```



Symbol类型还可以用于定义一组常量，保证这组常量的值都是不相等的。

```javascript
const log = {};
log.fangfa = {
    ONE: Symbol('Ethon'),
    TWO: Symbol('ROSE'),
    THREEY: Symbol('kakaka')
};
console.log(log.fangfa.ONE, 'Ethon message');
//结果：Symbol(Ethon) "Ethon message"
```



### 10.4 Symbol属性名的遍历

Symbol作为对象的属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，也不会被`Object.keys()`、`Object.getOwnPropertyNames()`、`JSON.stringify()`返回。



但是，它也不是私有属性，有一个`Object.getOwnPropertySymbol()`方法，可以获取指定对象的所有Symbol属性名。**该方法返回一个给定对象自身的所有 Symbol 属性的数组**。

```javascript
const obj = {};
let a = Symbol('a');
let b = Symbol('b');

obj[a] = 'Hello';
obj[b] = 'World';
const result = Object.getOwnPropertySymbols(obj);
console.log(result);
//结果：[Symbol(a), Symbol(b)]
//上面Object.getOwnPropertySymbols()方法可以获取所有Symbol属性名。
```



`Object.getOwnPropertySymbols()方法`与`for...in循环`、`Object.getOwnPropertyNames`方法进行对比的例子

```javascript
const obj2 = {};
const name = Symbol('name');
obj2[name] = 'Ethon';
for (let i in obj2) {
    console.log(i);    //没有输出
}
console.log(Object.getOwnPropertyNames(obj2));
//[]
console.log(Object.getOwnPropertySymbols(obj2));
//[Symbol(name)]
```



新的API，`Reflect.ownKeys()方法`可以返回所有类型的键名，包括常规键名和Symbol键名。(在第八章的对象扩展的属性遍历中有提到。)

示例：

```javascript
const obj3 = {
    name: 'yyy',
    [Symbol('id')]: 2181600,
    age: 22
};
const result1 = Reflect.ownKeys(obj3);
console.log(result1);
//["name", "age", Symbol(id)]
```



### 10.5 Symbol.for()方法

#### 1. Symbol.for()方法的用途

有时我们希望使用同一个Symbol值，Symbol.for()方法可以做到这一点。

`Symbol.for方法`的功能实现：它首先会看参数是由存在，如果已存在了就直接返回，如果不存在就新建一个Symbol类型的值。

```javascript
const res1 = Symbol.for('result');
const res2 = Symbol.for('result');
console.log(res1 === res2);  //true

//上面代码中，s1和s2都是 Symbol 值，但是它们都是由同样参数的Symbol.for方法生成的，所以实际上是同一个值。
```



#### 2. Symbol.for()和Symbol()的区别

参数如果相同，Symbol.for()每次放回的Symbol值都一样，而Symbol()则不管参数是否一样，每次放回都是不一样的Symbol值。

`Symbol.for()`与`Symbol()`这两种写法，都会生成新的 Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。

```javascript
const a1 = Symbol('yyy');
const a2 = Symbol('yyy');
console.log(a1 === a2);     //false

const a3 = Symbol.for('yyy');
const a4 = Symbol.for('yyy');
console.log(a3 === a4);     //true
```



### 10.6 Symbol.keyFor()方法

`Symbol.keyFor()`方法返回一个已登记的 Symbol 类型值的`描述`。

```javascript
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined

//上面代码中，变量s2属于未登记的 Symbol 值，所以返回undefined。
```

注意，`Symbol.for()`为 Symbol 值登记的名字，是全局环境的，不管有没有在全局环境运行。

```javascript
const a1 = Symbol.for('Ethon');
const a2 = Symbol('Ethon');
console.log(Symbol.keyFor(a1));   //Ethon
console.log(Symbol.keyFor(a2));   //undefined
```



## 十一、Set和Map

### 11.1 Set

**ES6提供了新的数据结构`Set`。它类似于数组，但是成员的值都是唯一的，没有重复的值。**

Set本身是一个构造函数，用来生成Set对象。

```javascript
const s = new Set();
[2, 3, 4, 5, 6, 2, 2, 3, 2].forEach(x => s.add(x));
console.log(...s);
//利用对象的扩展运算符查看  结果：2 3 4 5 6
```

#### 11.1.1 Set集合中的值可以是任意类型。

代码示例：

```js
const set1 = new Set([1, 2, 3, 4, 5, 6])

//添加对象
set1.add({}) 
//添加数组
set1.add([11,12,13,14]) 
//添加字符串
set1.add('hello')
//添加布尔值
set1.add(true)
```

结果图示:

![image-20211222164814655](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211222164834.png)

#### 11.1.2 Set实现去重。

`Set函数 `接收一个数组，可以实现去重。

示例：

```javascript
const set = new Set([1, 1, 2, 3, 5, 6]);
console.log(Array.from(set));  //[1, 2, 3, 5, 6]
console.log([...set]);  //[1, 2, 3, 5, 6]
```

#### 11.1.3 Set构造函数还能去除字符串里的重复字符。

示例：

```javascript
const result = new Set("aabbcaae");
const string = [...result].join('');
console.log(string); //abce
```

#### 11.1.4 向Set对象加入对象值都是不相等的。

示例：

```javascript
const set1 = new Set();
set1.add({});
set1.add({});
console.log(set1.size);  //2==>说明新增加的空对象不相等。
```



### 11.2 Set实例的属性和方法

#### 1. Set具有的属性

Set实例的具有的属性有：

- `Set.prototype.constructor`：构造函数
- `Set.prototype.size`：返回Set实例的成员总数。

```javascript
const set = new Set();
console.dir(set);
```



#### 2. Set实例的方法---操作方法

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为Set的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

示例：

```javascript
set.add(1).add(2).add(3);
console.log(set); // {1, 2, 3}
console.log(set.has(1));  //true
console.log(set.delete(2)); //true
console.log(set);  //删除后的set对象：{1, 3}
set.clear();
console.log(set);  //清除后的set对象：{}
```



#### 3. Set实例的方法---遍历方法

- `Set.prototype.keys()`：返回键名的遍历器。
- `Set.prototype.values()`：返回键值的遍历器。
- `Set.prototype.entries()`：返回键值对的遍历器。
- `Set.prototype.forEach()`：使用回调函数遍历每个成员。

示例：

```javascript
const set1 = new Set(['red', 'green', 'yellow']);
console.log(set1.keys());
//SetIterator {"red", "green", "yellow"}
for (let i of set1.keys()) {
    console.log(i);
}
//red
//green
//yellow
```

因为Set对象没有键名，只有键值，所以keys方法和values方法行为完全一致。

```javascript
const set1 = new Set(['red', 'green', 'yellow']);
for (let i of set1.values()) {
    console.log(i)
}
//red
//green
//yellow

for (let i of set1.entries()) {
    console.log(i);
}
//["red", "red"]
//["green", "green"]
//["yellow", "yellow"]
```



> 可以直接用`for...of循环`遍历Set。

示例：

```javascript
let set2 = new Set(['red', 'green', 'blue']);

for (let x of set2) {
    console.log(x);
}
// red
// green
// blue
```



>遍历的应用

扩展运算符(...)内部使用for...of循环，所以可以用于Set结构

```javascript
let set3 = new Set(['Ethon', 'Rose', 'jack']);
console.log([...set3]);//["Ethon", "Rose", "jack"]
// 也可以这样使用
console.log([...new Set(['Ethon', 'Rose', 'jack'])]);
//["Ethon", "Rose", "jack"]
```

遍历Set的同时改变Set结构

```javascript
// 方法一
let set4 = new Set([1, 2, 3]);
set4 = new Set([...set4].map(val => val * 2));
console.log(set4);
// set4的值是2, 4, 6

// 方法二
let set5 = new Set([1, 2, 3]);
set5 = new Set(Array.from(set5, val => val * 2));
console.log(set5);
// set5的值是2, 4, 6
```



### 11.3 Map

JavaScript 的对象（Object），**本质上是键值对的集合（Hash 结构）**

在ES6中提供了`Map`数据结构。它类似于对象，也是键值对的集合，**但是“键”的范围不限于字符串，各种类型的值都可以当作键。**



Object结构：==“字符串--值”的对应==

Map结构:=="值--值"的对应==,是一种更完善的Hash结构。

```javascript
const m = new Map();
const o = { p: 'hello world' };
m.set(o);
console.log(m.get(o));
```



Map作为构造函数，可以接受一个数组作为参数。该数组的成员是一个个表示`键值对的数组`。

```javascript
const map = new Map([['name', 'MIYA'], ['age', 20], ['title', 'Author']]);
console.log(map);
//{"name" => "MIYA", "age" => 20, "title" => "Author"}
console.log(map.size); //3
console.log(map.get('age'));  //20
```



Map构造函数接受数组作为参数，实际上执行的是下面的算法

```javascript
const items = [['name', '张三'], ['age', 22], ['title', 'Author']];
const map2 = new Map();
items.forEach(([key, values]) => map2.set(key, values));
```



Map构造函数接收的参数是任何具有 `Iterator接口` ，且每个成员都是一个双元素的数组数据结构类型。

如下Map构造函数就接收了Set结构且里面每个成员都是双元素作为参数。

```javascript
const set = new Set([['foo', 1], ['bar', 2]]);
console.log(set); //{ Array(2), Array(2) }
const map1 = new Map(set);
console.log(map1);
//{"foo" => 1, "bar" => 2}
const m2 = new Map([['name', 'Ethon']]);
const m3 = new Map(m2);
console.log(m3);  //{"name" => "Ethon"}
```



如果对同一个键多次赋值，后面的值将覆盖前面的值。

示例：

```javascript
const map3 = new Map();
map3.set('name', 'Ethon');
map3.set('name', 'Rose');
console.log(map3); //{"name" => "Rose"}  说明前面的值被覆盖了
```

如果读取一个未知的键，则返回undefined。

```javascript
console.log(new Map().get('asdasd'));   //undefined
```

如果读取的键没有value值，也会返回undefined。

```javascript
const Map1 = new Map();
Map1.set('o');
console.log(Map1.get(o));
//undefined
```



只有同一个对象的引用，Map结构才将其视为同一个键。

```javascript
const map4 = new Map();
map4.set(['a'], '555');
map4.get(['a'])  //undefined
```

表面是同一个键，但实际上这是两个不同的数组实例，内存地址是不一样的，因此get方法无法读取该键，返回undefined。



### 11.4 Map实例的属性和操作方法

#### 1. 属性

size属性：size属性返回 Map 结构的成员总数。

```javascript
const map = new Map();
map.set('foo', true);
map.set('bar', false);
map.size    // 2
```

#### 2. 操作方法

一、Map.prototype.set(key,value):set方法设置键名key对应的键值为value，然后返回整个 Map 结构。

```javascript
const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined

//set方法返回的是当前的Map对象，因此可以采用链式写法。
let map1 = new Map()
.set(1, 'a')
.set(2, 'b')
.set(3, 'c');
console.log(map1);
//Map(3) {1 => "a", 2 => "b", 3 => "c"}
```

二、Map.prototype.get(key):get方法读取key对应的键值，如果找不到key，返回undefined。

```javascript
const map2 = new Map();
map2.set('id', 2181600);
console.log(map2.get('id'));  //2181600
```

三、Map.prototype.has(key):has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```javascript
const m3 = new Map();

m3.set('edition', 6);
m3.set(262, 'standard');
m3.set(undefined, 'nah');

m3.has('edition')     // true
m3.has('years')       // false
m3.has(262)           // true
m3.has(undefined)     // true
```

四、Map.prototype.delete(key):delete方法删除某个键，成功返回true。如果删除失败，返回false

```javascript
const m4 = new Map();
m4.set(undefined, 'nah');
m4.has(undefined)     // true

m4.delete(undefined)
m4.has(undefined)       // false

```

五、Map.prototype.clear():clear方法清除所有成员，没有返回值。

```js
let map3 = new Map();
map3.set('foo', true);
map3.set('bar', false);

map3.size // 2
map3.clear()
map3.size // 0
```



#### 3. 遍历方法

- Map.prototype.keys()：返回键名的遍历器。
- Map.prototype.values()：返回键值的遍历器。
- Map.prototype.entries()：返回所有成员的遍历器。
- Map.prototype.forEach()：遍历 Map 的所有成员。



> **Map 结构的默认遍历器接口（Symbol.iterator属性），就是entries方法。**

```javascript
console.log(map[Symbol.iterator] === map.entries);
// true
```



> Map结构转为数组结构，比较快速的方法是使用扩展运算符(...)

示例：

```javascript
const map4 = new Map([[1, 'one'], [2, 'two'], [3, 'three']]);
console.log(map4.keys());//{1, 2, 3}
console.log(map4.values());  //{"one", "two", "three"}
console.log(map4.entries());  //{1 => "one", 2 => "two", 3 => "three"}
console.log([...map4.keys()]); // [1, 2, 3]
console.log([...map4.values()]);  // ['one', 'two', 'three']
console.log([...map4.entries()]);  //[[1,'one'], [2, 'two'], [3, 'three']]
console.log([...map4]);//[[1,'one'], [2, 'two'], [3, 'three']]
```



### 11.5 Map与其他数据结构的相互转换

#### 1. Map转为数组

Map转为数组最方便的方法就是使用扩展运算符(...)

```javascript
const myMap = new Map().set(true, 7).set({ name: 'ethon' }, ['abc']);
console.log([...myMap]);
//[ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

#### 2. 数组转Map

```javascript
const myMap1 = new Map([['name', 'Ethon'], ['age', 22]]);
console.log(myMap1);
//{"name" => "Ethon", "age" => 22}
```

#### 3. Map转为对象

如果Map对象的==所有键都是字符串==,它可以无损地转为对象

```javascript
function strMapToObj(strMap) {
    const obj = Object.create(null);
    for (let [k, v] of strMap) {
        obj[k] = v;
    }
    return obj;
}
const myMap2 = new Map().set('yes', true).set('no', false);
console.log(strMapToObj(myMap2));
//{yes: true, no: false}
//如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名
```

#### 4. 对象转Map

```javascript
const newObj = { 'a': 1, 'b': 2 };
console.log(Object.entries(newObj)); //[Array(2), Array(2)]
const result = new Map(Object.entries(newObj));
console.log(result);
//{"a" => 1, "b" => 2}
```



#### 5. Map转为JSON

Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

示例：

```javascript
function strMapToJson(strMap) {
    return JSON.stringify(strMapToObj(strMap));
}

let myMapp = new Map().set('yes', true).set('no', false);
strMapToJson(myMapp)
// '{"yes":true,"no":false}'
```

另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON

示例：

```javascript
function mapToArrayJson(map) {
return JSON.stringify([...map]);
}

let myMappp = new Map().set(true, 7).set({ foo: 3 }, ['abc']);
mapToArrayJson(myMappp)
// '[[true,7],[{"foo":3},["abc"]]]'
```



#### 6. JSON转为Map

JSON 转为 Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap1(jsonStr) {
    return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap1('{"yes": true, "no": false}')
Map {'yes' => true, 'no' => false}
```

有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 ==Map 转为数组 JSON 的逆操作。==

```javascript
function jsonToMap2(jsonStr) {
    return new Map(JSON.parse(jsonStr));
}

jsonToMap2('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```



### 11.6 WeakSet

`WeakSet` 即弱集合。弱集合中的值**只能是Object或者继承来自Object类型。**如果使用非对象设置值，那么会抛出 类型错误TypeError。

#### 11.6.1 WeakSet的API方法

一、`has(value)`方法：

```js
const val1 = { id: 1 },
      val2 = { id: 2 },
      val3 = { id: 3 }
const ws1 = new WeakSet([val1, val2, val3])

console.log(ws1.has(val1)) //=>true
console.log(ws1.has(val2)) //=>true
console.log(ws1.has(val3)) //=>true
```

二、`add(value)`方法：

```js
const val1 = { id: 1 },
      val2 = { id: 2 },
      val3 = { id: 3 }
const ws1 = new WeakSet([val1, val2, val3])

const val4 = { id: 4 }
const val5 = { id: 5 }
ws1.add(val4)
ws1.add(val5)
```

因为 `add(value)` 方法返回的是弱集合的实例，所以可以链式调用

```js
const val1 = { id: 1 },
      val2 = { id: 2 },
      val3 = { id: 3 }
const ws1 = new WeakSet([val1, val2, val3])

const val6 = { id: 6 }
const val7 = { id: 7 }
ws1.add(val6).add(val7)
```

三、`delete(value)` 方法：

```js
const val1 = { id: 1 },
      val2 = { id: 2 },
      val3 = { id: 3 }
const ws1 = new WeakSet([val1, val2, val3])

//这里以删除val2值为例子
// 未删除之前
console.log(ws1.has(val2)) // =>true
ws1.delete(val2)
// 删除之后
console.log(ws1.has(val2)) // =>false
```

#### 11.6.2 WeakSet的弱值

弱值的意思就是**这些对象的引用不是正式的引用，所以不会阻止垃圾回收。**

举个例子理解理解。

```js
const ws = new WeakSet()
const val1 = { id: 1 },
      val2 = { id: 2 }
ws.add(val1).add(val2)
ws.add({ id: 3 })
console.log(ws) //=> 结果是：弱集合中只有两个弱值。
```

结果图示：

![image-20211222172059568](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211222172108.png)

思考：为什么弱集合中会没有`{id:3}`这个弱值？

原因是：`{id:3}` 这个弱值在外部没有变量记录着这个对象的引用地址，换句话说就是没有变量在使用这个对象，以至于`{id:3}` 这个对象会被垃圾回收程序当作是垃圾，从而对它进行垃圾回收。然后，这个值就从 `弱集合WeakSet` 中消失。

**总结：**

>`弱集合WeakSet` 的弱值不会阻止垃圾回收，当该弱值在外部没有被引用了，那么该弱值就会从 `弱集合WeakSet` 中消失。

#### 11.6.3 WeakSet的使用场景

![image-20211222173223865](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211222173237.png)



### 11.7 WeakMap

`WeakMap` 即弱映射字典。**弱映射字典的键名只能是Object，或者继承来自Object的类型。**如果不是Object,则会抛出类型错误（TypeError）。

#### 11.7.1 WeakMap的API方法

一、`has(key)` 方法：判断弱映射字典中是否有该键值对。

```js
const key1 = { id: 1 },
      key2 = { id: 2 },
      key3 = { id: 3 }

const wm = new WeakMap([
  [key1, 'val1'],
  [key2, 'val2'],
  [key3, 'val3'],
])

console.log(wm.has(key1)) // => true
console.log(wm.has(key2)) // => true
console.log(wm.has(key3)) // => true
```

二、`get(key)` 方法：获取弱映射字典中的某个元素的键值。

```js
const key1 = { id: 1 },
      key2 = { id: 2 },
      key3 = { id: 3 }

const wm = new WeakMap([
  [key1, 'val1'],
  [key2, 'val2'],
  [key3, 'val3'],
])

console.log(wm.get(key1)) // =>'val1'
console.log(wm.get(key2)) // =>'val2'
console.log(wm.get(key3)) // =>'val3'
```

三、`set(key,value)` 方法：向弱映射字典中添加新的键值对。

==注意：key必须是Object类型。==

```js
const key1 = { id: 1 },
      key2 = { id: 2 },
      key3 = { id: 3 }

const wm = new WeakMap([
  [key1, 'val1'],
  [key2, 'val2'],
  [key3, 'val3'],
])

//向弱映射字典中添加新的键值对。
const key4 = { id: 4 }
const key5 = { id: 5 }
const key6 = { id: 6 }
wm.set(key6, 'val6')
```

四、`delete(key)` 方法：向弱映射字典中删除每个键值对。

```js
const key1 = { id: 1 },
      key2 = { id: 2 },
      key3 = { id: 3 }

const wm = new WeakMap([
  [key1, 'val1'],
  [key2, 'val2'],
  [key3, 'val3'],
])

//这里以删除key2为例子
// 未删除之前
console.log(wm.has(key2)) // =>true
wm.delete(key2)
// 删除之后
console.log(wm.has(key2)) // =>false
```

#### 11.7.2 WeakMap的弱键

弱键的意思就是**这些对象的引用不是正式的引用，所以不会阻止垃圾回收。**

==简单点说就是当外部对象的引用指向为null后，WeakMap内的该对象的映射就会消失。==

一、举个例子理解：

```js
let k1 = { id: 10, val: '辣椒' },
    k2 = { id: 11, val: '苹果' },
    k3 = { id: 12, val: '香蕉' }
const wm1 = new WeakMap([
  [k1, '辣椒我爱吃'],
  [k2, '苹果我也爱吃'],
  [k3, '香蕉我不喜欢'],
])
```

在外部如果没有变量指向该弱键的引用，那么该弱键就会被当作垃圾被垃圾回收程序进行垃圾处理。

```js
k1 = null //当改对象的引用地址指向为null时，wm1内部的该对象的映射也会消失。

console.log('第二个弱映射字典', wm1.get(k1)) // =>undefined
```

二、再举个例子：

```js
const wm1 = new WeakMap()

wm1.set({}, 'value') 
```

`set()方法` 初始化了一个新对象并将它用作一个字符串的键。因为没有指向这个对象的其他引用，所以当这行代码执行完成后，这个对象键就会被当作垃圾回收。

#### 11.7.3 WeakMap的实际应用场景

一、私有变量：

```js
//利用闭包，使得外部不能直接通过弱键直接拿到弱字典中的值，同时也真形成了私有变量。
const User = (() => {
  const wm = new WeakMap()

  class User {
    constructor(id) {
      this.isProperty = Symbol('id') //Symbol数据类型是对字符串的描述
      this.setId(id)
    }
    // 设置私有属性
    setPrivate(property, value) {
      const privateMembers = wm.get(this) || {}
      privateMembers[property] = value
      wm.set(this, privateMembers)
    }
    // 获取私有属性
    getPrivate(property) {
      return wm.get(this)[property] //先获取到对象，然后再获取相应的值
    }
    setId(id) {
      this.setPrivate(this.isProperty, id)
    }
    getId() {
      return this.getPrivate(this.isProperty)
    }
  }
  return User
})()

const user = new User(2181600)
console.log(user.getId()) // => 2181600
user.setId(2181579)
console.log(user.getId()) // => 2181579

console.log(wm) // => 报错
```

二、DOM节点元数据：

![image-20211222234015264](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211222234025.png)



## 十二、Iterator和for...of循环

### 12.1 Iterator的概念

`遍历器（Iterator）`就是这样一种机制。它是一种接口，为各种不同的数据结构提供统一的访问机制。

任何数据类型具备iterator接口,就可以完成遍历操作。

```javascript
const arr = ['盖伦', '寒冰', '提莫'];
console.log(arr);
console.log(arr[Symbol.iterator]());
```

Iterator遍历过程:

1. 创建一个指针对象,指向当前数据结构的起始位置.==遍历器对象本质上就是指针对象==.
2. 第一次调用指针对象的next()方法,可以使指针指向数据结构的第一个成员
3. 第二次调用指针对象的next()方法,指向数据结构的第二个成员
4. 不断调用指针对象的next()方法,直到它指向数据结构的结束位置.



每一次调用next方法.都会返回数据结构当前成员的信息.具体来说,就是返回一个包含`value`和`done`两个属性的对象.

其中，`value属性是当前成员的值`，`done属性是一个布尔值，表示遍历是否结束`。

```javascript
const arr1 = ['盖伦', '小黑', '小白', '剑圣'];
const iterator = arr1[Symbol.iterator]();
//[Symbol.iterator]()是遍历器生成函数，会返回一个遍历器对象，后面会再提到

console.log(iterator.next());   //{value: "盖伦", done: false}
console.log(iterator.next());   //{value: "小黑", done: false}
console.log(iterator.next());   //{value: "小白", done: false}
console.log(iterator.next());   //{value: "剑圣", done: false}
console.log(iterator.next());   //{value: undefined, done: true}
```



### 12.2 默认的Iterator接口

具有Iterator接口的数据结构,可以使用`for...of`进行遍历.

原生具备Iterator接口的数据结构有:

- Array
- String
- Set
- Map
- 函数的arguments对象
- NodeList对象

```javascript
let arr = ['a', 'b', 'c'];
let iter = arr[Symbol.iterator]();
iter.next() // { value: 'a', done: false }
iter.next() // { value: 'b', done: false }
iter.next() // { value: 'c', done: false }
iter.next() // { value: undefined, done: true }
```



ES6规定,默认的Iterator接口部署在**数据结构的Symbol.iterator属性**,也就是说一个数据结构具有Symbol.iterator属性就是"可遍历的"

==Symbol.iterator本身是一个函数,执行这个函数会返回一个遍历器对象。==

```javascript
const obj = {
    [Symbol.iterator]: function () {
        return {
            next: function () {
                return {
                    value: 1,
                    done: true
                };
            }
        };
    }
};
```



### 12.3 调用Iterator接口的场合

#### #解构赋值

对数组和Set结构进行解构赋值时,会默认调用`Symbol.iterator方法`.

示例：

```javascript
let set = new Set().add('a').add('b').add('c');
console.log(set);  
//结果：{"a", "b", "c"}

let [x, y] = set
console.log([x, y]);
//结果： ["a", "b"]

let [first, ...end] = set;
console.log(first, end);
//first='a';  end='['b','c']'
```

#### #扩展运算符

==扩展运算符(...)也会调用默认的Iterator接口==

```javascript
var str = 'hello';
console.log([...str]);   //["h", "e", "l", "l", "o"]
console.log('a', ...str, 'c')
//a h e l l o c
//上面代码的扩展运算符内部就调用 Iterator 接口。
```

#### #yield*

yield*后面跟的是一个可遍历的结构,它会调用该结构的遍历器接口.

```javascript
let generator = function* () {
    yield 1;
    yield* [2, 3, 4];
    yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```



### 12.4 字符串的Iterator接口

字符串是一个类似数组的对象，也原生具有 Iterator 接口

```javascript
var someString = "hi";
typeof someString[Symbol.iterator]
// "function"

var iterator = someString[Symbol.iterator]();

iterator.next()  // { value: "h", done: false }
iterator.next()  // { value: "i", done: false }
iterator.next()  // { value: undefined, done: true }
```



### 12.5 遍历器接口与生成器函数

Symbol.iterator()方法的最简单实现，还是使用下一章要介绍的 Generator 函数。

```javascript
let myIterable = {
    [Symbol.iterator]: function* () {
        yield 1;
        yield 2;
        yield 3;
    }
};
[...myIterable] // [1, 2, 3]

// 或者采用下面的简洁写法

let obj = {
    *[Symbol.iterator]() {
        yield 'hello';
        yield 'world';
    }
};

for (let x of obj) {
    console.log(x);
}
// "hello"
// "world"

//上面代码中，Symbol.iterator()方法几乎不用部署任何代码，只要用 yield 命令给出每一步的返回值即可
```



### 12.6 对象使用for...of遍历

对于普通的对象，`for...of结构`不能直接使用，会报错，必须部署了 Iterator 接口后才能使用

```javascript
let es6 = {
    edition: 6,
    committee: "TC39",
    standard: "ECMA-262"
};

for (let e of es6) {
    console.log(e);
}
//报错：TypeError: es6[Symbol.iterator] is not a function
```

解决方法:使用`Object.keys方法`将对象的键名生成一个数组,然后遍历这个数组.

```javascript
let es6 = {
    edition: 6,
    committee: "TC39",
    standard: "ECMA-262"
};
for (let key of Object.keys(es6)) {
    console.log(key + ': ' + es6[key]);
}
//edition: 6
//committee: TC39
//standard: ECMA-262
```

另一个方法是使用Generator函数将对象重新包装一下.

```javascript
let es6 = {
    edition: 6,
    committee: "TC39",
    standard: "ECMA-262"
};
function* entries(obj) {
    for (let key of Object.keys(obj)) {
        yield [key, obj[key]];
    }
}
for (let [key, value] of entries(es6)) {
    console.log(key, '->', value);
}
//edition -> 6
//committee -> TC39
//standard -> ECMA-262
```



## 十三、Generator函数的语法

### 13.1 生成器函数简介

Generator函数是一个状态机,封装了很多内部状态.

**执行Generator函数会返回一个迭代器对象**,说明生成器函数除了状态机,还是一个迭代器生成函数.返回的遍历器对象可以依次遍历生成器内部的每一个状态.



它和普通函数的区别：

1. function关键字与函数名之间有一个*号.
2. 函数体内使用yield表达式,表示不同的内部状态.



==**生成器函数是分段执行的,遇到yield表达式就会暂停执行,而next()方法可以恢复执行.**==



生成器函数调用和普通函数调用一样,在函数名后面加一对圆括号.不同的是,生成器函数返回的是一个`指向内部状态的指针对象.`

==遍历器对象可以调用next()方法,使得指针指向下一个状态.==

示例：

```javascript
function* func(args) {
    yield 'hello';
    yield 'world';
    return 'ending';
}
const res = func();
console.log(res);
//结果：func {<suspended>}

//使用遍历器对象的next()方法
console.log(res.next()); //{value: "hello", done: false}
console.log(res.next()); //{value: "world", done: false}
console.log(res.next()); //{value: "ending", done: true}
```



#### #yield和return的区别:

yield表达式和return语句的区别:

1. return语句不具备位置记忆功能,而yield表达式具有,能知道下次执行的位置从哪开始.
2. 可以执行多个yield表达式,但return语句只能执行一次.

#### #Generator函数可以不用yield表达式

Generator函数可以不用yield表达式，这时就变成了一个单纯的暂缓执行函数。

示例：

```javascript
function* f() {
    console.log('你执行了!');
}
var result = f();  //如果是普通函数就会执行，把结果返回给变量result。
setTimeout(function () {
    result.next();
}, 2000)
//上面代码中，函数f如果是普通函数，在为变量generator赋值时就会执行。
//两秒后通过next()执行了f()函数.
```



#### #yield表达式如果用在另一个表达式之中,必须放在圆括号里面.

yield表达式如果用在另一个表达式之中,必须放在圆括号里面.

示例：

```javascript
function* demo() {
    console.log('Hello' + yield); //报错 SyntaxError
    console.log('Hello' + yield 123); //报错 SyntaxError

    console.log('Hello' + (yield)); // OK
    console.log('Hello' + (yield 123)); // OK
}
```

#### #yield表达式用作函数参数或放在赋值表达式的右边,可以不加括号

示例：

```javascript
function* demo() {
    foo(yield 'a', yield 'b'); // OK
    let input = yield; // OK
}
```



### 13.2 与Iterator接口的关系

任意一个对象的`Symbol.iterator方法`,就是一个**遍历器生成函数**,调用该函数,会返回一个该对象的遍历器对象.

==**Generator(生成器)函数就是遍历器生成函数,**==因此可以把生成器函数赋值给对象的Symbol.iterator属性,从而使得该对象具有iterator接口.

示例：

```javascript
let myIterator = {}; //创建新对象
myIterator[Symbol.iterator] = function* f() {
    yield 1;
    yield 2;
    yield 3;
}
console.log([...myIterator]);  //[1, 2, 3]
```

上面代码中，`Generator 函数`赋值给`Symbol.iterator属性`，从而使得myIterable对象具有了 Iterator 接口，可以被`扩展运算符`遍历了。



> Generator函数执行后,返回一个遍历器对象.该对象本身也具有Symbol.iterator属性,执行后返回自身对象.

示例：

```javascript
function* func() {
    yield 1;
}
var g = func();
console.log(g);
console.log(g[Symbol.iterator]() === g);  //true
```



### 13.3 next方法的参数

每一个yield表达式代表一个不同的内部状态,它本身没有返回值.next方法可以带一个参数,该参数会被当作上一个yield表达式的值.

==next方法在第一次调用的时候,参数是无效的.==

示例：

```javascript
function* fnc() {
    for (let i = 0; true; i++) {
      //如果next()方法不带参数，resetd
        var reset = yield i;
        if (reset) { i = -1 }
    }
};
var fnc = fnc();
console.log(fnc.next());  //{value: 0, done: false}
console.log(fnc.next());  //{value: 1, done: false}
console.log(fnc.next());  //{value: 2, done: false}

console.log(fnc.next(true)); //{value: 0, done: false}
//next方法的参数会被当作上一个yield表达式的值。
```



指针对象的next()方法参数的示例2:加深对next()方法参数的理解

```javascript
function* foo(x) {
    var y = 2 * (yield (x + 1));
    var z = yield (y / 3);
    return (x + y + z);
}
const a = foo(5);
console.log(a.next());  //{value: 6, done: false}
console.log(a.next());  //{value: NaN, done: false}
console.log(a.next());  //{value: NaN, done: true}
// 上面代码中，第二次运行next方法的时候不带参数，导致 y 的值等于2 * undefined（即NaN），除以 3 以后还是NaN，因此返回对象的value属性也等于NaN。第三次运行Next方法的时候不带参数，所以z等于undefined，返回对象的value属性等于5 + NaN + undefined，即NaN。

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
// 如果向next方法提供参数，返回结果就完全不一样了。上面代码第一次调用b的next方法时，返回x+1的值6；第二次调用next方法，将上一次yield表达式的值设为12，因此y等于24，返回y / 3的值8；第三次调用next方法，将上一次yield表达式的值设为13，因此z等于13，这时x等于5，y等于24，所以return语句的值等于42。


```



### 13.4 for..of循环

`for...of循环`可以自动遍历生成器函数运行时生成的遍历器对象,且此时不再需要调用next()方法

```javascript
function* func() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
    return 6;
}
for (let i of func()) {
    console.log(i);
}
// 1 2 3 4 5
```



**原生的JavaScript对象没有遍历器接口,无法使用for...of循环,通过Generator函数为它加上这个接口,就可以使用了.**

```javascript
function* objectEntries(obj) {
    let objKeys = Reflect.ownKeys(obj);
    for (let keyItem of objKeys) {
        yield [keyItem, obj[keyItem]];
    }
};
let obj1 = { name: 'Ethon', age: 100 };
for (let [key, value] of objectEntries(obj1)) {
    console.log(`${key}:${value}`);
}
//name:Ethon
//age:100
```



及那个生成器函数加到对象的Symbol.iterator属性上面,使得该对象能使用for...of遍历

```javascript
function* objectEntries1() {
    let objKeys = Object.keys(this);
    for (let keyItem of objKeys) {
        yield [keyItem, this[keyItem]];
    }
};
let obj2 = { name: 'ROSE', age: 20 };
obj2[Symbol.iterator] = objectEntries1;
for (let [key, value] of obj2) {
    console.log(`${key}:${value}`);
}
//name:ROSE
//age:20
```



> `for..of循环` 、`扩展运算符(...)` 、`解构赋值`和`Array.from方法`内部调用的都是遍历器接口,这说明他们可以将生成器函数返回的遍历器对象作为参数.

示例：

```javascript
function* numbers() {
    yield 1
    yield 2
    return 3
    yield 4
}

// 扩展运算符
[...numbers()] // [1, 2]

// Array.from 方法
Array.from(numbers()) // [1, 2]

// 解构赋值
let [x, y] = numbers();
x // 1
y // 2

// for...of 循环
for (let n of numbers()) {
    console.log(n)
}
// 1
// 2
```



### 13.5 生成器原型对象的throw方法

生成器函数返回的遍历器对象,都有一个throw方法,可以在函数体外抛出错误,然后在生成器函数体内捕获.

```javascript
function* g() {
    try {
        yield 2;
    } catch (e) {
        console.log('内部捕获', e);
    }
};
var i = g();
console.log(i.next());  //{value: 2, done: false}
try {
    i.throw('a');
    i.throw('b');
} catch (error) {
    console.log('外部捕获', error);
}
// 内部捕获 a
// 外部捕获 b
```

上面遍历器对象i连续抛出两个错误,第一个错误能被生成器函数体内的catch语句捕获,但catch捕获后就不再捕获错误,所以第二个错误抛出函数体外面,然后被函数体外的catch语句捕获到.



> throw方法可以接受一个参数，该参数会被catch语句接收，==建议抛出Error对象的实例。==

示例：

```javascript
function* func4() {
    try {
        yield 10;
    } catch (error) {
        console.log(error);
    };
};
var fun4 = func4();
console.log(fun4.next());  //{value: 10, done: false}
fun4.throw(new Error('出错了！'));  //Error: 出错了！
```



遍历器对象的throw方法和全局的throw命令不一样，**全局的throw命令只能被外部的catch语句捕获到错误**。

```javascript
function* func5() {
    try {
        yield 20;
    } catch (error) {
        console.log('内部捕获', e);
    }
};
var fn5 = func5();
console.log(fn5.next());
//{value: 20, done: false}
try {
    throw new Error('a');
} catch (error) {
    console.log('外部捕获', error);
}
//外部捕获 Error: a
//说明全局throw命令只能被外部的catch语句捕获到错误。
```



> throw方法抛出错误要被内部捕获，==前提是必须至少要执行一次next方法==.

示例：

```javascript
function* gen() {
    try {
        yield 1;
    } catch (e) {
        console.log(e);
    };
}
//生成器函数返回一个遍历器对象
var g = gen();
g.throw('错误了');
//报错：Uncaught 错误了

//上面的代码中，g.throw('错误了')执行时，next方法一次都没有执行过。这时，抛出的错误不会被内部捕获，而是直接在外部抛出，导致程序错误。
```



如果生成器函数内部和外部都没有部署try...catch代码块，那么throw方法抛出错误后程序将报错，直接终端执行。

```javascript
function* fnc3() {
    yield console.log('hello');
    yield console.log('world');
}
var f3 = fnc3();
f3.next();  //hello
f3.throw(); 
//报错：Uncaught undefined
```



throw方法被捕获以后，会附带执行下一条yield表达式。也就是说，会附带执行一次next方法。

```javascript
function* func() {
    try {
        yield console.log('内部执行了1');
    } catch (e) {
        console.log(e);
    }
    yield console.log('内部执行了2');
    yield console.log('内部执行了3');
}
const fnc = func();
fnc.next();  //内部执行了1
fnc.throw();
//函数体内部捕获错误  //undefined
//throw方法抛出的错误被捕获后，会附带执行下一条yield表达式.
//内部执行了2
fnc.next();   //内部执行了3
```



生成器函数体内部署了try...catch代码块，遍历器对象的throw方法抛出错误，不会影响下一次遍历。

```javascript
function* func1() {
    try {
        yield 1;
        yield 2;
    } catch (error) {
        console.log(error);
    }
    yield 3;
    yield 4;
}
const fn1 = func1();
console.log(fn1.next());  //{value: 1, done: false}
console.log(fn1.next());  //{value: 2, done: false}
console.log(fn1.throw('程序错误了')); //结果：程序错误了
//{value: 3, done: false}
//前面也说了throw方法被捕获后，会附带执行下一条yield表达式，也就是会附带执行一次next方法

//throw方法抛出错误后不影响下一此遍历。
console.log(fn1.next());  /{value: 4, done: false}
```



生成器函数体内抛出错误，可以在函数体外的catch捕获，相反，生成器函数体外抛出错误，可以在函数体内的catch捕获。

```javascript
function* foo() {
    var x = yield 3;
    var y = x.toUpperCase();
    yield y;
}
var it = foo();
console.log(it.next());//{value: 3, done: false}
try {
    console.log(it.next(42));
} catch (error) {
    console.log(error);
    //上面try语句中尝试执行it.next(42)，但是发生错误了
    // TypeError: x.toUpperCase is not a function
}
//上面代码中，第二个next方法向函数体内传入一个参数 42，数值是没有toUpperCase方法的，所以会抛出一个 TypeError 错误，被函数体外的catch捕获
```



==注意：一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用next方法，将返回一个value属性等于undefined、done属性等于true的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束了。==



### 13.6 生成器原型对象的return方法

Generator 函数返回的遍历器对象，还有一个return()方法，**可以返回给定的值**，并且终结遍历 Generator 函数

```javascript
function* gen() {
    yield 1;
    yield 2;
    yield 3;
}

var g = gen();
g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```

如果return()方法没有参数时调用，则返回值的value属性值为undefined。



### 13.7 yield*表达式

yield*表达式：如果yield表达后面跟的时一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象。

```javascript
function* nameFun() {
    yield 'Ethon';
    yield 'Rose';
    yield 'Tom';
}
function* ageFun() {
    yield 22;
    yield* nameFun();
    yield 30;
}
for (let item of ageFun()) {
    console.log(item);
    //22
    //Ethon
    //Rose
    //Tom
    //30
}
```



yield*表达式后面的生成器函数(没有return语句时)，等同于在生成器函数内部，部署一个for...of循环。

```javascript
function* concat(iter1, iter2) {
    yield* iter1;
    yield* iter2;
}
// 等同于
function* concat(iter1, iter2) {
    for (var value of iter1) {
        yield value;
    }
    for (var value of iter2) {
        yield value;
    }
}
```



如果yield*后面跟着一个数组，由于数组原生支持遍历器，因此就会遍历数组成员。

```javascript
function* gen() {
    yield* ["a", "b", "c"];
}
console.log(gen().next());
//{value: "a", done: false}
```

上面代码中，yield命令后面如果不加星号，返回的是整个数组，**加了星号就表示返回的是数组的遍历器对象**。



> 任何数据结构具有iterator接口，都可以被yield*表达式遍历。

示例：

```javascript
let read = (function* () {
    yield 'hello';
    yield* 'hello';
})()
console.log(read.next().value); //hello
console.log(read.next().value); //h
console.log(read.next().value); //e
```



如果被代理的生成器函数有return语句，那么就可以向代理它的生成器函数返回数据。

```javascript
function* foo() {
    yield 2;
    yield 3;
    return 'foo';
}
function* bar() {
    yield 1;
    var fo = yield* foo();
    console.log("fo:" + fo);
    yield 4;
}
var it = bar();
console.log(it.next());  //{value: 1, done: false}
console.log(it.next());  //{value: 2, done: false}
console.log(it.next());  //{value: 3, done: false}
console.log(it.next());
//fo:foo
//{value: 4, done: false}
console.log(it.next());  //{value: undefined, done: true}
//上面代码在第四次调用next方法的时候，屏幕上会有输出，这是因为函数foo的return语句，向函数bar提供了返回值。
```



### 13.8 作为对象属性的Generator函数

如果一个对象的属性是 Generator 函数，可以简写成下面的形式

```javascript
let obj = {
    *myGeneratorMethod() {
        yield 1;
    }
};
//相当于
let obj = {
    myGeneratorMethod: function* () {
        yield 1;
    }
}
```



### 13.9 生成器函数的this

Generator 函数总是返回一个遍历器，ES6 规定这个遍历器是 Generator 函数的实例，也继承了 Generator 函数的prototype对象上的方法。

```javascript
function* g() { }

g.prototype.hello = function () {
    return 'hi!';
};

let obj = g();

obj instanceof g // true
obj.hello() // 'hi!'
```



> ==生成器函数返回的总是遍历器对象，而不是this对象。==

示例：

```javascript
function* fn() {
    this.a = 11;
}
let obj1 = fn();
console.log(obj1.next());
//{value: undefined, done: true}
console.log(obj1.a);
//undefined.
```

生成器函数不是构成函数，不能跟new命令一起使用

```javascript
function* Fn() {
    yield 1;
    yield 2;
}
new Fn()  //报错
```



想办法让生成器函数返回一个正常的对象实例，既可以用next方法，又可以获得正常的this。

```javascript
function* gen() {
    this.a = 1;
    yield this.b = 2;
    yield this.c = 3;
}

function F() {
    return gen.call(gen.prototype);
}

var f = new F();

f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}

f.a // 1
f.b // 2
f.c // 3
```



### 13.10 生成器函数的应用

#### #异步操作的同步化表达

Generator 函数的暂停执行的效果，意味着可以把异步操作写在yield表达式里面，等到调用next方法时再往后执行

==生成器函数的一个重要实际意义就是用来处理异步操作，改写回调函数。==

```javascript
function* loadUI() {
    show();
    yield loadData();
    end();
}
const loader = loadUI();
loader.next();
function show() {
    console.log('页面开始加载....');
}
function loadData() {
    setTimeout(() => {
        console.log('数据加载完成');
        loader.next();
    }, 1000);
}
function end() {
    console.log('页面结束加载....')
}
```



#### #控制流程管理





#### #部署Iterator接口

生成器函数可以部署Iterator接口

```javascript
let myIterator = {}; //创建新对象
myIterator[Symbol.iterator] = function* f() {
    yield 1;
    yield 2;
    yield 3;
}
console.log([...myIterator]);  //[1, 2, 3]
```

