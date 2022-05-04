

# javaScript基础

## 一、标识符

在js中所有的可以自主命名的都可以称为`标识符`。

例如：变量、函数名、属性名都属于标识符。

### 1.1 命名标识符要遵守的规则

1.标识符中可以含有字符、数字、下划线、$。

2.标识符不能以数字开头。

3.标识符不能是ES中的关键字和保留字。

4.标识符一般都采用驼峰命名法。（首字母小写，每个单词的开头字母大写）



## 二、变量

### 2.1 变量的概念

`变量`是程序在内存中申请的一块用于存放数据的空间。变量是用于存放数据的容器，可以通过变量名获取数据，甚至修改数据。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg10zIf7FspMlPjibM3bQxBro5MnScFvHVAdwNxJ6MricibGgwtMgSnGbjGA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 2.2 变量的使用

**「1. 声明变量」**

```
// 1.声明变量
var num; // 声明一个 名称为 num 的变量
```

var是一个JS关键字，用来声明变量(variable变量的意思)。num是我们定义的变量名，可以通过变量名来访问内存中分配的空间。

**「2. 赋值」**

```
num = 10;//给 num 这个变量赋值为 10
```

**「3. 变量的初始化」**

声明一个变量并赋值， 我们称之为变量的初始化。

```
var num = 10;//声明变量并赋值为10 
```

**「4. 变量语法扩展」**

```
// 1.一个变量被重新赋值后，它原有的值会被覆盖掉，变量值以最后一次赋的值为准。
var num = 10;
num = 11;
// 2.同时声明多个变量(只需要写一个var,多个变量名之间用英文逗号隔开)
var num = 10, age = 15, name = 'fan';
```

> 声明变量特殊情况

|             情况              |          说明           |   结果    |
| :---------------------------: | :---------------------: | :-------: |
| var  age ; console.log (age); |      只声明 不赋值      | undefined |
|       console.log(age)        | 不声明 不赋值  直接使用 |   报错    |
| age  = 10; console.log (age); |     不声明  只赋值      |    10     |

**「5. 变量命名规范」**

1. 由字母、数字、下划线、美元符号组成。
2. 严格
3. 不能以数字开头。
4. 不能是关键字和保留字。
5. 变量名要有意义。
6. 遵守驼峰命名法。首字母小写，后面单词的首字母大写。如myFirstName.



## 三、数据类型

分为两类：简单数据类型(Number,String,Boolean,Undefined,Null)和复杂数据类型(object)。

| 简单数据类型 | 说明                                             | 默认值    |
| :----------- | :----------------------------------------------- | :-------- |
| Number       | 数字型,包含整型值和浮点型值                      | 0         |
| String       | 字符串型                                         | ""        |
| Boolean      | 布尔值型                                         | false     |
| Undefined    | var a;声明了变量a但是没有赋值，此时a = undefined | undefined |
| Null         | var a = null;声明了变量a为空值                   | null      |

### 3.1 Number数字型

**「数字型进制」**

```javascript
// 1.在JS中八进制前面加0，十六进制前面加 0x
var num1 = 07;   // 对应十进制的7
// 2.十六进制数字序列范围：0~9以及A~F
 var num = 0xA;   
```

**「数字型范围」**JavaScript中的数值有最大值和最小值

最大值:`Number.MAX_VALUE`，值为：1.7976931348623157e+308

最小值:`Number.MIN_VALUE`，值为：5e-32

特殊值：`Infinity` 无穷大 `-Infinity` 无穷小 `NaN` 代表一个非数字

isNaN():用来判断一个变量是否为非数字的类型。非数字型为true,数字型为false。

```javascript
var b=1;
isNaN(b);   //false 说明是数字型。
var a="yyy";
isNaN(a);   //true  说明是非数字类型。
```

### 3.2 String字符串型

```
// 1.字符串型可以是引号中的任意文本，语法为 单引号 和 双引号
var msg = '我的名字叫';
var name = "fan";
```

**「1. 字符串转义符」**都是以 \ 开头，详细如下

| 转义符 | 说明                       |
| :----- | :------------------------- |
| \n     | 换行符，n是 newline 的意思 |
| \\     | 斜杆 \                     |
| \'     | 单引号 ‘                   |
| \"     | 双引号 "                   |
| \t     | tab 缩进                   |
| \b     | 空格,b是 blank 的意思      |

**「2. 字符串长度」**
  字符串是由若干字符组成的，这些字符的数量就是字符串的长度。

```
// 1.字符串型可以是引号中的任意文本，语法为 单引号 和 双引号
var msg = '我是帅气的饭老板';
console.log(msg.length); // 显示 8
```

==**重要！！「3. 字符串拼接」**==
  多个字符串之间可以使用 + 进行拼接，其拼接方式为 **字符串 + 任何类型 = 拼接之后的新字符串。**
  拼接前会把与字符串相加的任何类型转成字符串，再拼接成一个新的字符串

```
//1.1 字符串 "相加"
alert('hello' + ' ' + 'world'); // hello world
//1.2 数值字符串 "相加"
alert('100' + '100'); // 100100
//1.3 数值字符串 + 数值
alert('11' + 12);     // 1112 +号口诀：数值相加，字符相连
// 1.4 字符串拼接加强
var age = 18;
alert("饭老板今年" + age +"岁了");
```

### 3.3 布尔型Boolean

布尔类型有两个值：true 和 false ，其中 true 表示真（对），而 false 表示假（错）。
布尔型和数字型相加的时候， true 的值为 1 ，false 的值为 0。

```
console.log(true + 1) // 2
console.log(false + 1) // 1
```

### 3.4 Undefined 和 Null

一个变量声明后没有赋值会有一个默认值undefined(如果相连或者相加时，注意结果)

```
  var variable;
  console.log(variable); // undefined
  console.log("你好" + variable); // 你好undefined
  console.log(11 + variable); // NaN
  console.log(true + variable);// NaN
```

  一个变量声明并赋值null,里面存的值为空

```
  var var2 = null;
  console.log(var2); // null
  console.log("你好" + var2); // 你好null
  console.log(11 + var2); // 11
  console.log(true + var2);// 1
```

null这个值专门用来表示一个`空的对象`。

**使用typeof检查一个null值时，会返回object。**   

所以有如下检测

```javascript
var i = null;
console.log(typeof i === 'object');  //true
```

### 3.5 获取变量类型及转换

检测变量的数据类型 `typeof`

```javascript
var num = 10;
console.log(typeof num)//结果为 number
```

字面量:是源代码中一个固定值的表示法，就是字面量如何去表达这个值。通过数据的格式特征可以判断数据的类型

有数字字面量:8,9,10

字符串字面量:'饭老板'，"前端开发"

布尔字面量:true,false



#### 转换为字符串

| 方式               | 说明                             | 案例                                                  |
| :----------------- | :------------------------------- | :---------------------------------------------------- |
| toString()方法     | 转成字符串                       | var num=1; console.log(num.toString())                |
| String()函数       | 强制转换                         | var num=1;  console.log(String(num));                 |
| **加号拼接字符串** | **和字符串拼接的结果都是字符串** | var num=1; alert(num+'我是字符串')    //num我是字符串 |

==注意：==null和undefined这两个值没有toSrting()方法，如果调用他们会报错。

#### 转换为数字型

| 方式                     | 说明                         | 案例               |
| :----------------------- | :--------------------------- | :----------------- |
| parseInt(String)函数     | 将string类型转成整数型       | parseInt('11')     |
| parseFloat(String)函数   | 将string类型转成浮点型       | parseFloat('11.2') |
| Number() 强制转换函数    | 将string类型强制转换为数值型 | Number('12')       |
| **js隐式转换 ( - \* /)** | 利用算数运算隐式转换为数值型 | '12' - 0           |

#### 转换为布尔型

==代表空、**否定的值会被转换为false，如空字符、0、NaN、null、undefined**    其余值都会被转换为true==

| 方式          | 说明                 | 案例            |
| :------------ | :------------------- | :-------------- |
| Boolean()函数 | 其他类型转换为布尔值 | Boolean('true') |

### 3.6 关键字和保留字

**「标识符」**指开发人员为变量、属性、函数、参数取得名字。标识符不能是关键字或保留字。

**「关键字」**指 JS本身已经使用了的字，不能再用它们充当变量名、方法名

> 包括：break、case、catch、continue、default、delete、do、else、finally、for、function、if、in、instanceof、new、return、switch、this、throw、try、typeof、var、void、while、with 等。

**「保留字」**实际上就是预留的“关键字”，意思是现在虽然还不是关键字，但是未来可能会成为关键字，同样不能使用它们当变量名或方法名。

> boolean、byte、char、class、const、debugger、double、enum、export、extends、fimal、float、goto、implements、import、int、interface、long、mative、package、private、protected、public、short、static、super、synchronized、throws、transient、volatile 等。
>
> 注意：如果将保留字用作变量名或函数名，那么除非将来的浏览器实现了该保留字，否则很可能收不到任何错误消息。当浏览器将其实现后，该单词将被看做关键字，如此将出现关键字错误。

## 四、运算符与流程控制

### 4.1 运算符(操作符)

**「运算符」**是用于实现赋值、比较和执行算数运算等功能的符号。常用运算符分类如下

算数运算符、递增和递减运算符、比较运算符、逻辑运算符、赋值运算符。

#### 算数运算符

| 运算符            | 描述       | 案例                |
| :---------------- | :--------- | :------------------ |
| +                 | 加         | 10+20=30            |
| -                 | 减         | 10-20=-10           |
| *                 | 乘         | 10*20=200           |
| ==/==   ---有小数 | 除         | 10/20=0.5           |
| %                 | 取余(取模) | 返回除法的余数9%2=1 |

#### 一元运算符的+

就是正号，不会对值产生任何影响。

但是对于`非Number类型的值`，它会**将其转换为Number。它的原理跟Number()函数一样**。

此方法在日期对象中用的多，比如

```javascript
var now =new Date(); 
console.log(now);  //Sat Apr 03 2021 15:58:43 GMT+0800 (中国标准时间)
但是利用 一元运算符的+
var now=+new Date();
console.log(+now);   //1617436698350
console.log(typeof now);    //Number类型
```

浮点数的精度问题

```javascript
var result = 0.1 + 0.2;    // 结果不是 0.3，而是：0.30000000000000004
console.log(0.07 * 100);   // 结果不是 7，  而是：7.000000000000001
```

浮点数值的最高精度是17位小数，但是在进行算数运算时其精确度远远不如整数,所以不要直接判断两个浮点数是否相等!

#### 表达式与返回值

表达式：由`数字`、`运算符`和`变量`组成的式子。

返回值：每一个表达式经过相应的运算之后，会有一个最终结果，称为表达式的返回值==（即表达式会有返回值！）==

#### 递增和递减运算符

递增和递减运算符必须配合变量使用。

==++a==:先自增加+1，然后放回值。

==a++==:先返回值，后再+1。

两者是有区别的！！！不要弄混淆了。

递增运算符：

```
  var  num = 10;
  alert(++num + 10);   // 21 使用口诀：先自加，后返回值
  
  var  num1 = 10;
  alert(10 + num1++);  // 20 使用口诀：先返回原值，后自加 
  var num = 1;
  var num2 = ++num + num++; //num = 2
  console.log(num2);//4
  
  var num = 1;
  var num1 = 1;
  var num2 = num++ + num1++; // 1 + 1
  console.log(num2);//2
  
  var num = 1;
  var num2 = num++ + num++;// 1 + 2 
  console.log(num2); // 3  
  
```

#### 关系运算符

| 运算符 | 描述                                    | 案例        | 结果  |
| :----: | :-------------------------------------- | :---------- | :---- |
|   <    | 小于号                                  | 1<2         | true  |
|   >    | 大于号                                  | 1>2         | false |
|   >=   | 大于等于号(大于或者等于)                | 2 >= 2      | true  |
|   <=   | 小于等于号(小于或者等于)                | 3 <= 2      | false |
|   ==   | 判等号(会转型)                          | 15 == '15'  | true  |
|   !=   | 不等号                                  | 37 != 37    | false |
|  ===   | 全等 全不等(**要求值和数据类型都一致**) | 37 === '37' | false |

还有个不全等：!==  ==要求值和数据类型都要一致==

#### 逻辑运算符

  逻辑运算符是用来进行布尔值运算的运算符
  短路运算:当有多个表达式（值）时,左边的表达式值可以确定结果时,就不再继续运算右边的表达式的值;

| 运算符 | 描述                  | 案例          | 特点                     |
| :----: | :-------------------- | :------------ | :----------------------- |
|   &&   | "逻辑与",简称"与" and | true && false | 两边都是 true才返回 true |
|  \|\|  | "逻辑或",简称"或" or  | true          | 有真为真                 |
|   !    | "逻辑非",简称"非" not | !true         | 取反                     |

**&& 与：在JS中的“与”属于短路的与，如果第一个值为false，则不会看第二个值。**

**|| 或：在JS中的“或”属于短路的或，如果第一个值为true，则不会检查第二个值。**

#### 赋值运算符

|  运算符  | 描述                 | 案例                 |
| :------: | :------------------- | :------------------- |
|    =     | 直接赋值             | var userName = 'fan' |
|  += -=   | 加减一个数后再赋值   | var age=5; age+=5    |
| *= /= %= | 乘、除、取模后再赋值 | var age=5; age*=5    |

#### 运算符优先级

| 优先级 | 运算符     | 顺序          |
| :----: | :--------- | :------------ |
|   1    | 小括号     | ()            |
|   2    | 一元运算符 | ! ++ --       |
|   3    | 算数运算符 | 先* / % 后+ - |
|   4    | 关系运算符 | >, >=, <, <=  |
|   5    | 相等运算符 | == != === !== |
|   6    | 逻辑运算符 | 先&& 后\|\|   |
|   7    | 赋值运算符 | =             |
|   8    | 逗号运算符 | ,             |

### 4.2 流程控制

​		**流程控制**在一个程序执行的过程中，各条代码的执行顺序对程序的结果是有直接影响的。很多时候我们要通过控制代码的执行顺序来实现我们要完成的功能。流程控制主要有三种结构，分别是**顺序结构**、**分支结构**和**循环结构**，代表三种代码执行的顺序。

### 4.3 分支流程控制

```javascript
// 1. 条件成立执行的代码语句
if (条件表达式) { 
}

// 2.if else 语句
if (条件表达式) {
    // [如果] 条件成立执行的代码
} else {
    // [否则] 执行的代码
}

// 3. if else if 语句(多分支语句)
// 适合于检查多重条件。
if (条件表达式1) {
    语句1；
} else if (条件表达式2)  {
    语句2；
} else if (条件表达式3)  {
    语句3；
        ....
} else {
    // 上述条件都不成立执行此处代码
}
```

### 4.4 三元表达式

> 条件表达式?语句1：语句2；

执行的流程：条件运算符在执行时，首先对条件表达式进行求值，

如果该值为`true`，则执行语句1，并返回执行结果。

如果改制为`false`，则执行语句2，并返回执行结果。

```javascript

```

### 4.5 switch分支流程控制

它用于基于不同的条件来执行不同的代码。当要针对变量设置一系列的特定值的选项时，就可以使用 switch。

```javascript
switch( 条件表达式 ){ 
  case value1:
    // 表达式 等于 value1 时要执行的代码
    break;
  case value2:
    // 表达式 等于 value2 时要执行的代码
    break;
  default:
    // 表达式 不等于任何一个 value 时要执行的代码
}

//示例
var i = +prompt('请输入你的分数');
switch (i >= 60) {
  case true:
    console.log("goods!");
    break;
  default:
    console.log("bad~");
    break;
}
```



## 五、循环与代码规范

### 5.1 循环

| 运算符     | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| 初始化变量 | 通常被用于初始化一个计数器,该表达式可以使用var关键字声明新的变量，这个变量帮我们来记录次数。 |
| 条件表达式 | 用于确定每一次循环是否能被执行，如果结果是true就继续循环，否则退出循环 |
| 操作表达式 | 每次循环的最后都要执行的表达式。通常用于更新计数器变量的值   |

```javascript
for(初始化变量; 条件表达式; 操作表达式 ){
    //循环体
}
```

**「执行流程」**

1. 初始化变量，初始化操作在整个 for 循环只会执行一次。
2. 执行条件表达式，如果为true，则执行循环体语句，否则退出循环，循环结束。
3. 执行操作表达式,此时第一轮结束。
4. 第二轮开始，直接去执行条件表达式（不再初始化变量），如果为 true ，则去执行循环体语句，否则退出循环。
5. 继续执行操作表达式，第二轮结束。......



#### 双重for循环

循环嵌套是指在一个循环语句中再定义一个循环语句的语法结构，例如在for循环语句中，可以再嵌套一个for 循环，这样的 for 循环语句我们称之为双重for循环。

```javascript
for (外循环的初始; 外循环的条件; 外循环的操作表达式) {
  for (内循环的初始; 内循环的条件; 内循环的操作表达式) {  
    需执行的代码;
  }
}
//for循环打印九九乘法表
var str = "";
for (var i = 1; i <= 9; i++) {
  for (var j = 1; j <= i; j++) {
    str += j + "x" + i + "=" + j * i + "\t";
  }
  str += "\n";
}
console.log(str);
```



#### while循环

1.先执行条件表达式，如果结果为 true，则执行循环体代码；如果为 false，则退出循环，执行后面代码

2.执行循环体代码

3.循环体代码执行完毕后，程序会继续判断执行条件表达式

```javascript
while (条件表达式) {
  // 循环体代码 
}

//计算1-100的累加和
var i = 1;
var sum = 0;
while (i <= 100) {
  sum += i;
  i++;
}
console.log(sum);
```



#### do-while循环

它和`while`循环的唯一区别在于:它永远会先执行结果，然后再判断。

```javascript
do {
  // 循环体代码 - 条件表达式为 true 时重复执行循环体代码
} while(条件表达式);
// 先执行一次循环体代码，再执行条件表达式
//计算100以内的偶数累加和
var i = 1;
var sum = 0;
do {
  if (i % 2 == 0) {
    sum += i;
  }
  i++;
} while (i <= 100);
console.log(sum);
```

用`do { ... } while()`循环要小心，循环体会至少执行1次，而`for`和`while`循环则可能一次都不执行。



#### continue、break

  `continue` 关键字用于立即跳出本次循环，继续下一次循环（本次循环体中 continue 之后的代码就会少执行一次）。
  `break` 关键字用于立即跳出整个循环（循环结束）。



#### 代码规范

**变量、函数的命名必须要有意义**

变量的名称一般用名词

函数的名称一般用动词

```javascript
2. 操作符规范
// 操作符的左右两侧各保留一个空格
for (var i = 1; i <= 5; i++) {
  if (i == 3) {
    break; // 直接退出整个 for 循环，跳到整个for循环下面的语句
  }
  console.log('我正在吃第' + i + '个包子呢');
}
3. 单行注释规范
for (var i = 1; i <= 5; i++) {
  if (i == 3) {
    break; // 单行注释前面注意有个空格
  }
  console.log('我正在吃第' + i + '个包子呢');
}
4. 其他规范
//关键词 操作符空格
if (true) {}
for (var i = 0; i<=10; i++) {}
```



## 六、数组与函数

### 6.1 数组

#### 数组的概念 

数组即==一组数据的集合==，其中的每个数据被称作`元素`，在数组中可以存放任意类型的元素。数组是一种将一组数据存储在单个变量名下的优雅方式。

#### 创建数组

- 利用new关键字创建数组；

```javascript
var 数组名 = new Array([n]);//[]代表可选 若写n，则代表数组的长度
var arr = new Array();//创建了一个名为 arr 的空数组
```

- 利用数组字面量创建数组

```javascript
// 1. 使用数组字面量方式创建空的数组
var 数组名 = [];//若写n，则代表数组的长度

//2. 使用数组字面量方式创建带初始值的数组
//3. 声明数组并赋值称为数组的初始化
var arr =['1','2','3','4'];
var arr2 = ['fan',true,17.5];//数组中可以存放任意类型的数据
```

#### 访问数组元素

索引(下标):用来访问数组元素的序号。索引从 `0` 开始

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg11zuLXUFm8CchlfazKYsn6SIAhks94FgU6eXrE1n85scEic0mBsVCfug/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
// 定义数组
var arrStus = [1,2,3];
// 获取数组中的第2个元素 
alert(arrStus[1]); 
// 如果访问数组时没有和索引值对应的元素(数组越界)，
// 返回值为undefined
```

#### 遍历数组

把数组中的元素从头到尾都访问一次。

```javascript
// 数组的长度,默认情况下等于元素的个数
// 当我们数组里面的元素发生了变化，length属性跟着一起变
// 如果设置的length属性值大于数组的元素个数，则会在数组末尾出现空白元素；
// 如果设置的length属性值小于数组的元素个数，则会把超过该值的数组元素删除
var arr = ["red", "blue", "green"];
for (var i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}

arr.length = 2;
console.log(arr);// red blue
```

#### 数组中新增元素

数组中可以通过以下方式在数组的末尾插入新元素;

```javascript
// 1. 数组[数组.length] = 新数据;
arr = [] //arr.length = 0;
for (var i = 0; i < 10; i++) {
  arr[arr.length] = '0';
}
console.log(arr);
```

#### 案例

```javascript
// 1. 筛选数组 大于10的元素选出来放到新数组中
var arr = [2, 0, 6, 1, 77, 0, 52, 0, 25, 7];
var newArr = [];
var j = 0;
for (var i = 0;i < arr.length; i++){
  if (arr[i] >= 10) {
    newArr[j] = arr[i];
    j++;
  }
}
console.log(newArr);
//第二种方法 利用数组长度的可变性
for (var i = 0;i < arr.length; i++){
  if (arr[i] >= 10) {
    newArr[j] = arr[i];
    j++;
  }
}
// 2. 翻转数组 
//  把旧数组索引号第4个取过来（arr.length - 1)，给新数组索引号第0个元素 (newArr.length)
//  我们采取 递减的方式  i--
var arr = ['red', 'green', 'blue', 'pink', 'purple'];
var newArr = [];
for(var i = arr.length - 1; i >= 0;i--) {
  newArr[newArr.length] = arr[i]
}
console.log(newArr);
// 3. 数组转换为字符串 用"|" 或其他符号分割
// 需要一个新变量用于存放转换完的字符串str
//遍历取出数据加到str后面然后加上分隔符
var arr = ['red', 'green', 'blue', 'pink', 'purple'];
var str = '';
for(var i = 0; i < arr.length; i++) {
  str += arr[i] + '|';
}
console.log(str);
// 4. 数组转换为字符串 用"|" 或其他符号分割
// 需要一个新变量用于存放转换完的字符串str
//遍历取出数据加到str后面然后加上分隔符
var arr = ['red', 'green', 'blue', 'pink', 'purple'];
var str = '';
for(var i = 0; i < arr.length; i++) {
  str += arr[i] + '|';
}
console.log(str);
```

`冒泡排序`：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1wFVYCSxwqYic9FXqU9WvZD7ko1WbSOc9wzV5HdNaOicFUy68zeWunqNg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
function sort(arr) {
    for(var i = 0; i < arr.length - 1; i++) {
        for(var j = 0; j < arr.length - 1 - i; j++) {
            if (arr[j] > arr[j+1]){
                var temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
    return arr;
}
var arr1 = sort([1,4,2,9]); 
console.log(arr1);//1 2 4 9
```

### 6.2 函数

#### 函数的概念

​		**封装了一段可被重复调用执行的代码块**，通过函数可以实现大量代码的重复使用。函数是一种数据类型,也是一个对象。

#### 函数的使用

声明函数

```javascript
1. 通过function关键字定义函数 -- 命名函数
function 函数名() {
  //函数体代码
}
// 1.1 function 是声明函数的关键字，必须小写
// 1.2 函数名 命名为动词形式 例: getSum


2. 通过函数表达式定义函数 ---匿名函数
var fn = function() {};
// 2.1 fn是变量名，不是函数名
// 2.2 fn是变量，只不过变量存储的是函数
// 2.3 函数表达式创建的函数可以通过 变量名(); 来调用
// 2.4 函数表达式也可以定义形参和调用传入实参。
匿名函数使用的第2种方式--匿名函数自调用
(function(){
  alert(123);
})();
```

调用函数

```
函数名();// 函数声明后调用才会执行函数体代码
```

函数的封装

**函数的封装是把一个或者多个功能通过函数的方式封装起来，对外只提供一个简单的函数接口。**

```javascript
/*
    例用封装函数计算1-100累加和
  */
function getSum() {
  var sumNum = 0; // 准备一个变量，保存累加和
  for (var i = 1; i <= 100; i++) {
    sumNum += i; // 把每个数值 都累加 到变量中
  }
  alert(sumNum);
}
// 调用函数
getSum();
```

#### 函数的参数

形参：函数定义时候，传递的参数(实参值传递给形参,不用声明的变量)

**实参：函数调用时候，传递的参数**

```javascript
//带参数的函数声明
function 函数名(形参1,形参2,形参3...) {
  //函数体
}
// 带参数的函数调用
函数名(实参1,实参2,实参3...);
```

**实参可以是任意的数据类型，也可以是一个对象。**当我们的==参数过多时==，可以==将参数封装到一个对象中，然后通过对象传递！！==，这是技巧。

易错点的地方：

```javascript
function mianji(r) {
  return 3.14 * r * r;
}
function fun(a) {
  console.log("a的值是：" + a);
}
console.log(fun(mianji));
console.log(fun(mianji(10)));

/* mianji和mianji(10)的区别 
  mianji相对于函数变量，给fun函数传递的是function mianji(r) {
            return 3.14 * r * r;
}
而mianji(10)是返回后的值，那fun(mianji(10))函数==>fun(314);
*/
```

示例：

```javascript
function sayHello(o) {
  console.log("我是" + obj.name + "今年" + obj.age + "性别" + obj.sex + "家在" + obj.address);
}
//创建一个对象
var obj = {
  name: "小杨",
  age: 22,
  sex: "男",
  address: "广东中山"
}
sayHello(obj)//将对象作为实参传递过去。

//结果：我是小杨今年22性别男家在广东中山
```

> 函数形参和实参数量不匹配时

| 参数个数             | 说明                              |
| :------------------- | :-------------------------------- |
| 实参个数等于形参个数 | 输出正确结果                      |
| 实参个数多于形参个数 | 只取到形参的个数                  |
| 实参个数小于形参     | 多的形参定义为undefined,结果为NaN |

```javascript
function getSum(a, b, c) {
  return a + b + c;
}
// js中形参的默认值是undefined。
// 调用函数
var n = getSum(1, 2);// n = NaN
var n = getSum(1, 2, 3, 4); //1 + 2 +3 = 6
```

#### 函数的返回值

返回值：函数调用整体代表的数据；函数执行完成后可以通过return语句将指定数据返回 。

==注意==:返回值可以是任意的数据类型，也可以是一个对象，也可以是一个函数。

```javascript
// 声明函数
function 函数名() {
  ...
  return 需要返回的值;
  // 1. 函数遇到return会停止执行，并返回指定的值
  // 1. 如果函数没有return 返回的值是undefined
}
// 调用函数
函数名(); //此时调用函数就可以得到函数体内return的值
break,continue,return的区别
```

break: 结束当前的循环体 (如for、while)

continue: 跳出本次循环，继续执行下次循环

return: 不仅可以退出`(函数体内)`循环，还能够返回return语句中的值，同时还可以`结束当前的函数体内的代码`

```javascript
//避免踩坑 return只能结束函数体内的代码
function breakDown() {
  for (var i = 0; i < 10; i++) {
    if (i == 5) {
      return 1;
    }
    console.log(i);
  }
}
breakDown();

//避免踩坑2 函数如果有return 则返回的是 return 后面的值；
// return d,a,b; 返回的是b的值
//如果函数没有 return语句，则返回undefined

```

#### arguments的使用

  当不确定有多少个参数传递的时候，可以用 arguments 来获取。JS中，arguments实际上它是当前函数的一个内置对象。所有函数都内置了一个 arguments 对象，**arguments 对象中存储了传递的所有实参**。arguments展示形式是一个伪数组，因此可以进行遍历。

伪数组具有以下特点：

1.具有length属性

2.按索引方式存储数据

3.不具有数组的push，pop等方法

```javascript
function fn() {
  //arguments 里面存储了所有传递过来的实参
  console.log(arguments);// [1,2,3...]
  console.log(arguments[1]); // 2
  console.log(arguments.length); // 3

  //我们可以按照数组的方式 遍历argument  
}
fn(1, 2, 3);
// 用伪数组 实现求最大值
function getMax() {
  var max = arguments[0];
  for (var i = 1; i < arguments.length; i++) {
    if (arguments[i] > arguments[0]) {
      max = arguments[i];
    }
  }
  return max;
}
var result = getMax(1,3,77,5,85)
colsole.log(result);
```



## 七、作用域

### 7.1 作用域

**「作用域」**一段程序代码中所用到的名字并不总是有效和可靠的，而限定这个名字的可用性代码范围就是这个名字的作用域。

- 作用域的使用提高了程序逻辑的局部性，增强了程序的可靠性，减少了名字冲突。
- ES6之前作用域有两种 `全局作用域`和`局部作用域`(函数作用域)

**「全局作用域」**作用于所有代码执行的环境(整个 script 标签内部)或者一个独立的js文件。

**「局部作用域」**作用于函数内部的代码环境，就是局部作用域。因为跟函数有关系，所以也被称为`函数作用域`。

**「JS没有块级作用域」**

- 块作用域由 {} 包括
- 在其他编程语言，if语句中，循环语句创建的变量，仅仅只能在本if语句，本循环语句中使用，如下👇👇

```javascript
if(true){
  int num = 123;
  System.out.print(num); //123
}
System.out.print(num);//报错
```

-------以上java代码会报错，因为代码中 {}是一块作用域，其中声明的变量num，在{}之外不能使用，而JavaScript代码则不会报错

-------Js中没有块级作用域 (在ES6之前)

```javascript
if(true){
  var num = 123;
  console.log(num); // 123
}
console.log(num);// 123
```

### 7.2 变量的作用域

在JavaScript中，根据作用域的不同，变量可以分为两种:

`全局变量`和`局部变量`

**「全局变量」**在全局作用域下声明的变量(在函数外部定义的变量)

- 全局变量在代码的任何位置都可以使用
- 在全局作用域下 var 声明的变量 是全局变量
- 特殊情况下，在函数内不使用var声明的变量也是全局变量(不建议使用)。

**「局部变量」**在局部作用域下声明的变量(在函数内部定义的变量)

- 局部变量只能在函数内部使用
- 在函数内部 var声明的变量是局部变量
- **函数的形参实际上就是局部变量**

**「全局变量和局部变量的区别」**

- `全局变量:`在任何一个地方都可以使用，只有在浏览器关闭时才会销毁，因此比较占内存
- `局部变量:`旨在函数内部使用，当其所在的代码块被执行时，才会被初始化；当代码块运行结束后，就会被销毁，因此更节省内存空间。

### 7.3 作用域链

**「作用域链」**只要是代码都在一个作用域中，写在函数内部的局部作用域，未卸载仍和行数内部即在全局作用域中；如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域；根据`[内部函数可以访问外部函数变量]` 的这种机制，用链式查找决定哪些数据能被内部函数访问，就称作作用域链。

```javascript
function f1() {
  var num = 123;
  function f2() {
    console.log( num );
  }
  f2();
}
var num = 456;
f1();
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1nLvBzPbOzhHzq04EPib4YqOvAMCVWYRZwjJ1ZmicsxuTXChF9qdRAWzw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

`作用域链`采取==就近原则的方式==来查找变量最终的值

```javascript
var a = 1;
function fn1() {
  var a = 2;
  var b = '22';
  fn2();
  function fn2() {
    var a = 3;
    fn3();
    function fn3() {
      var a = 4;
      console.log(a); //a的值 4
      console.log(b); //b的值 '22'
    }
  }
}
fn1();
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1OQJFh6v31DwpbOHRiam3jibSh4Xkvs727QcMs1aroAK8fKvsaibJyKjvw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 7.4 预解析

**「预解析相关概念」**JavaScript代码是由浏览器中的JavaScript解析器来执行的。JavaScript解析器在运行JavaScript代码的时候分为两步:预解析和代码执行。

- **「预解析」**在当前作用域下，JS代码执行之前，浏览器会默认把带有 ==var==和 ==function==声明的变量在内存中进行提前声明或定义。
- **「代码执行」**从上往下执行JS语句

预解析会把变量和函数的声明在代码执行之前完成，预解析也叫做变量、函数提升。

**「变量预解析(变量提升)」** 变量的声明会被提升到当前作用域的最上面，变量的赋值不提升。

```javascript
console.log(num);  // 结果是多少？
var num = 10;      // ？

相当于
var num;
console.log(num);// 结果是 undefined
num = 10;
结果: undefined
注意: 变量提升只提升声明，不提升赋值。
```

**「函数预解析(函数提升)」** 函数的声明会被提升到当前作用域的最上面，但是不会调用函数。

```javascript
fn();
function fn() {
  console.log('打印');
}
结果: 控制台打印字符串 --- "打印"

以上相对于
function fn(){
  console.log('打印');
}						//函数声明提升到最前面
fn();
注意: 函数声明代表函数整体，所以函数提升后，函数名代表整个函数，但是函数并没有被调用！
```

**「函数表达式声明函数问题」**

```javascript
函数表达式创建函数，会执行变量提升，此时接收函数的变量名无法正确的调用
fn();
var fn = function(){
  console.log("想不到吧");
}
结果:报错提示 "fn is not a function"
解释: 该段代码执行之前，会做变量声明提升，fn在提升之后的值
是undefined;而fn调用是在fn被赋值为函数体之前，
此时fn的值是undefined，所以无法被调用。
预解析案例1
var num = 10;
fun();

function fun(){
  console.log(num);
  var num = 20;
}

相当于执行了以下操作 结果打印 undefined
var num;

function fun(){
  var num;
  console.log(num);
  num = 20;
}
num = 10;
fun(); 


预解析案例2
var a = 18;
f1();

function f1(){
  var b = 9;
  console.log(a);
  console.log(b);
  var a = '123';
}

相当于执行了以下操作 结果为 undefined 9
var a;
function f1(){
  var b;
  var a;
  b = 9;
  console.log(a);
  console.log(b);
  a = '123';  
}
a = 18;
f1();  


预解析案例3
f1();
console.log(c);
console.log(b);
console.log(a);

function f1() {
  var a = b = c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}

相当于执行了以下操作 结果为 9 9 9 9 9 "报错--a is not defined"
function f1() {
  var a;
  a = b = c = 9;
  //相当于 var a = 9; b=9; c=9; b和c 直接赋值，没有var声明，当全局变量看。
  // 差异：集体声明 var a = 9,b = 9, c = 9;
  console.log(a);
  console.log(b);
  console.log(c);
}
f1();
console.log(c);
console.log(b);
console.log(a);
```



## 八、对象

### 8.1 对象的分类

1.内置对象

- 由ES标准中定义的对象，在任何的ES的实现中都可以使用

- 比如，`Math`  `String`  `Number`  `Boolean`  `Function`  `Object`  `.....`


2.宿主对象

​					-由JS的运行环境提供的对象，目前来讲主要指由浏览器提供的对象

​					--比如  `BOM` ` DOM`

3.自定义对象

​					-由开发人员自己创建的对象。

### 8.2 对象的概念

**「对象」**在JavaScript中，对象是一组无序的相关属性和方法的集合，所有的事物都是对象，例如字符串、数值、数组、函数等

对象是由属性和方法组成的

属性：事物的特征，在对象中用属性来表示（常用名词）

方法：事物的行为，在对象中常用方法来表示（常用动词）

**「为什么需要对象」**

保存一个值时，可以使用变量，保存多个值（一组值）时，可以使用数组，如果保存一个的完整信息呢？

为了更好地存储一组数据，对象应用而生；对象中为每项数据设置了属性名称，可以访问数据更语义化，数据结构清晰，表意明显，方便开发者使用。

```javascript
var obj = {
  "name":"fan",
  "sex":"male",
  "age":18,
  "height":155
}
```

### 8.3 创建对象的三种方式

#### 1. 利用字面量创建对象

使用对象字面量创建对象：

就是花括号 { } 里面包含了表达这个具体事物（对象）的属性和方法；{ } 里面采取键值对的形式表示
-`键`：相当于`属性名`
-`值`：相当于`属性值`，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）

```javascript
// star 就是创建的对象
var star = {
  name : 'pink',
  age : 18,
  sex : '男',
  sayHi : function() {
    alert('大家好啊');
  }
};
```

> 对象的使用

==对象的属性==：对象中`存储具体数据`的“键值对”中的键称为对象的属性，即对象中存储具体数据的项。

==对象的方法==：对象中`存储函数`的“键值对”中的“键”称为对象的方法，即对象中存储函数的项。

**1.向对象中添加属性**

语法：

```javascript
对象.属性名 = 属性值;
对象[“属性名”] = 属性值;       //这种方式能够使用特殊的属性名
```

==注意：==针对`对象["属性名"]=属性值`这种方法要说一下：

---使用[]这种形式去操作属性，**更加灵活**。

​	在[]中可以直接传递一个变量，这样变量是多少就会去读取那个属性。

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
//以下两个对比区别
console.log("使用对象['属性名']的结果：" + p.i);   //使用对象['属性名']的结果：undefined
console.log("使用对象['属性名']的结果：" + p[i]);  //使用对象['属性名']的结果：xiaoyang
</script>
```

！！！这个在后面javascript高级会再一次提。

**2.访问对象的属性**

语法:

```javascript
对象.属性名
对象[“属性名”];     //“属性名”可以s字符串常量，也可以是字符串变量
```

如果读取一个对象中没有的属性，它不会报错，而是返回一个undefined

**3.动态删减属性**

通过delete删除对象属性。

语法：

```javascript
delete 对象.属性名  
delete 对象["属性名"]
```

**4.调用对象的方法**

语法：

```javascript
对象.方法名()；
```

**变量、属性、函数、方法总结：**

`①变量`：单独声明赋值，单独存在
`②属性`:对象里面的变量称为属性，不需要声明，用来描述该对象的特征。
`③方法`：方法是对象的一部分，函数不是对象的一部分，函数是单独封装操作的容器。对象里面的函数称为方法，方法不需要声明，使用"对象.方法名()"的方式就可以调用，方法用来描述该对象的行为和功能。
`④函数`:单独存在的，通过"函数名()"的方式就可以调用。

```javascript
console.log(star.name)     // 调用名字属性
console.log(star['name'])  // 调用名字属性

star.sayHi();
```



#### 2. 利用new Object创建对象

创建空对象

```javascript
通过内置构造函数Object创建对象，此时andy变量已经保存了创建出来的空对象
var andy = new Object();  
```

给空对象添加属性和方法

```javascript
通过对象操作属性和方法的方式，来为对象增加属性和方法
andy.name = 'pink';
andy.age = 18; // andy.age = 19修改对象属性
andy.sex = '男；// andy.phoneNum = 110 添加属性
andy.sayHi = function() {
  alert('大家好');
}
obj.sayHi();调用对象的方法 //第二种写法 obj['sayHi']();

// Object()第一个字母大写；
//new Object() 需要new关键字，使用的格式:对象.属性 = 值
```



#### 3. 利用构造函数创建对象

`构造函数`是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值,它总与new运算符一起使用，我们可以把对象中一些公共的属性和方法抽出来，然后封装到这个函数里面。

**一个构造函数我们也可以称为一个类**

构造函数就是一个普通的函数，只是他的调用方式不同，
如果直接调用，它就是一个普通函数
如果使用new来调用，则它就是一个构造函数

构造函数的封装格式：

```javascript
function 构造函数名(形参1，形参2，形参3...) {
  this.属性名1 = 参数1;
  this.属性名2 = 参数2;
  this.属性名3 = 参数3;
  this.方法名 = 函数体;
}
```

构造函数的调用格式

```javascript
var obj = new 构造函数名(实参1，实参2，实参3);
// 以上代码中，obj即接收到构造函数创建出来的对象。
注意事项：
1.构造函数约定首字母大写
2.函数内的属性和方法前面需要添加this，表示当前对象的属性和方法
3.构造函数中不需要retrun返回结果
4.但我们创建对象的时候，必须用new 来调用构造函数

1.其他：构造函数如Stars(),抽取了对象的公共部分，封装到了函数里面，它
泛指某一大类(class)
2.创建对象，如new Stars();特指某一个，利用new关键字创建对象的过程
我们也称为对象实例化

```

示例：

```javascript
//构造函数
function Person(uname, age, socre) {
  this.name = uname;
  this.ages = age;
  this.socres = socre;
  this.fangfa = function (a) {
    console.log(this);    //parson
  }
  this.s = function () {
    alert('gun');
  }
  // console.log(this);    //parson
}
var person = new Person("yangyeyong", 22, 100);
console.log(person.fangfa());
```

**new关键字的作用(面试题)**

1.在构造函数代码开始执行之前，创建一个空对象；

2.修改this的指向，把this指向创建出来的空对象；

3.执行构造函数内的代码，给这个新对象添加属性和方法

4.在函数完成之后，返回这个创建出来的新对象(所以构造函数里面不需要return)

```javascript
// 工厂函数创建对象 这个把创建好的对象返回给函数调用处
function createPerson(name, age, job) {
  var person = new Object();
  person.name = name;
  person.age = age;
  person.job = job;
  person.sayHi = function(){
    console.log('Hello,everyBody');
  }
  return person;
}
var p1 = createPerson('张三', 22, 'actor');
```

------



### 8.4 检查一个对象是否是一个构造函数的示例

使用`instanceof`可以检查一个对象是否是一个类的实例。

语法：

```javascript
对象 instanceof 构造函数
```

如果是则返回true，否则返回false。

==**Object是所有对象的祖先，所以任何对象和Object做instanceof都会返回true**==

示例：

```javascript
var xiaoming = {
  name: '小明',
  birth: 1990,
  school: 'No.1 Middle School',
  height: 1.70,
  weight: 65,
  score: null
};
结果
console.log(xiaoming instanceof Object);  //true
```



### 8.5 检测对象是否拥有某一属性

用`in`操作符

```javascript
var xiaoming = {
  name: '小明',
  birth: 1990,
  school: 'No.1 Middle School',
  height: 1.70,
  weight: 65,
  score: null
};
console.log('home' in xiaoming);   //false
console.log('name' in xiaoming);   //true
```

==注意==：如果`in`判断一个属性存在，这个属性不一定是**它自己所拥有的**，而是可能`继承`的来的。

例如

```javascript
‘toString’ in xiaoming   //true
这里要说一下为什么'toString'会存在，原因是改方法存在原型对象中，通过继承得过来的。
如下检测：
 console.log(xiaoming.__proto__.hasOwnProperty('toString')); //ture
```

### 8.6 遍历对象

for...in 语句用于对`数组`或者`对象的属性`进行循环操作。

其语法如下：

```javascript
for (变量 in 对象名字) {
  // 在此执行代码
}
每次执行时，会将对象中的一个属性名赋值给变量。
语法中的变量是自定义的，它需要符合命名规范，通常我们会将这个变量写为 k 或者 key。

for (var k in obj) {
  console.log(k);      // 这里的 k 是属性名
  console.log(obj[k]); // 这里的 obj[k] 是属性值
}
```

### 8.7 原型（prototype）

创建的每一个函数，**解析器都会向函数中添加一个属性`prototype`**

prototype属性对应着一个对象，这个对象我们称为原型对象。   **理解：（prototype属性的值是一个原型对象的地址）**

如果函数作为普通函数调用prototype没有任何作用

当函数作为`构造函数`的形式调用时，**它所创建的对象中都会有一个隐含的属性执行改原型对象。**

```javascript
这个隐含的属性可以通过   对象.__proto__  来访问。
```

**原型对象就相当于一个公共的区域，凡是通过同一个构造函数创建的对象他们通常都可以访问到相同的原型对象。**

我们可以将对象中共有的属性和方法统一设置到原型对象中。这样我们只需要添加一次，就可以使所有的对象都可以使用。

示例：

![image-20210405161241794](D:\Typora记录文档\img\image-20210405161241794.png)



原型对象也是对象，所以它也有原型，当我们使用一个对象的属性或方法时，会先在自身中寻找，自身中如果有，则直接使用，如果没有则取原型对象中寻找，如果原型对象中有，则使用，如果还是没有，则继续去原型的原型中寻找,直到找到**Object对象的原型（即对象的组先）**，Object对象的原型没有原型，如果在Object中依然没有找到，则返回undefined。（**undefined的意思时原型对象中也没有这个属性）**

示例：

```javascript
function Myclass() {

}
var mc = new Myclass();
console.log(mc.__proto__.__proto__.hasOwnProperty("hasOwnProperty"));  //true
console.log(mc.__proto__.__proto__.__proto__);  //null
```

==用**hasOwnProperty()**判断属性是这个对象自身拥有的==

```javascript
xiaoming.hasOwnProperty('name');
>true
xiaoming.hasOwnProperty('toString');
>false
```

### 8.8 垃圾回收（GC）

在JS中拥有自动的垃圾回收机制，会自动将这些垃圾对象从内存中销毁，我们不需要也不能进行垃圾回收的操作我们需要做的只是要**将不再使用的对象设置null即可。**

示例：

```javascript
var obj =new Object();
obj=null;  //这样就就能垃圾回收
```



## 九、内置对象

**「内置对象」**JavaScript 中的对象分为3种：自定义对象 、内置对象、 浏览器对象
前面两种对象是JS基础内容，属于ECMAScript;第三个浏览器对象属于JS独有的，JS API讲解内置对象就是指js语言自带的一些对象，这些对象供开发者使用，并提供了一些常用的或是最基本而非必要的功能(属性和方法),内置对象最大的优点就是帮助我们快速开发。

**「查文档」**学习一个内置对象的使用，只要学会其常用成员的使用即可，我们可以通过查文档学习。
`MDN:`https://developer.mozilla.org/zh-CN/



### 9.1 Math对象

**Math对象不是构造函数，它具有数学常数和函数的属性和方法，跟数学相关。**

| 属性、方法名                              | 功能                                       |
| :---------------------------------------- | :----------------------------------------- |
| Math.PI                                   | 圆周率                                     |
| ==Math.floor()==     ---也叫地板函数。    | 向下取整                                   |
| ==Math.ceil()==       ---也叫天花板函数。 | 向上取整                                   |
| Math.round()                              | 四舍五入版 就近取整  注意 -3.5  结果是  -3 |
| Math.abs()                                | 绝对值                                     |
| Math.max()/Math.min()                     | 求最大和最小值                             |
| Math.random()                             | 获取范围在[0,1)内的随机值                  |

```javascript
- 获取指定范围的随机整数
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; 
}
```



### 9.2 日期对象

创建一个新`Date`对象的唯一方法是通过[`new`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new) 操作符，例如：`let now = new Date();`

Date 对象和 Math 对象不一样，**Date是一个构造函数**，所以使用时需要实例化后才能使用其中具体方法和属性。Date 实例用来处理日期和时间

**使用Date实例化日期对象**，获取当前时间必须实例化

获取指定时间的日期对象

>如果没有提供参数，那么新创建的Date对象表示实例化时刻的日期和时间。

```javascript
var now = new Date();     //Mon Mar 29 2021 13:06:38 GMT+0800 (中国标准时间)
```

>如果有参数，返回参数指定时间。

```javascript
var future = new Date('2020/10/1');   //Thu Oct 01 2020 00:00:00 GMT+0800 (中国标准时间)
```

续上

使用Date实例的方法和属性

getMonth()方法获取到的月份 + 1 = 当月![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1sR1a3mNu324E7ibqsxHGsJKbSsvwzJY5fZRsKAR47X9vSiczpmvhiaPJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
//参数常用写法 数字型或者字符串型 '2019-10-1 8:8:8'
var date1 = new Date(2019,10,1);
//日期格式化
// 格式化日期 年 月 日
var date = new Date();
console.log(date.getFullYear()); //返回当前日期的年 2020
console.log(date.getMonth() + 1); //月份 返回的月份小1个月，记得月份加1呦
console.log(date.getDate()); //返回的是 几号
console.log(date.getDay); //周一返回的是1 周六返回的是6 周日返回的是0
//我们写一个 2020年 9月 6日 星期日
var year = date.getFullYear();
var month = date.getMonth() + 1;
var dates = date.getDate();
var day = date.getDay();
if (day == 0) {
  day = "星期日";
}
console.log("今天是" + year + "年" + month + "月" + dates + "日" + day);
//格式化日期 时分秒
var date = new Date();
console.log(date.getHours()); //时
console.log(date.getMinutes()); //分
console.log(date.getSeconds()); // 秒

//封装一个函数返回当前的 时 分 秒 格式 08:08:08
function getTimer() {
  var time = new Date();
  var h = time.getHours();
  var h = h < 10 ? "0" + h : h;

  var m = time.getMinutes();
  var m = m < 10 ? "0" + m : m;

  var s = time.getSeconds();
  var s = s < 10 ? "0" + s : s;
  return h + ":" + h + ":" + s;
}
console.log(getTimer());
```

`获取Date日期总的毫秒数(时间戳)`
基于1970年1月1日(世界标准世界)起的毫秒数

```javascript
// 实例化Date对象
var now = new Date();
// 1. 用于获取对象的原始值
console.log(now.valueOf()) 
console.log(now.getTime()) 

// 2. 简单写可以这么做 (最常用的)
var now = + new Date();   

// 3. HTML5中提供的方法，有兼容性问题
var now = Date.now();
倒计时案例 ：
1. 输入的时间减去现在的时间就是剩余的时间，即倒计时。
2.用时间戳来做，用户输入时间总的毫秒数减去现在时间的总的毫秒数，
得到的就是剩余时间的毫秒数
3.把剩余时间总的毫秒数转换为天、时、分、秒  (时间戳转换时分秒)
转换公式如下：
d = parseInt(总秒数/60/60/24) // 计算天数
h = parseInt(总秒数/60/60) // 计算小时
m = parseInt(总秒数/60%60);   // 计算分钟  
s = parseInt(总秒数%60);      // 计算当前秒数 
// 倒计时案例 封装函数实现
function countDown(time) {
  var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
  var inputTime = +new Date(time); // 返回的是用户输入时间总的毫秒数
  var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数
  var d = parseInt(times / 60 / 60 / 24); // 天
  d = d < 10 ? "0" + d : d;
  var h = parseInt((times / 60 / 60) % 24); //时
  h = h < 10 ? "0" + h : h;
  var m = parseInt((times / 60) % 60); // 分
  m = m < 10 ? "0" + m : m;
  var s = parseInt(times % 60); // 当前的秒
  s = s < 10 ? "0" + s : s;
  return d + "天" + h + "时" + m + "分" + s + "秒";
}
console.log(countDown("2020-10-1 18:00:00"));
var date = new Date();
console.log(date);
```



># +new Date()

**js在某个数据类型前使用‘+’，这个操作目的是为了将该数据类型转换为Number类型，如果转换失败，则返回NaN;**

```javascript
var i = +new Date();
console.log(i);
console.log(typeof i);  //由原来的Object类型转换为了number类型。
var b = +new Date();
console.log(b - i);
```



### 9.3 数组对象

#### 创建数组的两种方式

**1. 字面量方式**                                   `var arr = [1,"test",true];`

**2. 实例化数组对象 new Array()**    `var arr = new Array();`

注意：上面代码中arr创建出的是一个空数组，如果需要使用构造函数Array创建非空数组，可以在创建数组时传入参数

​		如果只传入一个参数(数字)，则参数规定了数组的长度。

​		如果传入了多个参数，则参数称为数组的元素。



#### 检测是否为数组

**1. instanceof 运算符**

instanceof 可以判断一个对象是否是某个构造函数的实例

```javascript
var arr = [1, 23];
var obj = {};
console.log(arr instanceof Array); // true
console.log(obj instanceof Array); // false
```

**2. Array.isArray()**

Array.isArray()用于判断一个对象是否为数组，isArray() 是 HTML5 中提供的方法

```javascript
var arr = [1, 23];
var obj = {};
console.log(Array.isArray(arr));   // true
console.log(Array.isArray(obj));   // false
```

**3. 注意 typeof用法**

typeof 用于判断变量的类型

```javascript
var arr = [1, 23];
console.log(typeof arr) // object 对象arr是构造函数的实例因此是对象数据类型
```



#### 添加删除数组元素的方法

数组中有进行增加、删除元素的方法，部分方法如下表

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1gibK1W7qC7oQGanLYkTicAxTOibs48z6G36etWUAz6uCxkBnzU4Nib12Sg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
var arr = [1, 2, 3];
console.log(arr.push(4, 5)); // 5 向数组末尾添加元素
arr.pop(); //删除数组最后一个值并返回
console.log(arr); // [1,2,3,4]

// 向数组的开头添加元素并返回数组长度 
console.log(arr.unshift(5, 6)); // 6  数组变为[5,6,1,2,3,4]
console.log(arr.shift()); // 5 删除数组开头的元素并返回该值
```



#### 数组排序

数组中有对数组本身排序的方法，部分方法如下表

| 方法名    | 说明                         | 是否修改原数组                    |
| :-------- | :--------------------------- | :-------------------------------- |
| reverse() | 颠倒数组中元素的顺序，无参数 | 该方法会改变原来的数组 返回新数组 |
| sort()    | 对数组的元素进行排序         | 该方法会改变原来的数组 返回新数组 |

注意：**sort方法需要传入参数(函数)来设置升序、降序排序**

​			如果传入“function(a,b){ return a-b;}”，则为升序

​			如果传入“function(a,b){ return b-a;}”，则为降序

```javascript
// 踩坑 数组sort()排序(冒泡排序) return a - b 则升序
// 写法固定 参考如下
var arr1 = [13,4,77,1,7];
arr1.sort(function(a,b){
  return a-b;
});
console.log(arr1);
```



#### 数组索引方法

数组中有获取数组指定元素索引值的方法，部分方法如下表

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg15icchjhbHfYwwL5A1w4xD4Jxv1wrT1FfQImAeDUbpR5pMvwEiawe2ibbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
var arr = [1, 2, 3, 4, 5, 4, 1, 2];
// 查找元素2的索引
console.log(arr.indexOf(2)); // 1
// 查找元素1在数组中的最后一个索引
console.log(arr.lastIndexOf(1)); // 6
```



#### 数组转换为字符串

数组中有把数组转化为字符串的方法，部分方法如下表

注意：join方法如果不传入参数，则按照 “ , ”拼接元素![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1ibSEpz3vpxrfDTzWzLcVHbAKCBlxjOzb7QOgjocc7ibbo0iczhzqYOoAg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
var arr = [1, 2, 3, 4];
var arr2 = arr;
var str = arr.toString(); // 将数组转换为字符串
console.log(str); // 1,2,3,4

var str2 = arr2.join("|");//按照键入字符将数组转换为字符串
console.log(str2);
//结果：1|2|3|4
```

#### 其他方法

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1oYBMqkSHVZds8w2DFshX2571AaFcBLicNLBlMa0OMibomZBQelV070TQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
var arr = [1, 2, 3, 4];
var arr2 = [5, 6, 7, 8];
var arr3 = arr.concat(arr2);
console.log(arr3); // [1,2,3,4,5,6,7,8]

// slice(begin,end) 是一种左闭右开区间 [1,3)
// 从索引1出开始截取，到索引3之前
var arr4 = arr.slice(1, 3);
console.log(arr4); // [2,3]
```

#### splice()    重要！

`splice()`方法是修改`Array`的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：

参数： 
   第一个：删除开始位置的索引
   第二个：删除的个数 
   第三个：三个以后，都是替换的元素，这些元素将会插入到开始位置索引的前边  

```javascript
// 6.splice万能方法
// 参数：
// 第一个：删除开始位置的索引
// 第二个：删除的个数
// 第三个：三个以后，都是替换的元素，这些元素将会插入到开始位置索引的前边
var arr = ["杨叶永", "杨青青", "杨秀琪", "易光楷", "杨文伟"];
//一个参数
result = arr.splice(1);
console.log(result);  //["杨青青", "杨秀琪", "易光楷", "杨文伟"]
console.log("原数组的结果：" + arr);  //原数组的结果：杨叶永
//两个参数
result = arr.splice(0, 2);
console.log(result);   // ["杨叶永", "杨青青"]
console.log("原数组的结果：" + arr);  //原数组的结果：杨秀琪,易光楷,杨文伟
//三个参数
//删除后替换
result = arr.splice(1, 3, "赵裕鑫", "孙悟空", "猪八戒");
console.log(result);  //["杨青青", "杨秀琪", "易光楷"]
console.log("原数组的结果：" + arr);   // 原数组的结果：杨叶永,赵裕鑫,孙悟空,猪八戒,杨文伟
//不删除替换
result = arr.splice(1, 0, "赵裕鑫", "孙悟空", "猪八戒");
console.log(result); //[]
console.log("原数组的结果：" + arr);  //原数组的结果：杨叶永,赵裕鑫,孙悟空,猪八戒,杨青青,杨秀琪,易光楷,杨文伟

```

==splice()会对原数组产生影响！！！==



#### from()方法 (ES6)

`Array.from()` 方法从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

```javascript
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]

Reset
> Array ["f", "o", "o"]
> Array [2, 4, 6]
```



### 9.4 基本包装类型

为了方便操作基本数据类型，JavaScript 还提供了三个特殊的引用类型：String、Number和 Boolean。
  `基本包装类型就是把简单数据类型包装成为复杂数据类型`，这样基本数据类型就有了`属性`和`方法`。

```javascript
// 下面代码有什么问题？
var str = 'andy';
console.log(str.length); // 4
```

  按道理基本数据类型是没有属性和方法的，而对象才有属性和方法，但上面代码却可以执行，这是因为 js 会把基本数据类型包装为复杂数据类型，其执行过程如下 ：

```javascript
// 1. 生成临时变量，把简单类型包装为复杂数据类型
var temp = new String('andy');
// 2. 赋值给我们声明的字符变量
str = temp;
// 3. 销毁临时变量
temp = null;
```



### 9.5 字符串对象

#### 字符串的不可变

​			指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间。

​			当重新给字符串变量赋值的时候，变量之前保存的字符串不会被修改，依然在内存中重新给字符串赋值，会重新在内存中开辟空间，这个特点就是==字符串的不可变==。

​			由于字符串的不可变，在==大量拼接字符串==的时候会有效率问题



#### 根据字符返回位置

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是返回指定字符的位置的方法：![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg19on1l42TIwA7hXGXUQu4EVPwGdRibsibh2Tcyndq0eM0icBYlyibUd1nhA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> indexOf("要查找的字符串的值"，开始位置);

```javascript
var str = "anndy";
console.log(str.indexOf("d")); // 3
//指定从索引号为4的地方开始查找字符"d"
console.log(str.indexOf("d", 4)); // -1

console.log(str.lastIndexOf("n")); // 2
```

案例：查找字符串"abcoefoxyozzopp"中所有o出现的位置以及次数

1. 先查找第一个o出现的位置
2. 然后 只要indexOf 返回的结果不是 -1 就继续往后查找
3. 因为indexOf 只能查找到第一个，所以后面的查找，利用第二个参数，当前索引加1，从而继续查找

```javascript
var str = "oabcoefoxyozzopp";
var index = str.indexOf("o");
var num = 0;
while (index !== -1) {
  console.log(index);
  num++;
  index = str.indexOf("o", index + 1);   //第二参数为开始的位置。
}
```

**「根据位置返回字符」**

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是根据位置返回指定位置上的字符：![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1d4P0xqicUEsFsqZQYyS1sNKn1sQYRwkrPetCzcszdjSktgz7UEzpdbA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```javascript
// 根据位置返回字符
// 1. charAt(index) 根据位置返回字符
var str = 'andy';
console.log(str.charAt(3)); // y
// 遍历所有的字符
for (var i = 0; i < str.length; i++) {
  console.log(str.charAt(i));
} // a n d y

// 2. charCodeAt(index)  
//返回相应索引号的字符ASCII值 目的： 判断用户按下了那个键 
console.log(str.charCodeAt(0)); // 97
// 3. str[index] H5 新增的
console.log(str[0]); // a
```

案例：判断一个字符串 'abcoefoxyozzopp' 中出现次数最多的字符，并统计其次数

核心算法：利用 charAt(） 遍历这个字符串

2. 把每个字符都存储给对象， 如果对象没有该属性，就为1，如果存在了就 +1
3. 遍历对象，得到最大值和该字符 注意：在遍历的过程中，把字符串中的每个字符作为对象的属性存储在对象中，对应的属性值是该字符出现的次数

```javascript
var str = "abcoefoxyozzopp";
var o = {};
for (var i = 0; i < str.length; i++) {
  var chars = str.charAt(i); // chars 是 字符串的每一个字符
  if (o[chars]) {
    // o[chars] 得到的是属性值   //不是有效的变量名必须用：对象[‘特殊变量名’]
    o[chars]++;
  } else {
    o[chars] = 1;     //赋值，相对于o.chars=1;只不过这里的属性名不是有效的，所以只能用对象[‘特殊变量名’]这种形式。
  }
}
console.log(o);
// 2. 遍历对象
var max = 0;
var ch = "";
for (var k in o) {
  // k 得到是 属性名
  // o[k] 得到的是属性值
  if (o[k] > max) {
    max = o[k];
    ch = k;
  }
}
console.log(max);
console.log("最多的字符是" + ch);
```



#### 字符串操作方法

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是部分操作方法：

![image-20210405222538562](D:\Typora记录文档\img\image-20210405222538562.png)

concat()方法:

```javascript
// 字符串操作方法
// 1. concat('字符串1','字符串2'....)
var str = 'andy';
console.log(str.concat('red')); // andyred
```

substr('截取的起始位置','截取字符个数'):

```javascript
// 2. substr('截取的起始位置', '截取几个字符');
var str1 = '改革春风吹满地';
// 第一个2 是索引号的2 从第几个开始  第二个2 是取几个字符
console.log(str1.substr(2, 2)); // 春风
```

replace()方法:

replace() 方法用于在字符串中用一些字符替换另一些字符，其使用格式如下：

```javascript
字符串.replace(被替换的字符串， 要替换为的字符串)；   //改方法一次只能替换一个字符
```

==调用改方法，原字符串不会改变。==

示例：

```javascript
// 1. 替换字符 replace('被替换的字符', '替换为的字符')  它只会替换第一个字符
var str = "andyandy";
console.log(str.replace("a", "b")); // bndyandy

// 有一个字符串 'abcoefoxyozzopp'  要求把里面所有的 o 替换为 *
var str1 = "abcoefoxyozzopp";
while (str1.indexOf("o") !== -1) {
  str = str.replace("o", "*");
  str = str.replace("a", "");  //如果替换新的内容为空，则是相对于删除字符。  //ndyndy
}
console.log(str); // abc*ef*xy*zz*pp
```



#### #split()方法

> split()方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，**返回的是一个新数组。**

其使用格式如下：

```javascript
//字符串.split("分割字符")

字符转换为数组 split('分隔符')    
// 前面我们学过 join 把数组转换为字符串
var str2 = "red, pink, blue";
console.log(str2.split(",")); //[red,pink,blue]
var str3 = "red&pink&blue";
console.log(str3.split("&")); // [red,pink,blue]
```

如果split()方法的参数为空，则会将==每个字符拆分为数组中的一部分==

```javascript
var str="adsdasdasd";
var result=str.split("");
console.log(result);   //["a", "d", "s", "d", "a", "s", "d", "a", "s", "d"]
```



#### #trim()方法

该**`trim()`**方法从字符串的两端删除空格。

示例：

```js
const greeting = '   Hello world!   ';

console.log(greeting);
// expected output: "   Hello world!   ";

console.log(greeting.trim());
// expected output: "Hello world!";

```

该方法返回的是一个删除开头和结尾空格的新字符串。



## 十、简单数据类型和复杂数据类型

**「简单类型（基本数据类型、值类型）」**：在存储时变量中存储的是值本身，包括string ，number，boolean，undefined，null

**「复杂数据类型（引用类型）」**：在存储时变量中存储的仅仅是地址（引用），通过 new 关键字创建的对象（系统对象、自定义对象），如 Object、Array、Date等；

**「堆栈」**

堆栈空间分配区别：

1、栈（操作系统）：由操作系统自动分配释放存放函数的参数值、局部变量的值等。其操作方式类似于数据结构中的栈；

**简单数据类型存放到栈里面**

2、堆（操作系统）：存储复杂类型(对象)，一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收。

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1jzx4J8oTMHhE7wH3Gibe6Vm3MwwZic3NNbfGpzSx3ej3e61ekggPmWwg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 10.1 简单数据类型的存储方式

值类型变量的数据直接存放在变量（栈空间）中

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1ZqBzWrBbOHPKkEdUkj6W9P47IhaBkGLLtaZk6gMBKO5f1zibu2GmvDg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### 10.2 复杂数据类型的存储方式

引用类型变量（栈空间）里存放的是地址，真正的对象实例存放在堆空间中

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg1qSYCNHocgsPF5gforAfHojic5W3v14jBQY4w8TREtvysUm82mD79Nww/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![image-20210404163558119](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210404163558119.png)



### 10.3 简单类型传参

  函数的形参也可以看做是一个变量，当我们把一个值类型变量作为参数传给函数的形参时，其实是把变量在栈空间里的值复制了一份给形参，那么在方法内部对形参做任何修改，都不会影响到的外部变量。

```javascript
function fn(a) {
  a++;
  console.log(a); 
}
var x = 10;
fn(x);
console.log(x)；
```

运行结果如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg19jHibmWpibic6PTBoJTpvjR84jdiczZLTSRn8TWC1rTNq7EQHqRbu0lBAA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



### 10.4 复杂数据类型传参

  函数的形参也可以看做是一个变量，当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给了形参，**形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象。**

```javascript
function Person(name) {
  this.name = name;
}
function f1(x) { // x = p
  console.log(x.name); // 2. 这个输出什么 ?    
  x.name = "张学友";
  console.log(x.name); // 3. 这个输出什么 ?    
}
var p = new Person("刘德华");
console.log(p.name);    // 1. 这个输出什么 ?   
f1(p);
console.log(p.name);    // 4. 这个输出什么 ?  
```

运行结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/y7EkeCWAzmruAmtJf4nict04DRBVU4Jg17qicFrVibtHDx7pSYFvXPnVoQqGfAHOwtn877LCFBhaYP5hNQpWxALXA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



## 十一、正则表达式

正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript中，正则表达式也是对象。

这个正则表达式可以用来检查一个字符串中是否某些字符。

这些模式被用于 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp) 的 [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 和 [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 方法, 以及 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String) 的 [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)、[`matchAll`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)、[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)、[`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) 和 [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法。



JavaScript有两种方式创建一个正则表达式：

第一种方式：通过`new RegExp('正则表达式')`创建一个RegExp对象。

第二种方式：直接通过`/正则表达式/`写出来

==两则区别：使用构造函数的方式创建更加灵活，使用字面量的方式创建更加简单。==

### 11.1 new RegExp 

==语法：var reg=new RegExp("正则表达式","匹配模式");==

**在构造函数中可以传递一个匹配模式作为第二个参数。**

​	**i**		忽略大小写

​	**g**		全局匹配模式

设置匹配模式时，可以都不设置，也可以设置1个，也可以全设置，设置时没有顺序要求

```javascript
var reg = new RegExp("a","i");
//使用typeof检查正则对象，会返回Object
console.log(typeof reg);    //object
```

**正则表达式的test()方法：**

使用这个方法可以用来检查一个字符串是否符合正则表达式的规则。如果符合则返回true，否则返回false。

```javascript
var reg = new RegExp("a","i");
var str = "a";
console.log(typeof reg); //object
// var result = reg.test(str);  //true
// var result = reg.test("a");  //true
// var result = reg.test("bbcacc"); //ture
// var result = reg.test("bbcAcc"); //ture
// var result = reg.test("bbccdd");  //false   说明不含有字符a
console.log(result);
```

### 11.2 通过字面量创建正则表达式

==语法：var 变量 =/正则表达式/匹配模式==

如上例子用字面量方式改写：

```javascript
var reg = /a/i;
var result = reg.test("ASDASD");  //ture
console.log("字面量方式创建的结果：" + result);
```



#### 字符

`[]`:[]里面内容是或的关系。

```javascript
[ab]==a|b;
[a-z]:任意小写字母
[A-Z]:任意大写字母
[A-z]:任意字母
[0-9]:任意数字
[^ ]:除了
\w:任意字母、数字、_ 相当于[A-z0-9_];
\W:除了字母、数字、_ 相当于[^A-z0-9_];
\d:任意的数字   		相当于[0-9];
\D:除了数字[^0]    相当于[^0-9];
\s空格
\S:除了空格
\b:单词边界
\B:除了单词边界  
```

#### 字符串和正则表达式的方法

> 1.split()

-可以将一个字符串拆分为一个数组。

-方法中可以传递一个==正则表达式==作为参数，这样方法将会根据正则表达式拆分字符串。

**split()方法即使不指定全局匹配，也会全都拆分**

```javascript
var str = "1a2b3c4d5e6f7";
var result = str.split(/[a-z]/);
var result1 = str.split(/[a-z]/i);
console.log(result1); //["1", "2", "3", "4", "5", "6", "7"]
console.log(result);  //["1", "2", "3", "4", "5", "6", "7"]

//result和result1结果一样说明即使不指定全局匹配，也会全都拆分
```

>2. search()

-可以搜索字符串中是否含有指定内容

-如果==搜索到指定内容，则返回第一次出现的索引，如果没有搜索到则返回-1==.

-它可以接收一个正则表达式作为参数，然后根据正则表达式去检索字符串。

==-search()只会查找第一个，即使设置全局匹配也没用。==

例子：

```javascript
var str = "hello abc hello aec hello afc";
var result = str.search(/a[bef]c/);
console.log(result);    //6
```

>3.match()    ---这个很实用，重点记住

-可以根据正则表达式，从一个字符串中==将符合条件的内容提取出来==。

-match()方法只会找到第一个符合要求的内容，找到以后就停止，所以需要设置==正则表达式为全局模式==。

**-match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果。**

例子：

```javascript
var str = "1a2b3c4d5e6f7";
var result = str.match(/[A-z]/);
var result1 = str.match(/[A-z]/g);
console.log(result); //["a"];
console.log(result1); // ["a", "b", "c", "d", "e", "f"]
```

>4. replace()

-可以将字符串中指定内容替换为新的内容。

-参数:1.被替换的内容，可以接收一个正则表达式作为参数。

​		   2.新的内容。

例子：

```javascript
var str = "1a2b3c4d5e6f7";
var result = str.replace(/[A-z]/g, "-");
console.log(result); //1-2-3-4-5-6-7
```

**如果替换新的内容为空，则是相对于删除字符。**

```javascript
var str = "1a2b3c4d5e6f7";
var result = str.replace(/[A-z]/g, "");
console.log(result); 		//1234567
```



#### 量词

-通过量词可以设置一个内容出现的次数。

-量词只对它前边的一个内容起作用。

{n}			正好出现n次

{m,n}		正好出现m至n次

这里解释一下{m,n}

```javascript
n 和 m 都是整数。匹配前面的字符至少n次，最多m次。
例如/a{3}/
匹配“caandy”中的前两个a，也匹配“caaaaaaandy”中的前三个a。
注意，当匹配”caaaaaaandy“时，匹配的值是“aaa”，即使原始的字符串中有更多的a。
//也就是说，当字符串中的查询的那个字符次数少于字符串中改字母出现的次数也不会报错。
```

{m，}		m次以上

＋号		  至少一个，相对于{1，}

-号			0个或多个，相对于{0，}

？			0个或1个，相对于{0，1}



#### 转义字符

在正则表达式中使用`\`作为转义字符

`\.`:表示 `.`

`\\`:表示`\`

示例：

```javascript
var reg = /\.\.\\/;  //.表示任意字符
console.log("打印reg：" + reg);  //结果是：打印reg：/\.\.\\/
console.log(reg.test("b..\\cs"));  //ture  在字符串中\表示转义字符
var reg1 = new RegExp("\\.");
console.log("打印：" + reg1);  //结果是/\./
console.log(reg1.test("asdas."));  //

！！！字面量中的/\\/等于 构造函数中的new RegExp("\\\\");
```

==注意：==`\`在字符串中是转义字符，**如果要使用\则需要使用`\\`来代替。**



例题：

```javascript
//用正则表达式去除空格
var str = "             he  llo                 ";
console.log("打印str：" + str);
var reg = /^\s*|\s*$/g;
var result = str.replace(reg, "");
console.log("替换后的结果：" + result);     //替换后的结果：he  llo
```



## 十二、JSON

JSON简写：JavaScript Object Notation，即：JS对象表示法，它是一种`数据交换格式`

### 12.1 JSON 格式

1.JSON中属性值允许的类型有：

![image-20210323160524142](https://cdn.jsdelivr.net/gh/threey333/Picture/js/image-20210323160524142.png)

==注意：==对象不能是函数、正则表达式对象、日期对象等等，只能是`普通对象`

2.JSON字符串中的**属性名必须加双引号**。

3.JSON分类：

1.对象{}   ----`JSON对象`

2.数字[]   ----`JSON数组`

### 12.2 JSON转换

**在JS中，有一个工具类，叫JSON。**

这个对象可以帮助我们将一个JSON转换位JS对象，也可以将一个JS对象转换为JSON。

#### JSON转对象

通过JSON的parse()方法。

```javascript
var obj = JSON.parse('{"name":"小明","age":14}', function (key, value) {
  if (key === 'name') {
    return value + '同学';
  }
  return value;
});
console.log(JSON.stringify(obj)); // {name: '小明同学', age: 14}
```



#### 对象转换为JSON

通过JSON的stringify()方法：

```javascript
var xiaoming = {
  name: '小明',
  age: 14,
  gender: true,
  height: 1.65,
  grade: null,
  'middle-school': '\"W3C\" Middle School',
  skills: ['JavaScript', 'Java', 'Python', 'Lisp']
};
var s = JSON.stringify(xiaoming);
console.log(s);

//{"name":"小明","age":14,"gender":true,"height":1.65,"grade":null,"middle-school":"\"W3C\" Middle School","skills":["JavaScript","Java","Python","Lisp"]}
```

要输出得好看一些，可以加上参数，按缩进输出：

```javascript
JSON.stringify(xiaoming, '  ');
```

结果：

```javascript
{
  "name": "小明",
  "age": 14,
  "gender": true,
  "height": 1.65,
  "grade": null,
  "middle-school": "\"W3C\" Middle School",
  "skills": [
    "JavaScript",
    "Java",
    "Python",
    "Lisp"
  ]
}
```

