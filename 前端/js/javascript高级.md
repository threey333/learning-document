# javascript高级

## 一、基础总结

### 1.1  数据类型

==JavaScript一共有 **8** 种数据类型，其中 **7** 种原始数据类型(基本数据类型)，**1** 种引用值类型==

```javascript
原始数据类型：undefined、null、number、String、boolean、symbol (ES6)、bigint (ES10)

引用值类型:Object。
```

#### 1.1.1 用typeof符判断

- 可以判断：undefined/数值/字符串/布尔值/function.
- 不可以判断：null与object、object与array。

示例：可以判断的

![image-20210430113000766](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430113000766.png)

示例：不可以判断的

![image-20210430113020327](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430113020327.png)



#### 1.1.2 数据类型的区别

>undefined与null的区别？

1. undefined代表变量声明了但没有赋值
2. null是定义了并且赋值了，只是值为null（空对象的引用。）

>什么时候给变量赋值为null

1. 开始前赋值为null，表明将要赋值为对象
2. 结束前赋值为null，表明将对象成为垃圾对象，然后垃圾回收期回收

>严格区别变量类型与数据类型？

1. 变量类型即`变量在内存中值的类型`

   ​	如果是基本类型，内存中保存的就是基本类型的值

   ​	如果是引用类型，内存中保存的就是引用类型的地址

2. 数据类型：即基本类型和引用类型。



### 1.2 数据、变量、内存

#### 1.2.1 什么是数据

- 存储在内存中代表特定信息的东西，本质上是010101....
- 数据的特点：可传递，可运算
- 一切皆数据
- 内存中所有操作的目标：数据
  - 算术运算
  - 逻辑运算
  - 赋值
  - 运行函数

#### 1.2.2 什么是内存？

- 内存条通电后产生的可存储数据的空间(临时的)
- 内存产生和死亡：内存条（电路板）== > 通电== >产生内存空间 == >存储数据 ==> 处理数据 == >断电 ==> 内存空间和数据都消失
- 一块小内存的2个数据
  - 内部存储的数据
  - 地址值
- 内存分类
  - 栈：全局变量/局部变量
  - 堆：对象

#### 1.2.3 什么是变量？

- 可变化的量，由变量名和变量值组成
- 每个变量都对应一个小内存，变量名用来查找对应的内存，变量值是内存中保存的数据

#### 1.2.4 内存，数据，变量三者之间的关系

- 内存用来存储数据的空间
- 变量是内存的标识



### 1.3. 对象

##### 什么是对象

对象就是多个数据的封装体，用来保存多个数据的容器，一个对象代表现实中的一个事物。

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构）

##### 为什么要用对象

使用对象是为了方便统一管理多个数据

##### 对象的组成

属性：`属性名(字符串)`和`属性值(任意类型)`组成

方法：一种特别的属性（属性值是函数）

##### 如何访问对象内部数据？

​	`对象.属性名`：编码简单，有时不能用

​	`对象['属性名']`：编码麻烦，能通用  ==>使用[]这种形式去操作属性，**更加灵活**。

>什么时候必须使用[‘属性名']的方式

1. 属性名含特殊字符：-、空格等等。
2. 属性名是变量。

示例：使用对象['属性名']的方式

```javascript
var p = {
    name: "xiaoyang",
    fangfa: function () {
        console.log('方法');
    }
};
var i = 'name';
var yyy = 'myAge';
var value = 18;
// p.yyy = value;       //{yyy: 18}
p[yyy] = value;   //{myAge: 18}
console.log(p);
console.log("使用对象['属性名']的结果：" + p.i);   //使用对象['属性名']的结果：undefined
console.log("使用对象['属性名']的结果：" + p[i]);  //使用对象['属性名']的结果：xiaoyang
```



##### 对象中容易搞混淆的地方

```javascript
// 第一种
var obj = new Object();
obj.name = 'yyy';
console.log(obj.name);  //yyy
var obj1 = obj;
console.log(obj1);  //{name: "yyy"}
obj1.name = 'zyx';
console.log(obj);  //{name: "zyx"}

//第二种
var a = { age: 12 };
var b = a;
//a和b存放的都是堆内存中对象实例的地址，指向的是同一个内存空间。
a = { name: "yyy", age: 20 };
//给a重新赋值一个实例对象的地址，指向新的一个内存空间，以至于后面a.age变成了20
console.log(b.age);  //12
console.log(a.age);  //20
//还有一种情况
fn2(a);
function fn2(obj) {
    obj = { age: 15 };
    console.log("函数内部" + obj + "的情况：" + obj.age);   //15
    //这里obj存放的是obj实例对象的地址，不会影响a的存放的地址。
}
console.log(a); //{name: "yyy", age: 20}
```



### 1.4 函数

#### 1.4.1 定义函数的三种方式

方式1：函数声明方式

![image-20210426224049076](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426224049076.png)

方式2：函数表达式方式

![image-20210426224104990](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426224104990.png)

方式3：构造函数Function方式

![image-20210426224209810](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426224209810.png)

==利用构造函数Function方式的案例：==【代码实时预览效果.html】

[实例](file:///D:/%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/%E5%89%8D%E7%AB%AF/JS%E8%B5%84%E6%96%99/js/%E6%A1%88%E4%BE%8B/js%E9%AB%98%E7%BA%A7%E6%A1%88%E4%BE%8B/%E5%9F%BA%E7%A1%80%E6%80%BB%E7%BB%93/02-%E5%88%A9%E7%94%A8%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0Functiond%E7%9A%84%E6%A1%88%E4%BE%8B.html)



#### 1.4.2 try-catch

try尝试 try中的代码始终都要执行

catch 捕获:如果try中的代码存在错误，会被catch捕获到，意味着执行catch里面的代码

语法：

```javascript
try {
    ...
} catch (error) {
	
}
```

用法：

- 把可能错误的代码放在try中，尝试去执行
- 如果有错误，catch会捕获到，但不会影响后面的代码执行。

![image-20210427182802819](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427182802819.png)



#### 1.4.3 this指向

==this的指向在函数定义的时候是确定不了的==，只有函数执行的时候才能确定this到底指向谁，一般情况下this的最终指向的是那个调用它的对象。

**JS只有全局作用域和函数局部作用域，不存在想当然的对象作用域**.

示例：

```javascript
var a = 456;
var o = {
  a: 123,
  b: this.a,
  scope: this,
  fn: () => { console.log(this); }    //window
};
console.log(o.b);   //456
console.log(o.scope);  //window
console.log(o.fn);    //undefined

//为什么这样呢？因为对象括号里面没有作用域这一说法，所以this指向都是window，但函数有而箭头函数又没有。
```



> 1.函数调用模式

全局作用域或者普通函数中this指向全局对象window

```javascript
console.log(this);   //window

function fn() {
  console.log(this);   //window

}
window.fn();    
window.setTimeout(function() {
  console.log(this);    //window

}, 1000);
```



> 2.方法调用模式

this指向调用方法的对象（谁调用this指向谁）

==通过. 或者是 [] 都是属于方法调用模式==

```javascript
// 2. 方法调用中 谁调用this指向谁
var o = {
    sayHi: function() {
        console.log(this); // this指向的是 o 这个对象

    }
}
o.sayHi();

// 对象['属性名']
o['sayHi']();
```



> 3.构造函数中this指向构造函数的实例

```javascript
//构造函数
function Person(uname, age, socre) {
  this.name = uname;
  this.ages = age;
  this.socres = socre;    
  this.fangfa = function (a) {
    console.log(a);     //方法
  }
  this.s = function () {
    alert('gun');       //方法
  }
  console.log(this);    //指向person
}
var person = new Person("yangyeyong", 22, 100);   //对象实例化
```



>上下文调用模式

上下文调用模式也叫方法借用模式，用来修改this的指向。

方法有：`call方法`、`apply方法`、`bind方法`

1.call方法可以调用一个函数，并且可以指定这个函数的this指向

示例如下：

```javascript
//1.call方法可以调用一个函数，并且可以指定这个函数的this指向
function fn1(a, b) {
    console.log(this);
    console.log(a + b);
}
fn1(10, 20);
//this:window
//30

//2.call的第一个参数指向的是this。其余的参数是和函数的参数一致
fn1.call([10, 20], 10, 20);
//this:[10,20]
//30
// 3.
```

![image-20210604124403075](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210604124403075.png)



用途：**伪数组借用数组的方法**

```javascript
伪数组与数组

1. 伪数组其实就是一个对象，但是跟数组一样，伪数组也会有length属性，也有0,1,2,3等属性。
2. 伪数组并没有数组的方法，不能使用push/pop等方法
3. 伪数组可以跟数组一样进行遍历，通过下标操作。
4. 常见的伪数组：arguments、document.getElementsByTagName的返回值、ducument.querySelectorAll的返回值、JQuery对象。

var arrF = {
    0:"嘻嘻",
    1:"哈哈",
    2:"呵呵",
    length:3
}
Array.prototype.push.call(arrF, "哼");
Array.prototype.join.call(arrF, "哈");

//将伪数组转换成真数组
var arr=Array.prototype.slice.call(arrF);
```



2.apply()方法的作用和 call()方法类似，只有一个区别，就是apply()方法接受的是**一个包含多个参数的数组**。而`call()`方法接受的是**若干个参数的列表**

**call和apply的使用场景：**

- 如果参数比较少，使用call会更加简洁
- 如果参数存放在数组中，使用apply会更方便

示例：

```javascript
function fn2(a, b) {
  console.log(this);  //{name: "yyy"}
  console.log(a + b)  //4000
}
fn2.apply({ name: "yyy" }, [1000, 3000]);  
```

![image-20210427221434103](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427221434103.png)

apply的使用场景：

![image-20210427222136237](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427222136237.png)



3.bind()方法创建一个新的函数, 可以绑定新的函数的`this指向`

示例如下：

```javascript
//返回值：新的函数
//参数：新函数的this指向，当绑定了新函数的this指向后，无论使用何种调用模式，this都不会改变。
var newFn = fn.bind(window);
```

示例：

![image-20210427224020675](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427224020675.png)



> 5.箭头函数中的this

箭头函数没有自己的this指向，其会继承离得最近的父函数上下文环境的this指向。

示例：

```javascript
obj = {
  a: 123,
  c: ()=>{
    return this.a
  }
};
console.log(obj.c()); // undefined

```

​		这里解释为什么undefined，因为箭头函数没有this指向，它只会继承父级作用域，而obj对象也没有作用域，所以箭头函数继承了window全局作用域。而window全局作用域下没有变量a,所以undefined。



#### 1.4.4 再深入理解箭头函数的this

```js
var obj = {
  birth: 1990,
  getAge: function () {
    var b = this.birth; // 1990
    var fn = () => new Date().getFullYear() - this.birth; // this指向obj对象
    return fn();
  }
};
obj.getAge(); // 25
```

这里箭头函数中的this为什么不是指向windw？原因就是上面总结的那句话--------==this会继承父级作用域==。

而函数是有自己的作用域的，也就产生局部作用域。



==总结：==

**任何一个函数都有属于自己的this指向**

**this的指向只看`函数调用模式`，不看函数在哪调用的**

1. 以函数调用模式时，this是window
2. 以方法调用模式时，this就是调用方法的对象
3. 以构造函数的形式调用时，this就是新创建的对象
4. 箭头函数没有自己的this，仍然相当于一个全局的对象，它会**继承父级作用域.**

几种特殊的this指向

- 定时器中的this指向了window，因为定时器的function最终是由window来调用
- 事件中的this指向的是当前元素，在事件触发的时候，浏览器让当前元素调用function
- addEventListener的中的事件函数默认是作为调用addEventListener对象的方法



##### 4.5 回调函数

1.什么函数才是回调函数？

​		你定义的函数没有调用但最终它执行了。

2.常见的回调函数

​		绑定dom事件回调函数、定时器回调函数、ajax请求回调函数、生命周期回调函数



##### 4.6 立即执行函数	(在第二章的沙箱模式再提及)

立即执行函数作用：

1. 隐藏实现
2. 不会污染外部（全局）命名空间
3. 用它来编码JS模块



## 二、函数高级

### 1.沙箱模式(立即执行函数)

==自调用函数也叫做沙箱模式==:即创建一个函数的同时,立调用了这个函数。

> 写法

第一种：(function(){}())

![image-20210426170608820](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426170608820.png)

第二种：(function(){})()

![image-20210426170819062](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426170819062.png)

>作用

1.独立创建了一个作用域，里面所有的变量都是局部变量，==不会污染外部（全局）命名空间==

2.隐藏实现。

3.制作js模块。

>沙箱模式通常的用法

1.在沙箱模式中，需要暴露什么,就给window对象添加属性。

2.一个模块一个js文件（`后面模块化思想的来源）,而在一个js文件中==通常也只会暴露一个核心的功能。==（目的是为了减少全局污染）

示例：

![image-20210426171943453](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210426171943453.png)

### 2. 严格模式

语法：”use strict“

使用严格模式的建议：不要再全局中开启严格模式。

​		因为在页面中会引用第三方的js文件，如果这些js文件的代码没有严格遵循严格模式，是无法在我们的严格模式下运行，会直接报错的，代码无法实现功能。

​		==严格模式一般都是搭配沙箱模式一起使用==，在沙箱内开启严格模式，只会在沙箱内起到效果。

![image-20210427212006197](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427212006197.png)

==注意==：严格模式"use strict"；这句话必须写在**当前作用域的最顶部**



### 3. 原型与原型链

##### 原型（prototype）       

==在javascript基础也有原型的笔记，两者结合看==

> **函数的prototype属性(图)**

创建的每一个函数，**解析器都会向函数中添加一个属性`prototype`**

每个函数都有一个prototype属性,它默认指向一个Object空对象(即称为：原型对象) 

原型对象中有一个属性constructor,它指向`函数对象 `

![image-20210411225545380](D:\Typora记录文档\img\image-20210411225545380.png)那怎么确定它是指向函数对象？

利用`===运算符`判断。例如

```javascript
function Fn() {

}
Fn.prototype.test = function () {
  console.log('原型中的方法：test');
}
console.log(Fn.prototype.constructor === Fn);  //true
```

**理解：（prototype属性的值是一个原型对象的地址）**

#### 给原型对象添加属性(一般都是方法)

作用：函数的所有实例对象自动拥有原型中的属性(方法)

```javascript
function Fn() {

}
Fn.prototype.test = function () {
  console.log('原型中的方法：test');
}
console.log(Fn.prototype.constructor === Fn);  //true
console.log(Fn.prototype)   //test: ƒ ()  constructor: ƒ fn()   __proto__: Object

//当函数为构造函数时
var fn = new Fn();
console.log(fn.__proto__);  //test: ƒ ()  constructor: ƒ fn()   __proto__: Object
console.log(fn.test());   //原型中的方法：test
```



##### 显式原型和隐式原型

1.每个函数function都有一个`prototype,即显式原型属性`，默认指向一个空的Object对象(即原型对象)

==空Object对象即没有我们自己定义的数据。==

```javascript
function Fn() {   	//内部语句：this.prototype={}

}
console.log(Fn.prototype);  //{constructor: ƒ}
```

2.每个实例对象都有一个`__proto__，即称为隐式原型`。

```javascript
var fn = new Fn();      //这里内部语句是：this.__proto__=Fn.prototype
onsole.log(fn.__proto__);  //{constructor: ƒ}
```

3.对象的隐式原型等于构造函数的显式原型:

```javascript
console.log(Fn.prototype === fn.__proto__);  //true
```

用图说明原型中的内存情况：![image-20210412151234170](D:\Typora记录文档\img\image-20210412151234170.png)

==总结：==1.函数的prototype属性：在定义函数时自动添加的，默认值是一个空object对象。

​			2.对象的`__proto__`属性：创建对象时自动添加的，默认值为构造函数的prototype属性值，也就是原型对象的地址。

​			**3.程序员能直接操作显式原型，但不能直接操作隐式原型（ES6之前）**



##### 原型链

> #### 1.原型链（图解如下）

![image-20210412161237676](D:\Typora记录文档\img\image-20210412161237676.png)

访问一个对象的属性时：

​			*现在自身属性中查找，找到则返回

​			*如果没有，再沿着`__proto__`这条链向上查找，找到返回。

​			*如果最终没找到，返回undefined。（说明没有这个属性）

别名：隐式原型链

作用：查找对象的属性(方法)

```javascript
//网页中就有一个对象object。
console.log(Object.prototype.prototype); 	//undefined,说明没有这个属性
console.log(Object.prototype.__proto__); 	//null，说明原型到尽头,该原型对象是组先
```

>#### 2.构造函数/原型/实例对象的关系	（图解如下）

```javascript
var o1=new Object();
var o2={};
console.log(o1.__proto__===Object.prototype);  //true;
//说明实例对象的隐式原型等于构造函数的显示原型。
```

![image-20210412171418473](D:\Typora记录文档\img\image-20210412171418473.png)



>#### 3.构造函数/原型/实例对象的关系2   （图解如下）

所有的函数对象都有两个属性，一个是`显式属性`，一个是`隐式属性`。

怎么理解上面这句话？用如下代码解释

```javascript
function Foo(){};
var Foo=new Function();
```

**上面两行代码是一样的，也就解释了任何的函数对象都有显式属性和隐式属性。**

还有一点，==所有的构造函数实例对象的隐式原型式一样的==,但前提是==同一个构造函数实例出来的对象==

​					不同的构造函数实例出来的对象的隐式原型是不一样的。

示例：

```javascript
function Fn() {
  this.test1 = function () {
    console.log(this);
  }
};
var fo = new Function();
var fo0 = new Function();
var fo1 = new Fn();
console.log(fo0.__proto__ === fo.__proto__);  //true   说明构造函数实例出来的对象的__proto__是一样的
console.log(fo.__proto__ === fo1.__proto__);  //false		说明不同构造函数实例出来的对象的__proto__是不一样的
```

![原型链](D:\Typora记录文档\img\原型链.png)

还有以下两点注意：

```javascript
var Function=new Function();
function Function(){
  
};
```

只有如上这样才能保证`Function自身的显式原型`和`Function自身的隐式原型式`一样的。

这不也证明了**任何的函数对象都有显式属性和隐式属性。**

```javascript
var Object=new Function();
function Function(){};
console.log(Object.__proto__ === Function.prototype)   //true
```

第一行和第二行说明：**Object对象是Function构造函数的实例**，也说明实例对象的隐式原型等于构造函数的显式原型。



> Function.prototype成员

- arguments：获取函数的实参，已经被废弃了，现在推荐的做法是使用函数内部可用的 `arguments`对象来访问函数的实参。 
  - （废弃的意思：已经从 Web 标准中删除，虽然一些浏览器目前仍然支持它，但也许会在未来的某个时间停止支持，请尽量不要使用该特性。 ）
- length：获取形参的长度
- name：获取函数的名字，此属性不允许修改
- caller: 用于获取当前在函数是在哪个函数中调用的，已经被废弃了。
- constructor：指向当前构造函数，Function
- call：调用函数，重新指向this
- apply：调用函数，重新指向this
- bind：重新指向this，返回一个新的函数，不调用。





>#### 原型链的补充

**1.函数的显式原型指向的对象默认是`Object空对象`（但Object不满足）**

什么意思？

==**Object是所有对象的祖先，所以任何对象和Object做instanceof都会返回true**==（但Object不满足）

```javascript
function Fn() {
  this.test1 = function () {
    console.log(this);
  }
};
console.log(Fn.prototype instanceof Object);  //true
console.log(Object.prototype instanceof Object);  //false
console.log(Function.prototype instanceof Object);  //true
```

为什么Object函数对象不满足？

因为Object的原型属性指向的是`Object空对象的组先`，而其他函数的显式原型对象的原型才是Object空对象的组先。

```javascript
console.log(Object.prototype);   //Object的原型对象直接是组先
//而其他函数对象原型的原型才是主先
console.log(Fn.prototype.__proto__) 
console.log(Function.prototype.__proto__)
```

验证

```javascript
console.log(Object.prototype === Fn.prototype.__proto__);  //true
console.log(Object.prototype === Function.prototype.__proto__)  //true
//说明Object的原型属性指向的是`Object空对象的组先`，而其他函数的显式原型对象的原型才是Object空对象的组先。
```

**2.所有函数都是Function的实例（包括它本身Function）**

```javascript
console.log(Function.__proto__===Function.prototype);
```

**3.Object的原型对象是原型链的尽头**     

这一结论也说明了上面第一点为什么Object函数对象不满足。

```javascript
console.log(Object.prototype.__proto__)  //null
```



>#### 原型链_属性问题

1.读取对象的属性值时：会自动到原型链中查找

2.设置对象的属性值时，不会查找原型链，如果当前对象中没有此属性，直接添加此属性并设置其值

**3.方法一般定义在原型中，属性一般通过构造函数定义在对象本身上。**







##### instanceof

1.instanceof是如何判断的？

表达式：A instanceof B

如果`B函数的显式原型对象`在`A对象的原型链`上，返回true，否则返回false。







### 4. 执行上下文与执行上下文栈

#### 4.1 变量提升与函数提升

1.变量声明提升

​		通过var定义(声明)的变量，在定义语句之前就可以访问到

​		值：undefined

2.函数声明提升

​		通过function声明的函数，在之前就可以直接调用

​		值：函数定义（对象）

==注意：==**变量提升优先于函数提升。**也就是说先执行变量提升后执行函数提升。



#### 4.2 执行上下文

代码分类：1.全局代码逐行解读    2.函数(局部)代码逐行解读

>一、全局执行上下文

1.在执行全局代码前将window确定为全局执行上下文。

2.对全局数据进行`预解析处理`

​		-var定义的全局变量==>undefined,添加为window的属性。

​		-function声明的全局函数==>赋值(fun)，添加为window的方法

​		-this==>赋值(window)

3.开始逐行解读上下文过程。

>二、函数执行上下文

1.在调用函数，准备执行函数体之前，创建对应的函数执行上下文对象(**虚拟的，存在于栈中**)

2.对象局部数据进行预解析处理

​		-形参变量==>赋值(实参) ==>添加为执行上下文的属性。

​		-arguments==>赋值(实参列表)，添加为执行上下文的属性。

​		-var定义的局部变量==>undefined，添加为执行上下文的属性。

​		-function声明的函数==>赋值(fun),添加为执行上下文的方法

​		-this==>赋值（调用函数的对象）

3.开始逐行解读函数体代码



#### 4.3 执行上下文栈

==**什么时候产生执行上下文对象？函数执行的时候**==

那怎么知道产生的上下文对象数量？`公式:N+1`，N代码函数执行次数，1代表winodw。

>执行上下文栈

示例：

```javascript
//1.进入全局执行上下文
const a = 10;
var bar = function (x) {
    let b = 5;
    foo(x + b);     //3.进入foo函数执行上下文
}

var foo = function (y) {
    let c = 5;
    console.log(a + c + y)  //30
}
bar(10);     //2.进入bar函数执行上下文
```

用图理解执行上下文栈：

![image-20210421211059260](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210421211059260.png)

==栈容器的特点：==后进先出。因为出入口只有一个

理解说明：

1. 在全局代码执行前，JS引擎就会创建一个栈来存储管理所有的执行上下文对象
2. 在全局执行上下文（window）确定后，将其添加到栈中（**压栈，永远是最底下**）如图上所示
3. 在函数执行上下文创建后，将其添加到栈中（压栈）
4. 在当前函数执行完后，将栈顶的对象移除（出栈）
5. 当所有的代码执行完后，栈中只剩下window



执行上下文栈的面试题

```js
console.log('global begin:' + i); //undefined
var i = 1;
foo(1);
function foo(i) {
  if (i == 4) {
    return;
  }
  console.log('foo() begin:' + i);  //
  foo(i + 1);  //回调函数
  console.log('foo() end:' + i);  //
}
console.log('global end:' + i);  //1


/* 结果：global begin:undefined
                foo() begin:1
                foo() begin:2
                foo() begin:3
                foo() end:3
                foo() end:2
                foo() end:1
                global end:1

        一共产生5个执行上下文。
        window、foo(1)、foo(2)、foo(3)、foo(4)
*/
```



### 5. 作用域与作用域链

##### 作用域的理解

- 它就相当于一块“地盘”，一个代码段所在的区域
- 它是静态的（相当于上下文对象），在编写代码时就确定了，即==**作用域在编写函数的时候就确定了**==

分类

- 全局作用域
- 函数作用域
- 块级作用域（ES6有）

**作用：隔离变量，不同的作用域下同名变量不会有冲突**



作用域的示例：

```javascript
var a = 10, b = 20;
function fn(x) {
    var a = 100, c = 300;
    console.log('fn()', a, b, c, x);  //100,20,300,10
    function bar(x) {
        var a = 1000, d = 400
        console.log('bar()', a, b, c, d, x);  
        //第一次 1000 20 300 400 100
        //第二次 1000 20 300 400 200
    }
    bar(100);
    bar(200);
}
fn(10);
```



##### 作用域与执行上下文

**作用域与执行上下文区别1**

- 全局作用域之外，每个函数都会创建自己的作用域，作用域在函定义时就已经确定了。而不是在函数调用时
- 全局执行上下文对象是在全局作用域确定之后，js代码马上执行之前创建
- 函数执行上下文是在调用函数时，函数体代码执行之前创建

**作用域与执行上下文区别2**

- 作用域时静态的，只要函数定义好了就一直存在，且不会再变化
- 执行上下文是动态的，调用函数时创建，函数调用结束时就会被自动释放

**两者联系**

- 执行上下文是从属于所在的作用域（如下图）
- 全局上下文对象==>全局作用域
- 函数上下文对象==>对应的函数作用域

![image-20210421220615733](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210421220615733.png)

### 6. 闭包

#### 闭包的理解

>1.如何产生闭包？

当一个`嵌套的内部(子)函数`引用了`嵌套的外部(父)函数的变量`时，就产生了闭包

示例：

```javascript
function fn1() {
    var a = 1;
    function fn2() {
        console.log(a);  //执行函数定义就会产生闭包（不用调用内部函数），即嵌套内部函数声明时就会产生闭包。
    }
    fn2();
}
fn1();
```



>2.闭包是什么？

理解一：闭包是`嵌套的的内部函数`（绝大部分人）

理解二：包含`被引用变量（函数）的对象`（极少数人）

**我的理解是：函数的执行，导致函数被定义。**

==注意：闭包存在于嵌套的内部函数中==

>3.产生闭包的条件

1.函数嵌套（即有外部函数和内部函数嵌套）

2.内部函数引用了外部函数的数据（变量/函数）

3.执行外部函数

示例：

```javascript
function fn1() {
    var a = 1;    //当执行var a=1这行，fn2函数是已函数提升完了，因为js引擎的预解析处理，fn2闭包产生。
    function fn2() {   //执行函数定义就会产生闭包（不用调用内部函数），即嵌套内部函数声明时就会产生闭包。
        console.log(a);
    }
    fn2();
}
fn1();
```

![image-20210414144533266](D:\Typora记录文档\img\image-20210414144533266.png)



但是嵌套的内部函数如果是`函数表达式`方式，则要**执行完函数表达式才产生闭包**。

示例：    内部函数使用函数表达式方式。

```javascript
function fn1() {
  var a = 1;     //当执行到var a=1这行，fn2函数还是undefined，说明闭包没产生。
  var fn2 = function () {   //要执行完函数表达式后才能产生闭包。即执行到第5行才产生闭包。
    console.log(a);
  }
  fn2();
}
fn1();
```

![image-20210414144847110](D:\Typora记录文档\img\image-20210414144847110.png)

#### 闭包的作用

一：使用函数内部的变量在函数执行完后，**仍然存活在内存中(延长了局部变量的生命周期)。**

​		==本来函数执行完，函数内部的变量是被自动释放的==

二：让函数外部可以操作(读写)到函数内部的数据（变量/函数）。

示例：

```javascript
function fn1() {
    var arr = new Array(1000);
    function fn2() {
        console.log(arr.length);
    }
    return fn2;
}
var f = fn1();   //把内部函数的地址赋值给变量，使内部函数的生命周期延长。即内部函数还生效。
f();   //1000   //函数外操作这内部函数的局部变量。
```



#### 闭包的生命周期

1.产生：在嵌套内部函数定义执行完时就产生(不是在调用)。

2.死亡：在嵌套的内部函数成为垃圾对象时。--> ==函数对象=null==；

```javascript
function fn1() {
  var arr = new Array(1000);
  function fn2() {
    console.log(arr.length);
  }
  return fn2;
}
var f = fn1();   //把内部函数的地址赋值给变量，使内部函数的生命周期延长。即内部函数还生效。
f();
/* 
            执行完fn2函数后使其变成垃圾对象，回收内存空间。
        */
f = null;  //让内部函数成为垃圾对象--->回收闭包
```



#### 闭包的应用

1.能创建特定功能的JS文件。

2.将所有的数据和功能都封装在一个函数内部（私有的）

3.只向外暴露一个包含n个方法的对象或函数

4.模块的使用者，只需要通过`模块暴露的对象`调用方法来实现对应的功能。



#### 闭包的缺点及解决

>缺点

1.函数执行完后，函数内的==局部变量(变量、函数变量)==没有被释放，占用内存时间变长。

2.容易造成内存泄漏。

闭包造成的内存泄露

```javascript
function fn() {
  var a = 4;
  function fn3() {
    console.log(++a);
  }
  return fn3;
}
var f = fn();
f();  
//如果不把f变成垃圾对象回收，局部变量a会一直都在，这样也会容易造成内存泄露
```



>解决

1.能不用闭包就不用.

2.及时释放.  --->==函数对象=null==；



#### 内存溢出与内存泄漏

 内存溢出：

​        1.一种程序运行出现的错误

​        2.当程序运行需要的内存超过了剩余的内存时，就会抛出内存溢出的错误。

 内存泄露：

​        1.占用的内存没有及时释放

​        2.内存泄漏积累多了就会容易导致内存溢出。

>常见的内存泄露

1.意外的全局变量

```javascript
function fn() {
a = 2;   //全局变量
console.log(a);  //2
}
```

2.没有及时清理的计时器或回调函数

```javascript
var timer = setTimeout(function () {
  console.log('----');
}, 1000);
clearTimeout(timer);

//闭包
function fn() {
  var a = 4;
  function fn3() {
    console.log(++a);
  }
  return fn3;
}
var f = fn();
f();
```

3.闭包

```javascript
function fn() {
  var a = 4;
  function fn3() {
    console.log(++a);
  }
  return fn3;
}
var f = fn();
f();
```



### 5.防抖和节流

##### 什么是防抖和节流？

防抖和节流就是限制函数执行的次数。

1. 防抖：通过setTimeout的方式，在一定的时间间隔内，将多次触发变成`一次触发`;
2. 节流：减少一段时间的触发频率。



## 三、面向对象高级

### 1. 对象创建模式





### 2. 继承模式

##### 2.1 原型链继承

一、套路做法

```javascript
1.定义父类型构造函数
2.给父类型的原型添加方法
3.定义子类型的构造函数
4.创建父类型的对象赋值给子类型的原型
5.将子类型原型的构造属性设置为子类型   //即Sub.prototype.constructor=Sub;下面是漏掉的
6.给子类型原型添加方法
7.创建子类型的对象：可以调用父类型的方法
```

二、关键

**1.子类型的原型为父类型的一个实例对象**

图解示例：

![image-20210414165518665](D:\Typora记录文档\img\image-20210414165518665.png)

示例2：

![image-20210427111843708](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427111843708.png)

![image-20210427112124405](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427112124405.png)

​    

使用原型链继承时需要注意两个方面：

1. 子对象需要重写父对象中的某个方法，或者需要添加父对象中不存在的某个方法时，一定要放在继承语句之后。
2. 通过原型链继承后，不能使用对象字面量创建原型方法，否则会重写原型链，从而切断子对象和父对象的联系。

示例：

![image-20210427115744036](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427115744036.png)



在上面的例子中，Son的原型通过字面量方式创建原型方法，使得`Son.prototype`==>指向了`Object的原型对象`

![image-20210427120153122](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427120153122.png)

**==在实际开发中，很少单独使用原型链继承。==**



##### 2.2 借用构造函数继承

一、套路做法：

​	1.定义父类型构造函数

​	2.定义子类型构造函数

​	3.在子类型构造函数中调用父类型构造函数

二、关键

**在子类型构造函数中通过利用call()方法调用父类型构造函数**

示例：

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
function Student(name, age, price) {
  // this.name = name;
  // this.age = age;
  /* 
                除了上面这种还可以借用别的构造函数
            */
  Person.call(this, name, age);
  this.price = price;
}
var s1 = new Student('yyy', 22, 20000);
console.log(s1)  //Student {name: "yyy", age: 22, price: 20000}
```



##### 2.3 组合继承 （常用）

==方法是所有实例对象公用的，所以用原型继承；属性是每个对象自身的，所以用构造函数继承==

所以就有了**组合继承**

示例：

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
Person.prototype.test = function (name) {
  this.name = name;
}
function Student(name, age, price) {
  Person.call(this, name, age);   //为了得到属性
  this.price = price;
}
//方法原型链继承
Student.prototype = new Person();  //原型链继承  //为了能看到父类型的方法
Student.prototype.constructor = Student;  //修正constructor属性
Student.prototype.setPrice = function (price) {
  this.price = price;
}
console.log(Student.prototype);  //Person {name: undefined, age: undefined, constructor: ƒ}
var s1 = new Student('yangyeyong', 20, 20000);
console.log(s1);  //Student {name: "yangyeyong", age: 20, price: 20000}
s1.test('yyy');
s1.setPrice(25000);
console.log(s1);  //Student {name: "yyy", age: 20, price: 25000}
```



##### 2.4 复制继承

复制继承的基本思路是：将`父对象的原型属性`和`方法`逐一复制给子对象。

复制继承的基本思路：

```javascript
for(let item in 父对象){
    if(父对象.hasOwnProperty(item)){
        this[item]=父对象[item];
    }
}
//此功能重复，用函数封装起来复用
```

![image-20210427164810200](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427164810200.png)

上面的例子中，父对象father的hobby属性是引用类型数据，所以修改一处，其它地方都会变。

![image-20210427165848730](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427165848730.png)

修改后： father.hobby[0] = 'sleep';

![image-20210427170013405](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427170013405.png)



##### 2.5 组合借用构造函数和复制继承  （常用）

1. 属性是通过借用构造函数来继承

   方法通过复制继承来实现

![image-20210427173509500](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427173509500.png)

结果：

![image-20210427173714798](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427173714798.png)



##### 2.6 原型式继承（经典继承）

在ECMAScript通过增加Object.create()方法将原型式继承的概念规范化了。

​		语法：var newO=Object.create(obj)

​		作用：该方法可以创建一个新的对象并且返回出来

示例：

![image-20210427180724670](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210427180724670.png)



## 四、线程机制与事件机制

##### 浏览器内核模块组成

主线程

- js引擎模块：负责js程序的编译运行
- html，css文档解析模块：负责页面文本的解析
- DOM、CSS模块：负责dom/css在内存中的相关处理
- 布局和渲染模块：负责页面的布局和效果的绘制(内存中的对象)

分线程

- 定时器模块：负责定时器的管理
- 事件响应模块：负责事件的管理
- 网络请求模块：负责ajax请求



> 事件循环模型：

![image-20210418171356141](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210418171356141.png)



由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为==事件循环（ event loop）==。

![image-20210401164201135](D:\Typora记录文档\img\image-20210401164201135.png)



1.如何证明js执行是单线程的？

- setTimeout()的回调函数是**在主线程执行的**
- 定时器回调函数只有在==运行栈中的代码全部执行完后才有可能执行==

示例：

```javascript
function fun1() {
    console.log('fun1()');
}
function fun2() {
    console.log('fun2()');
}
setTimeout(function () {
    console.log('定时器开始------');    //控制台打印：定时器开始------
    alert('22222');
    console.log('定时器完成------');    //要等弹出框函数在执行栈中完成就能打印
}, 1000);
```

2.为什么js要用单线程模式，而不用多线程模式？

- javaScrip的单线程，与它的用途有关。
- 作为浏览器脚本语言，javaScript的主要用途是与用户互动，以及操作DOM
- 这决定了javaScript只能是单线程，否则会带来很复杂的同步问题

>3.代码的分类

- 初始化代码(同步任务)：包含定时器、绑定dom事件监听、发送ajax请求
- 回调代码(异步任务)：处理回调逻辑



4.JS引擎执行代码的基本流程：

- 先执行初始化代码。
- 后执行回调代码。

示例：

```javascript
function fun1() {
    console.log('fun1()');
}
function fun2() {
    console.log('fun2()');
}
//定时器中的回调函数要在执行栈中全部执行完才往后进行。
setTimeout(function () {
    console.log('定时器开始------');
    alert('22222');
    console.log('定时器完成------');
}, 1000);
setTimeout(function () {
    console.log('第二个定时器开始-----');
    var i = Date.now();
    console.log(i);
    console.log('第二个定时器结束-----');
}, 1000)
fun1();
fun2();

//fun1()
//fun2()
//线程机制与事件机制.html:15 fun2()
//线程机制与事件机制.html:19 定时器开始------
//线程机制与事件机制.html:21 定时器完成------
//线程机制与事件机制.html:24 第二个定时器开始-----
//线程机制与事件机制.html:26 1618734402647
//线程机制与事件机制.html:27 第二个定时器结束-----
```

