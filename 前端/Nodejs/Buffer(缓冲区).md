# Buffer（缓冲区）

`Buffer` 类是 JavaScript [`Uint8Array`](http://url.nodejs.cn/ZbDkpm) 类的子类，并使用涵盖额外用例的方法对其进行扩展。 Node.js API 在支持 `Buffer` 的地方也接受普通的 [`Uint8Array`](http://url.nodejs.cn/ZbDkpm)。

虽然 `Buffer` 类在全局作用域内可用，但仍然建议通过 `import` 或 `require` 语句显式地引用它。

> 由于Buffer缓冲区知识点特别多，这里不全部描述。相关知识文档地址：http://nodejs.cn/api/buffer.html#buffer

进制知识图：

![image-20220131235340476](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220131235403.png)



### 一、字符编码

**当在 `Buffer` 和 `String` 之间进行转换时，可以指定字符编码。如果未指定字符编码，则默认使用UTF-8。**

NodeJS目前支持的字符编码有：

- `uft-8`：多字节编码的 Unicode 字符。 许多网页和其他文档格式使用 [UTF-8](http://url.nodejs.cn/mzW5jo)。 这是默认的字符编码。
- `'utf16le'`（别名：`'utf-16le'`）：多字节编码的 Unicode 字符。 与 `'utf8'` 不同，字符串中的每个字符都将使用 2 或 4 个字节进行编码。
- `latin1`：此字符编码仅支持 `U+0000` 至 `U+00FF` 的 Unicode 字符。 每个字符都使用单个字节进行编码。 不符合该范围的字符将被截断并映射到该范围内的字符。[有关Unicode字符列表地址](https://baike.baidu.com/item/Unicode%E5%AD%97%E7%AC%A6%E5%88%97%E8%A1%A8/12022016)

```js
import { Buffer } from 'buffer';

const buf3 = Buffer.from('hello world', 'latin1');

console.log(buf3) // => <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
```

- `base64`：就是Base64编码。
- `hex`：将每个字节编码为两个十六进制字符（即两个十六进制字符作为一个字节）。如果不是 `有效十六进制字符(123456789ABCDEF)`，那么就会截断。

```js
import { Buffer } from 'buffer';

Buffer.from('1ag', 'hex');
// 打印 <Buffer 1a>，当遇到第一个非十六进制值 ('g') 时，则截断数据。

Buffer.from('1a7g', 'hex');
// 打印 <Buffer 1a>，当数据以一位数 ('7') 结尾时，则截断数据。

Buffer.from('1634', 'hex');
// 打印 <Buffer 16 34>，表现所有数据。

Buffer.from('6e69ffaabb11136864651332131564646454646464546465487979741321', 'hex')
// 打印 <Buffer 6e 69 ff aa bb 11 13 68 64 65 13 32 13 15 64 64 64 54 64 64 64 54 64 65 48 79 79 74 13 21> 只要是有效十六进制字符，就不会被截断。
```

- `ascii`：仅适用于 7 位 `ASCII` 数据。 当将字符串编码为 `Buffer` 时，等效于使用 `latin1` 编码。

>说明：7位的ASCII数据包括：所有的大写和小写字母，数字0到9、标点符号，以及在美式英语中使用的特殊控制字符 。
>
>```js
>const buf = Buffer.from('runoob', 'ascii');
>// 等效于使用latin1编码
>const buf5 = Buffer.from('runoob', 'latin1')
>// 结果都是: <Buffer 72 75 6e 6f 6f 62>
>```

- `'binary' `:  `'latin1'` 的别名。



### 二、创建Buffer类

`Buffer类` 是直接处理二进制数据的全局类型。 它可以使用多种方式构建。

>注意：为了使 `Buffer` 实例的创建更可靠且不易出错，`new Buffer()` 构造函数的各种形式已被弃用，并由单独的 `Buffer.from()`、`Buffer.alloc()` 和 `Buffer.allocUnsafe()` 方法取代。

> **总结就是：以后创建Buffer类就使用下面这些新的API。**
>
> - ` Buffer.from(array)`：返回新的 `Buffer`，其中包含所提供八位字节的副本。
>- `Buffer.from(arrayBuffer[, byteOffset[, length]])` 返回新的 `Buffer`，其与给定的 `ArrayBuffer`共享相同的分配内存。
> - `Buffer.from(buffer)`：返回新的 `Buffer`，其中包含给定 `Buffer` 内容的副本。
>- `Buffer.from(string[, encoding])`：返回新的 `Buffer`，其中包含所提供字符串的副本。
> - `Buffer.alloc(size[, fill[, encoding]])`：返回指定大小的新初始化 `Buffer`。 此方法比`Buffer.allocUnsafe(size)`慢，但保证新创建的 `Buffer` 实例永远不会包含可能敏感的旧数据。 如果 `size` 不是数值，则会抛出 `TypeError`。
> - `Buffer.allocUnsafe(size)`和 [`Buffer.allocUnsafeSlow(size)`](http://nodejs.cn/api/buffer.html#static-method-bufferallocunsafeslowsize) 分别返回指定 `size` 的新的未初始化的 `Buffer`。 由于 `Buffer` 未初始化，分配的内存段可能包含潜在敏感的旧数据。
> 
> 
> 
> **注意：如果 `size` 小于或等于 `Buffer.poolSize`（默认值：8192） 的一半时：**
>
> - 则 `Buffer.allocUnsafe()` 和 `Buffer.from(array)` 返回的 `Buffer` 实例可以从共享的内部内存池中分配。 
>
> - `Buffer.allocUnsafeSlow()`返回的实例从不使用共享的内部内存池。



#### 2.1 Buffer.alloc(size[, fill[, encoding]])

参数说明：

- `size` <integer> 新的 `Buffer` 所需的长度。
- `fill` <String> | <Buffer> | <Uint8Array> | <integer> 用于预填充新 `Buffer` 的值。 **默认值:** `0`。
- `encoding` <string> 如果 `fill` 是字符串，则这就是它的编码。 **默认值:** `'utf8'`。

**返回值：**返回一个指定大小的 `Buffer` 实例，如果没有设置 fill，则默认填满 0。

示例一：`fill` 为undefined时，`Buffer` 将以零填充。

```js
import { Buffer } from 'buffer';

const buf = Buffer.alloc(5);

console.log(buf);
// 打印: <Buffer 00 00 00 00 00> =>此时编码默认使用的是'utf8'
```

示例二：`fill` 不为undefined，则分配的 `Buffer` 将通过调用 [`buf.fill(fill)` 进行初始化。

```js
import { Buffer } from 'buffer';

const buf = Buffer.alloc(5, 'a');  // =>相当于const buf = Buffer.alloc(5).fill('a')

console.log(buf);
// 打印: <Buffer 61 61 61 61 61>
```

示例三：同时指定了 `fill` 和 `encoding`，则分配的 `Buffer` 将通过调用 [`buf.fill(fill, encoding)` 进行初始化。

```js
import { Buffer } from 'buffer';

const buf = Buffer.alloc(8, '123456abcd', 'base64')

console.log(buf) // => <Buffer d7 6d f8 e7 a6 9b 71 d7>
// 相当于
const buf1 = Buffer.alloc(8).fill('123456abcd', 'base64')

console.log(buf1) // => <Buffer d7 6d f8 e7 a6 9b 71 d7>
//两者结果是一样的。
```

示例四：如果 `size` 不是数值，则会抛出 `TypeError`。

```js
import { Buffer } from 'buffer';

const buf = Buffer.alloc('a')

console.log(buf) // => The "size" argument must be of type number. Received type string ('a')
```



#### 2.2 Buffer.allocUnsafe(size)

参数说明：

- `size` <integer> 新的 `Buffer` 所需的长度。

**返回值：**返回一个指定大小的 `Buffer` 实例，但是它不会被初始化，所以它可能包含敏感的数据。

如果 `size` 大于Buffer的最大长度或小于0，则抛出 `ERR_INVALID_ARG_VALUE` 的错误。

示例：

```js
import { Buffer } from 'buffer';

const buf = Buffer.allocUnsafe(10)

console.log(buf) // => <Buffer 68 20 9c f4 8e 02 00 00 0a 00>

// 由于Buffer.allocUnsafe(size)这个方法返回的Buffer实例不会被初始化，所以需要fill或write重写
buf.fill(0)

console.log(buf) // => <Buffer 00 00 00 00 00 00 00 00 00 00>
```



#### 2.3 Buffer.from(array)

参数说明：

- `array` <integer [ ]>

**返回值：**返回的是一个被 `array` 的值初始化的新的 `Buffer` 实例。(传入的数组元素只能是整型，不然就会自动被 0 覆盖)

示例一：参数数组里的元素都是整型数字。

```js
import { Buffer } from 'buffer';

const buf = Buffer.from([1, 2, 3])
console.log(buf) // => <Buffer 01 02 03>

const buf1 = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72])
console.log(buf1) // => <Buffer 62 75 66 66 65 72>
```

示例二：参数array里的元素如果不是整型数字，那么自动被0覆盖。

```js
import { Buffer } from 'buffer';

const buf1 = Buffer.from([1, 2, 3, { a: 1 }, [1, 2, 3], 'a'])
console.log(buf1) // => <Buffer 01 02 03 00 00 00>
// 不是整型数字的自动被0覆盖。
```



#### 2.4 Buffer.from(arrayBuffer[, byteOffset[, length]])

参数说明：

- `arrayBuffer`  <ArrayBuffer> | <ShareArrayBuffer> 例如 `TypedArray(类型化数组视图)` 的 `.buffer` 属性。
- `byteOffset` <integer> 要暴露的第一个字节的索引。 默认值: 0。
- `length` <integer> 要暴露的字节数。默认值: `arrayBuffer.byteLength - byteOffset`。

**返回值：**返回一个新建的与给定的 `ArrayBuffer` 共享同一内存的 `Buffer`。

示例：

```js
import { Buffer } from 'buffer';

const arr = new Uint16Array(2); //实例化一个无符号整型数组，该数组单个元素值的范围是0~65535

arr[0] = 5000;
arr[1] = 4000;

// 与 `arr` 共享内存。
const buf = Buffer.from(arr.buffer);

console.log(buf);
// 打印: <Buffer 88 13 a0 0f>

// 更改原始的 Uint16Array 也会更改缓冲区。
arr[1] = 6000;

console.log(buf);
// 打印: <Buffer 88 13 70 17>
```



#### 2.5 Buffer.from(buffer)

参数说明：

- `buffer` <Buffer> | <Uint8Array>  要复制的现有 `Buffer` 或 `Unit8Array`。

该API是复制传入的 `Buffer实例` 的数据，产生一个新的 `Buffer实例`。

**返回值：**一个新的 `Buffer实例`。

示例：

```js
import { Buffer } from 'buffer';

const buf1 = Buffer.from('buffer');
const buf2 = Buffer.from(buf1);

buf1[0] = 0x61; //即把第一位变成a

console.log(buf1.toString());
// 打印: auffer
console.log(buf2.toString());
// 打印: buffer
```



#### 2.6 Buffer.from(string[, encoding])

参数说明：

- `string` <string> 要编码的字符串。
- `encoding` <string> `string` 的编码。 **默认值:** `'utf8'`。

**返回值：**返回一个被字符串初始化的新的 `Buffer实例`。

```js
import { Buffer } from 'buffer';

const buf1 = Buffer.from('this is a tést')
const buf2 = Buffer.from('7468697320697320612074c3a97374', 'hex');

console.log(buf1) // => <Buffer 74 68 69 73 20 69 73 20 61 20 74 c3 a9 73 74>
console.log(buf2) // => <Buffer 74 68 69 73 20 69 73 20 61 20 74 c3 a9 73 74> 

console.log(buf1.toString()) // => this is a tést
console.log(buf2.toString()) // => this is a tést

console.log(buf1.toString('hex')) // => 7468697320697320612074c3a97374
console.log(buf2.toString('hex')) // => 7468697320697320612074c3a97374
```



### 三、缓冲区的长度

要想知道缓冲区的长度，可以利用 `Buffer实例` 的 `length属性`。

该属性返回的是缓存区中的字节数。

使用示例：

```js
// 示例一
const buf = Buffer.alloc(6)
console.log(buf) // => <Buffer 00 00 00 00 00 00>
console.log(buf.length) // => 6

// 示例二
const buf1 = Buffer.from([1, 2, 3, 4])
console.log(buf1) // => <Buffer 01 02 03 04>
console.log(buf1.length) // => 4

// 示例三：
const buffer = Buffer.from('hello')
console.log(buffer) // => <Buffer 68 65 6c 6c 6f>
console.log(buffer.length) // => 5

//示例四：
const buf2 = Buffer.alloc(10)

console.log(buf2, '字节数：' + buf2.length) // => <Buffer 00 00 00 00 00 00 00 00 00 00> 字节数：10
buf2.write('abcd', 0, 'utf-8')
console.log(buf2, '字节数：' + buf2.length) // => <Buffer 61 62 63 64 00 00 00 00 00 00> 字节数：10
```



### 四、写入缓冲区

将字符串数据写入到缓冲区，有很多的API方法，这里只简单记录 `buf.write()` 这个API。

#### 4.1 buf.write(string, offset, length, encoding)

参数描述：

- `String` <string> 要写入缓冲区（buf）的字符串。
- `offset` <integer>（可选） 开始写入字符串之前要跳过的字节数。**默认值：0**。
- `length` <integer>  （可选）要写入的最大字节数（写入的字节数不会超过 `buf.length - offset`）。 **默认值:** `buf.length - offset`。
- `encodeing` <string> （可选）字符串的字符编码。**默认值:** `'utf8'`。

**返回值：**返回实际写入的大小。如果 buffer 空间不足， 则只会写入部分字符串。

示例一：写入的字符串长度大小 小于 缓冲区大小。

```js
import { Buffer } from 'buffer';

const buf = Buffer.alloc(256) //创建一个256长度大小的实例。

const length = buf.write('\u00bd + \u00bc = \u00be')

console.log(length) // => 12
console.log(`${length} bytes: ${buf.toString('utf-8', 0, length)}`) // => 12 bytes: ½ + ¼ = ¾
```

示例二：写入的字符串长度大小 大于 缓冲区大小，字符串会被截取。

```js
import { Buffer } from 'buffer';

const buf1 = Buffer.alloc(10) //创建一个256长度大小的实例。

const length1 = buf1.write('hello world') //该字符串长度是大于10了。

console.log(length1) // => 10
console.log(`${length1} bytes: ${buf1.toString()}`) // => hello worl
//结果可以发现，字符串被截取了，返回的长度就是缓冲区的大小。
```

示例三：跳过n个字节数进行写入字符串。

```js
import { Buffer } from 'buffer';

const buf2 = Buffer.alloc(12)

const length2 = buf2.write('abcd', 10)

console.log(length2) // => 2
console.log(`${length2} bytes: ${buf2.toString('utf8', 10, 12)}`) // => 2 bytes: ab
```

示例四：控制写入的字符串长度。

```js
import { Buffer } from 'buffer';

const buf3 = Buffer.alloc(6)
const length3 = buf3.write('threey333', 0, 6)

console.log(length3) // => 6 
console.log(`${length3} bytes: ${buf3.toString('utf8', 0, 6)}`) // => 6 bytes: threey
```



### 五、从缓冲区读取数据信息

从缓冲区读取数据信息可以使用 `toString()` 这个API方法。

#### 5.1 buf.toString([encoding[, start[, end]]])

参数描述：

- `encoding` <string> 要使用的字符编码。 **默认值:** `'utf8'`。
- `start` <integer> 开始解码的字节偏移量。 **默认值:** `0`。
- `end` <integer> 停止解码的字节偏移量（不包括在内）。 **默认值:** `buf.length`。

**返回值：**解码缓冲区中的数据并使用指定的编码返回字符串。

> ==注意：==如果 `encoding` 是 `'utf8'` 并且输入中的字节序列 `不是有效的 UTF-8`，则每个无效字节都将替换为替换字符 `U+FFFD`。

使用示例：

```js
import { Buffer } from 'buffer';

const buf1 = Buffer.allocUnsafe(26);

for (let i = 0; i < 26; i++) {
  // 97 是 'a' 的十进制 ASCII 值。
  buf1[i] = i + 97;
}

console.log(buf1.toString('utf8'));
// 打印: abcdefghijklmnopqrstuvwxyz
console.log(buf1.toString('utf8', 0, 5));
// 打印: abcde

const buf2 = Buffer.from('tést');

console.log(buf2.toString('hex'));
// 打印: 74c3a97374
console.log(buf2.toString('utf8', 0, 3));
// 打印: té
console.log(buf2.toString(undefined, 0, 3));
// 打印: té
```



### 六、将Buffer实例转换为JSON对象

要将 `Buffer实例` 转换为JSON对象，可以使用 `Buffer实例.toJSON()` 这个API方法。

>API方法：Buffer实例.toJSON()

**返回值：**返回 `Buffer实例` 的JSON表示。

==注意：== `JSON.stringify()`在字符串化 `Buffer` 实例时隐式调用此函数。

示例一：

```js
import { Buffer } from 'buffer';

const buf = Buffer.from([1, 2, 3, 4, 5]);

console.log(buf) // => <Buffer 01 02 03 04 05>

const json = JSON.stringify(buf);
console.log(json)
// 打印: {"type":"Buffer","data":[1,2,3,4,5]}

const copy = JSON.parse(json, (key, value) => {
  return value && value.type === 'Buffer' ?
    Buffer.from(value) :
  value;
});

console.log(copy) // => <Buffer 01 02 03 04 05>
```

示例二：直接使用 `toJSON()`。

```js
import { Buffer } from 'buffer';

const buf = Buffer.from([1, 2, 3, 4, 5]);

const json1 = buf.toJSON()
console.log(json1) // => { type: 'Buffer', data: [ 1, 2, 3, 4, 5 ] }
```

==注意：==直接使用 `toJSON()` 转换为JSON对象，**该JSON对象是未字符串化的**。



示例三：`Buffer.from()` 接受从此方法返回的格式的对象。

```js
import { Buffer } from 'buffer';

const json1 = buf.toJSON()

const buf2 = Buffer.from(json1)

console.log(buf2) // => <Buffer 01 02 03 04 05>
```

示例四：`Buffer.from(buf.toJSON())` 的工作方式类似于 `Buffer.from(buf)`。

```js
import { Buffer } from 'buffer';

const json1 = buf.toJSON()

const buf2 = Buffer.from(json1)
const buf3 = Buffer.from(buf)

console.log(buf2) // => <Buffer 01 02 03 04 05>
console.log(buf3) // => <Buffer 01 02 03 04 05>
```



### 七、缓冲区合并

使用 `Buffer.concat(list[, totalLength])` 可以使缓冲区进行合并。

>API方法：Buffer.concat(list[, totalLength])

参数描述：

- `list` <Buffer[]> | <Uint8Array[]> 要连接的 `Buffer实例` 或 `Uint8Array实例` 的列表。
- `totalLength` <integer> 连接时 `list列表` 中的 `Buffer实例` 的总长度。

**返回值：**返回一个由多个成员合并的 `新的Buffer实例`。

示例一：

```js
import { Buffer } from 'buffer';

// 示例一：
const buf1 = Buffer.from('hello ')
const buf2 = Buffer.from('threey')
const newBuf = Buffer.concat([buf1, buf2])

console.log(buf1.toString()) // => hello
console.log(buf2.toString()) // => world
console.log('缓冲区合并后的结果：', newBuf.toString()) // => 缓冲区合并后的结果： hello threey
```

示例二：

```js
import { Buffer } from 'buffer';

// 从三个 `Buffer` 实例的列表创建单个 `Buffer`。
const buf1 = Buffer.alloc(4);
const buf2 = Buffer.alloc(6);
const buf3 = Buffer.alloc(8);
const totalLength = buf1.length + buf2.length + buf3.length;

console.log(totalLength);
// 打印: 18

const bufA = Buffer.concat([buf1, buf2, buf3], totalLength);

console.log(bufA);
// 打印: <Buffer 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>
console.log(bufA.length);
// 打印: 18
```



### 八、拷贝缓冲区

使用 `buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])` 这个API可以拷贝缓冲区。

>API方法：buf.copy(target[, targetStart[, sourceStart[, sourceEnd]]])

参数描述：

- `target`：<Buffer> | <Uint8Array> 目标缓冲区，即将数据复制到目标 `Buffer实例` 或 `Uint8Array`。
- `targetStart` <integer> 目标缓冲区内开始写入的偏移量。**默认值：0。**
- `sourceStart` <integer> 被复制缓冲区内开始的偏移量。**默认值：0。**
- `sourceEnd` <integer> 被复制缓冲区内开始的偏移量。**默认值：该缓冲区的长度。**

**返回值：**没有返回值。

==注意：目标缓冲区可以是自己，具体看示例二。==

示例一：

```js
import { Buffer } from 'buffer';

// 创建两个 `Buffer` 实例。
const buf1 = Buffer.allocUnsafe(26);
const buf2 = Buffer.allocUnsafe(26).fill('!');

for (let i = 0; i < 26; i++) {
  // 97 是 'a' 的十进制 ASCII 值。
  buf1[i] = i + 97;
}

// 将 `buf1` 字节 16 到 19 复制到 `buf2` 中，从 `buf2` 的字节 8 开始。
buf1.copy(buf2, 8, 16, 20);
// 这相当于：
// buf2.set(buf1.subarray(16, 20), 8);

console.log(buf2.toString('ascii', 0, 25));
// 打印: !!!!!!!!qrst!!!!!!!!!!!!!
```

示例二：

```js
import { Buffer } from 'buffer';

// 创建 `Buffer` 并将数据从一个区域复制到同一 `Buffer` 内的重叠区域。

const buf = Buffer.allocUnsafe(26);

for (let i = 0; i < 26; i++) {
  // 97 是 'a' 的十进制 ASCII 值。
  buf[i] = i + 97;
}

buf.copy(buf, 0, 4, 10);

console.log(buf.toString());
// 打印: efghijghijklmnopqrstuvwxyz
```



### 九、缓冲区裁剪

使用 `buf.slice([start[, end]])` 这个API方法可以裁剪缓冲区。

>API方法：buf.slice([start[, end]])

参数描述：

- `start`：新的 `Buffer实例` 开始的位置。**默认值：0。**
- `end`：新的 `Buffer实例` 结束的位置。**默认值：buffer实例.length。**

**返回值：**返回一个新的 `Buffer实例`。该实例与原始缓冲区指向同一块内存（即一个改变，另一个也会发生变化）。但由 `start` 和 `end` 索引进行偏移和裁剪。（浅拷贝）

使用示例一：

```js
import { Buffer } from 'buffer';

const buf = Buffer.from('buffer');

const buf1 = buf.slice(0)

buf1[0] ++
console.log(buf1.toString()) // 打印：duffer

console.log(buf.toString()) // 打印：duffer
```

使用示例二：

```js
const buf = Buffer.from('buffer');

const copiedBuf = Uint8Array.prototype.slice.call(buf);
copiedBuf[0]++;
console.log(copiedBuf.toString());
// 打印: cuffer

console.log(buf.toString());
// 打印: buffer
```

