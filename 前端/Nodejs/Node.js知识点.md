# Node.js知识点

##  一、全局变量

在浏览器环境中，通常 `window ` 是全局对象，而在Node.js环境中，全局对象是 `global`。

- ==Node.js环境中提供了全局变量给我们直接使用。==

- 所有全局变量（除了 `global` 本身以外）都是 `global` 对象的属性。

常用的全局变量（也称`global属性`）：

1. `console`：用于打印日志。
2. `__dirname`：获取当前文件的绝对路径。
3. `__filename`：获取当前文件的全名路径（绝对路径+文件名） 。
4. `process`：描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口。通常在你写本地命令行程序的时候，少不了要 和它打交道。
5. `setTimeout` / `clearTimeout`：设置 / 清除延时器。
6. `setInterval` / `clearInterval`：设置 / 清除定时器。
7. CommonJS的模块加载`require`、模块导出`module.exports`。



## 二、常用模块

NodeJS中有很多模块，这里只记录常用的模块。常用模块有：`fs文件系统模块`、`path路径模块`、`http模块`、`url模块` 等。

==注意：由于这些常用模块并不是全局对象global的属性，所以使用它们之前要预先导入进来。==

### 2.1 fs文件系统模块

>`fs文件系统模块 `是nodejs中最常用的一个模块，因此掌握它是非常有必要的。fs模块的方法非常多,用到了哪个查哪个即可。
>
>文档地址：http://nodejs.cn/api/fs.html



#### 2.1.1 打开文件

使用 `fs文件系统模块` 打开文件，可以使用 `fs文件系统模块` 下的 `异步open方法`。

>API方法：fs.open(path[, flags[, mode]], callback)

参数描述：

- `path`：<string> | <Buffer> | <URL> 路径。
- `flags`：<string> | <number> 打开文件的行为，**默认值：'r'。** (可选参数)
- `mode`：<string> | <integer>  设置文件模式，**默认值：0o666(0o为八进制) 即可读可写。**（可选参数）
- `callback`：<Function> 回调函数，回调函数有两个参数（err，fd）。其中第二个参数代表一个**文件描述符**，用于访问回调中打开的文件。

**flags参数的值：**

| Flag | 描述                                                   |
| :--- | :----------------------------------------------------- |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。           |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。           |
| rs   | 以同步的方式读取文件。                                 |
| rs+  | 以同步的方式读取和写入文件。                           |
| w    | 以写入模式打开文件，如果文件不存在则创建。             |
| wx   | 类似 'w'，但是如果文件路径存在，则文件写入失败。       |
| w+   | 以读写模式打开文件，如果文件不存在则创建。             |
| wx+  | 类似 'w+'， 但是如果文件路径存在，则文件读写失败。     |
| a    | 以追加模式打开文件，如果文件不存在则创建。             |
| ax   | 类似 'a'， 但是如果文件路径存在，则文件追加失败。      |
| a+   | 以读取追加模式打开文件，如果文件不存在则创建。         |
| ax+  | 类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。 |

使用示例：

```js
const { open } = require('fs')

// 异步打开文件
console.log('准备打开文件')

open('test.txt', 'r+', (err, fd) => {
  if (err) throw err
  console.log('open succeeded!')
})
```



#### 2.1.2 获取文件信息

使用 `fs文件系统模块` 获取文件信息，可以使用 `fs文件系统模块` 下的 `异步readFile方法`。

>API方法：fs.stat(path, callback)

参数描述：

- `path` <string> | <Buffer> | <URL>：路径。
- `callback`：回调函数。该回调函数有两个参数 `(err,stats)`，其中 `stats` 是 `fs.Stats对象`。
  - `err` <Error> 
  - `stats` <fs.Stats> 

**`fs.stat(path)` 执行后，会将 `stats类的实例` 返回给其回调函数。可以通过stats类中的提供方法判断文件的相关属性。**

stats类中的方法有：

| 方法                      | 描述                                                         |
| :------------------------ | :----------------------------------------------------------- |
| stats.isFile()            | 如果是文件返回 true，否则返回 false。                        |
| stats.isDirectory()       | 如果是目录返回 true，否则返回 false。                        |
| stats.isBlockDevice()     | 如果是块设备返回 true，否则返回 false。                      |
| stats.isCharacterDevice() | 如果是字符设备返回 true，否则返回 false。                    |
| stats.isSymbolicLink()    | 如果是软链接返回 true，否则返回 false。                      |
| stats.isFIFO()            | 如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。 |
| stats.isSocket()          | 如果是 Socket 返回 true，否则返回 false。                    |

使用示例：

```js
const { stat } = require('fs')

const dir = __dirname

// 示例一：判断是否是文件
stat('./test.txt', (err, stat) => {
  if (err) throw err
  // 判断是否是文件
  console.log(stat.isFile()) // => true
  // 判断是否是目录
  console.log(stat.isDirectory()) // => false
})

// 示例二：判断是否是目录
stat(dir, (err, stat) => {
  if (err) throw err
  // 判断是否是文件
  console.log(stat.isFile()) // => false
  // 判断是否是目录
  console.log(stat.isDirectory()) // => true
})

// 示例三，查看Stats实例的信息
stat('./test1.txt', (err, stat) => {
  if (err) throw err
  console.log(stat)
  
//打印结果: 
//Stats {
//  dev: 411043372,       
//  mode: 33206,
//  nlink: 1,
//  uid: 0,
//  gid: 0,
//  rdev: 0,
//  blksize: 4096,        
//  ino: 2533274791369241,
//  size: 0,
//  blocks: 0,
//  atimeMs: 1643820656270.2808,
//  mtimeMs: 1643820656270.2808,
//  ctimeMs: 1643820656270.2808,
//  birthtimeMs: 1643820656270.2808,
//  atime: 2022-02-02T16:50:56.270Z,
//  mtime: 2022-02-02T16:50:56.270Z,
//  ctime: 2022-02-02T16:50:56.270Z,
//  birthtime: 2022-02-02T16:50:56.270Z
//}
})
```

==注意：==不推荐在调用 `fs.open()`、`fs.readFile()` 或 `fs.writeFile()` 之前使用 `fs.stat()` 检查文件是否存在。 而是，用户代码应该直接打开/读取/写入文件，并在文件不可用时处理引发的错误。



#### 2.1.3 读取文件信息

**读取文件信息，这里简单记录两种常用的API方法：readFile方法和read方法。**

##### 2.1.3.1 异步readFile方法：

一、使用 `fs文件系统模块` 读取文件信息，可以使用 `fs文件系统模块` 下的 `异步readFile方法`。

>API方法：fs.readFile(path[, options], callback)

参数描述：

- `path` <string> | <Buffer> | <URL> | <integer>：文件名或文件描述符。
- `options`：<Object> | <string> ：当该参数是对象的时候会包含{encoding, flag, signal}。默认编码为 `null`，flag为 `‘r’`；当该参数是个字符串的时候，表示的就是 `字符编码`。
- `callback` <Function>：回调函数，该回调函数有两个参数（`err`, `data`）。

**==注意：==**

>`fs.readFile()` 函数缓冲整个文件。 为了最小化内存成本，在可能的情况下优先通过 `fs.createReadStream()` 进行流式传输。

一：不带编码参数的使用示例：

```js
const fs = require('fs')

fs.readFile('./数据file.txt', (err, data) => {
  if (err) console.log(err.message)
  console.log(data)
})

//结果: <Buffer 68 65 6c 6c 6f 20 77 72 6f 6c 64 20 31 32 33 31 32 33 31 61 73 64 73 61 52 6f 73 65 2f 72 2f 6e 52 6f 73 65 0d 0a 52 6f 73 65>
//返回的是一个Buffer实例数据。
```

二：携带编码参数的使用示例：

```js
const fs = require('fs')

fs.readFile('./数据file.txt', 'utf-8', (err, data) => {
  if (err) console.log(err.message)
  console.log(data)
})

//结果：hello wrold 1231231asdsaRose/r/nRose
//返回的是一个字符串。
```

**两者的区别：**

- 如果有编码参数，那么返回的结果默认已转换成字符串。
- 如果没有编码参数，那么返回的结果是一个 `Buffer对象` ，即二进制数据。
- ==其实读取任意的文件，返回的结果都是二进制数据，即Buffer实例。==
- `Buffer实例`可以调用toString()方法转换成字符串（即从缓冲区中读取数据）。



##### 2.1.3.2 异步read方法：

对于需要尽可能快地读取文件内容的应用程序，最好直接使用 `fs.read()` 并让应用程序代码管理读取文件本身的全部内容。

二、使用 `fs文件系统模块` 读取文件信息，可以使用 `fs文件系统模块` 下的 `异步read方法`。（该方法读取文件信息比上面的方法更快。）

>API方法：fs.read(fd, buffer, offset, length, position, callback)

该API方法从 `fd` 指定的文件中读取数据。

参数描述：

- `fd` <integer>：通过 `fs.open()` 方法返回的文件描述符。
- `buffer` <Buffer> | <TypedArray> | <DataView>：数据写入的缓冲区。
- `offset` <integer>：要写入数据的缓冲区中的位置。
- `length` <integer>：读取的字节数。
- `position` <integer> | <bigint>：文件读取的位置。。 如果 `position` 为 `null` 或 `-1 `，则将从当前文件位置读取数据，并更新文件位置。 如果 `position` 是整数，则文件位置将保持不变。
- `callback`：回调函数，有三个参数`err`, `bytesRead`, `buffer`，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

使用示例：

```js
const { read, open } = require('fs')
const { Buffer } = require('buffer')

const buf = Buffer.alloc(200)
console.log(buf) // => <Buffer 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>

// 封装打开文件API
const openFile = ({ path, flags }) => {
  return new Promise((resolve, reject) => {
    open(path, flags, (err, fd) => {
      if (err) reject(err)
      resolve(fd)
    })
  })
}
// 封装读取文件API
const readFileWay = ({ fd, buf, offset, length, position }) => {
  return new Promise((resolve, reject) => {
    read(fd, buf, offset, length, position, (err, bytes, buf) => {
      if (err) reject(err)
      resolve({ bytes, buf })
    })
  })
}

// 读取文件信息这里有两种
console.log('准备打开文件')
openFile({ path: './test1.txt', flags: 'r+' }).then(success => {
  console.log('打开文件成功')
  console.log('准备读取文件')
  return readFileWay({ fd: success, buf, offset: 0, length: buf.length, position: 0 })
}).then(({ bytes, buf }) => {
  console.log(bytes)
  console.log(`${bytes} 个字节被读取`)
  if (bytes > 0) {
    console.log(buf.toString('utf8', 0)); // => hello world.
  }
}).catch(err => {
  console.log(err)
})
```



#### 2.1.4 将数据信息写入文件中/创建文件

使用 `fs文件系统模块` 将数据信息写入文件中 或者 创建文件，可以使用 `fs文件系统模块` 下的 `异步writeFile方法`。

>API方法：fs.writeFile(file, data[, options], callback)

参数描述：

- `file` <string> | <Buffer> | <URL> | <integer>：文件名或文件描述符。

- `data`：<string> | <Buffer> | <TypeArray> | <DataView> | <Object> ：要写入文件的数据。
- `options` ：该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'。
- `callback` <Function>：回调函数，回调函数只包含错误信息参数(err)，如果在写入失败时会返回该错误对象。

**==注意：==**

>- 该方法每次将数据信息写入到文件中都是会覆盖该文件的原来的内容。
>- 在同一个文件上多次使用 `fs.writeFile()` 而不等待回调是不安全的。 对于这种情况，建议使用`fs.createWriteStream()`。

示例一：

```js
const { writeFile, readFile } = require('fs')

writeFile('./test.txt', 'nodejs javascript vue.js', err => {
  if (err) throw err
  console.log('write successed')
  // 验证是否成功向文件写入数据
  console.log('-------')
  readFile('./test.txt', 'utf8', (err, data) => {
    if (err) throw err
    console.log(data) // => nodejs javascript vue.js
  })
})
```

示例二：当`data` 是缓冲区，则忽略 `encoding` 选项。

```js
const { writeFile, readFile } = require('fs')

const data = new Uint8Array(Buffer.from('Hello Node.js'));

new Promise((resolve, reject) => {
  writeFile('message.txt', data, (err) => {
    if (err) throw err;
    resolve('The file has been saved!')
  });
}).then(res => {
  console.log(res) // => The file has been saved!
  return new Promise((resolve, reject) => {
    readFile('message.txt', 'utf8', (err, data) => {
      if (err) throw err
      resolve(data)
    })
  })
}).then(res => {
  console.log(res)  // => Hello Node.js
})
```

示例三：如果有 `options` 并且是字符串，那么这些选项就是在指定编码。

```js
import { writeFile } from 'fs';

writeFile('message.txt', 'Hello Node.js', 'utf8', callback);
```



#### 2.1.5 向文件追加数据信息

使用 `fs文件系统模块` 向文件追加数据信息，可以使用 `fs文件系统模块` 下的 `异步appendFile方法`。

>API方法：fs.appendFile(path, data[, options], callback)

参数描述：

- `path` <string> | <Buffer> | <URL> | <number>：文件名或文件描述符。
- `data` <string> | <Buffer>：要向文件追加的数据信息。
- `options` <Object> | <string>：该参数是一个对象。当参数是一个对象的时候，其包含{encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'。当参数是一个字符串，则说明是字符编码encoding。

- `callback` <Function>：回调函数。该回调函数只有一个 `err`参数。

说明：该API方法是异步地将数据追加到文件，如果该文件尚不存在，则创建该文件。 `data` 可以是字符串或 `Buffer实例`。

使用示例一：

```js
const { appendFile } = require('fs')

// 向文件追加数据
appendFile('./test1.txt', '\r\nthreey is a boy!', err => {
  if (err) throw err
  console.log('追加数据成功')
})
```

使用示例二：如果 `options` 是字符串，则它指定编码：

```js
const { appendFile } = require('fs')

appendFile('message.txt', 'data to append', 'utf8', callback);
```



#### 2.1.6 关闭文件

使用 `fs文件系统模块` 关闭文件，可以使用 `fs文件系统模块` 下的 `异步close方法`。

>API方法：fs.close(fd, callback)

参数描述：

- `fd` <integer>：通过 `fs.open()` 方法返回的文件描述符。
- `callback`：回调函数，该回调函数只要 `err`。

使用示例：

```js
const { close } = require('fs')
const { read, open } = require('fs')
const { Buffer } = require('buffer')

const buf = Buffer.alloc(200)
console.log(buf) // => <Buffer 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00>

// 封装打开文件API
const openFile = ({ path, flags }) => {
  return new Promise((resolve, reject) => {
    open(path, flags, (err, fd) => {
      if (err) reject(err)
      resolve(fd)
    })
  })
}
// 封装读取文件API
const readFileWay = ({ fd, buf, offset, length, position }) => {
  return new Promise((resolve, reject) => {
    read(fd, buf, offset, length, position, (err, bytes, buf) => {
      if (err) reject(err)
      resolve({ bytes, buf, fd })
    })
  })
}

// 读取文件信息这里有两种
console.log('准备打开文件')
openFile({ path: './test1.txt', flags: 'r+' }).then(success => {
  console.log('打开文件成功')
  console.log('准备读取文件')
  return readFileWay({ fd: success, buf, offset: 0, length: buf.length, position: 0 })
}).then(({ bytes, buf, fd }) => {
  console.log(bytes)
  console.log(`${bytes} 个字节被读取`)
  if (bytes > 0) {
    console.log(buf.toString('utf8', 0)); // => hello world.
  }

  // 关闭文件
  close(fd, err => {
    if (err) reject(err)
    console.log('关闭文件成功')
  })
}).catch(err => {
  console.log(err)
})

```



#### 2.1.7 删除文件

使用 `fs文件系统模块` 删除文件，可以使用 `fs文件系统模块` 下的 `异步close方法`。

>API方法：fs.unlink(path, callback)

参数描述：

- `path` <string> | <Buffer> | <URL>：文件路径。
- `callback` <Function>：回调函数，该回调函数只有一个`err`参数。

==注意：==该API方法不适用于删除目录，无论是空目录还是其它目录。要删除目录，请使用 `fs.rmdir()`。

使用示例一：

```js
const { unlink } = require('fs')

// 假设 'path/file.txt' 是普通文件。
unlink('path/file.txt', (err) => {
  if (err) throw err;
  console.log('path/file.txt was deleted');
});
```

使用示例二：批量删除文件：

```js
const { unlink } = require('fs')

const deleteFiles = (url) => {
  unlink(url, err => {
    if (err) throw err
    console.log('删除该文件成功')
  })
}

for (let i = 1; i <= 6; i++) {
  deleteFiles(`./文件${i}.txt`)
}
```



#### 2.1.8 其它API方法

由于API方法过多，这里只记录常用的几个。如果想了解其它API方法的使用，可以查阅官方文档：[fs文件系统模块API](http://nodejs.cn/api/fs.html)。



### 2.2 path路径模块

`path路径模块` 是NodeJS官方提供的用于处理文件和目录的路径的实用工具模块。它提供了一系列属性和API方法给我们使用。

- 在实际开发过程中，我们可能会遇到过这个问题：在wiindows操作系统下写的代码，在Linux操作系统上执行不了，问题可能出现的地方就是路径。
- 比如说：在linux操作系统中，路径分隔符使用的是`/`，但是在windows操作系统中，路径使用的`\`，所以就会造成问题。

使用 `path路径模块` 处理文件和目录的路径，就会避免这些问题的发生。



#### 2.2.1 path.join([...paths])

该API方法是用来连接路径的。最重要的是**它会正确使用当前系统的路径分隔符，Unix系统是 `“/"`，Windows系统是 `"\"`。**

>API方法：path.join([...paths])

参数描述：

- `...path` <string> ：路径片段的序列。

**返回值：**返回一个各路径已连接在一起的总路径。

使用示例一：

```js
const { join } = require('path');

const splicePath = join('/foo', '/bar', 'wto', 'nodejs/js')

console.log(splicePath) // => '\foo\bar\wto\nodejs\js'
```

使用示例二：如果参数中出现一个不是字符串的，则会抛出错误。

```js
const { join } = require('path');

join('foo', {}, 'bar');
// 抛出 'TypeError: Path must be a string. Received {}'
```

使用示例三：各系统平台拼接路径。

```js
const { join } = require('path');

//windows系统下
join("abc","def","gg", "index.html")
"abc\def\gg\a.html"

//linux系统下
join("abc","def","gg", "index.html")
'abc/def/gg/index.html'
```



#### 2.2.2 path.basename(path[, ext])

该API方法是用来返回路径中最后一部分的。

>API方法：path.basename(path[, ext])

参数描述：

- `path` <string>：文件或目录的路径。
- `ext` <string> ：可选的文件扩展名。

**返回值：**返回 `path` 的最后一部分。

==注意：该API方法是会区分文件扩展名的大小写的。==

使用示例一：文件路径。

```js
const { basename } = require('path')

// 文件路径
const path = '/foo/bar/baz/asdf/quux.html'

const lastPartPath = basename(path)

console.log(lastPartPath) // => quux.html
// 返回的就是 =>（路径的最后一部分.文件扩展名）。
```

使用示例二：目录路径。

```js
const { basename } = require('path')

// 目录路径
const path = '/earningZiLiao/前端/Node.js/05-path路径模块'

const lastPartPath = basename(path)

console.log(lastPartPath) // => 05-path路径模块
// 返回的就是 =>（目录路径的最后一部分）。
```

使用示例三：区分文件扩展名。

```js
const { basename } = require('path')

// 区分文件扩展名
basename('C:\\foo.html', '.html');
// 返回: 'foo'

basename('C:\\foo.HTML', '.html');
// 返回: 'foo.HTML'
```



#### 2.2.3 path.extname(path)

>API方法：path.extname(path)

该API方法返回的是文件路径中的文件扩展名，即 `path` 的最后一部分中从最后一次出现的 `.`（句点）字符到字符串的结尾。 如果 `path` 的最后一部分中没有 `.`，或者除了 `path` 的基本名称的第一个字符之外没有 `.` 个字符，则返回空字符串。

使用示例：

```js
const path = require('path')

path.extname('index.html');
// 返回: '.html'

path.extname('index.coffee.md');
// 返回: '.md'

path.extname('index.');
// 返回: '.'

path.extname('index');
// 返回: ''

path.extname('.index');
// 返回: ''

path.extname('.index.md');
// 返回: '.md'
```



#### 2.2.4 其它API方法

由于API方法过多，这里只记录常用的几个。如果想了解其它API方法的使用，可以查阅官方文档：[path路径模块API](http://nodejs.cn/api/path.html)。



### 2.3 http模块

`http模块` 是NodeJS官方提供的用于搭建HTTP服务端和客户端，使用 HTTP 服务器或客户端功能必须调用 `http模块`。

由于http模块有很多知识，这里只记录几个常用的使用方法，更多可以查阅官方文档进行学习：[http模块](http://nodejs.cn/api/http.html)。



#### 2.3.1 创建web服务器

创建web服务器需要到 `http模块` 下的 `createServer()方法`,同时需要 `request` 事件。

>API方法：http.createServer([options],requestListen)

参数描述：

- `options` <Object>：这个参数有很多属性，查官方文档学习。（可选）
- `requestListen` <Function> ，该参数函数会自动添加到 `request`事件下。

详细说明：

>- 给服务器注册request事件，只要服务器接收到了客户端的请求，就会触发request事件
>- request事件有两个参数，request表示请求对象，可以获取所有与请求相关的信息，response是响应对象，可以获取所有与响应相关的信息。
>- 服务器监听的端口范围为：1-65535之间，推荐使用3000以上的端口，因为3000以下的端口一般留给系统使用。

示例一：

```js
const { createServer } = require('http')

// 创建本地服务器来从其接收数据
const server = createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.write('<h1>Hello world</h1>')
  res.end(JSON.stringify({
    data: 'Hello World!'
  }));
})

server.listen(9090, () => {
  console.log('服务器启动成功,请访问:http://localhost:9090')
})
```

示例二：

```js
const { createServer } = require('http')

// 创建本地服务器来从其接收数据
const server = createServer()

//监听请求事件
server.on('request', (req, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' })
  res.end(JSON.stringify({
    data: 'Hello world!'
  }))
})

server.listen(9090, () => {
  console.log('服务器启动成功，请访问:http://localhost:9090')
})
```



#### 2.3.2 request对象

文档地址：http://nodejs.cn/api/http.html#class-httpincomingmessage

`request` 对象下有很多的属性：

- `method`：请求方法。(仅适用于从http服务器获得请求) （只读）
- `url`：请求地址。(仅适用于从http服务器获得请求) 
- `headers`：请求头对象信息。（对象形式）
- `rawHeaders`：原始请求头完全按照收到的方式列出。（数组形式）

使用示例：

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {
  // 1.查看请求方式
  console.log(req.method)  // => 'GET'
  // 2.查看请求地址
  console.log(req.url) // => '/'
  // 3.查看请求头
  console.log(req.headers) // => 对象形式
  // 打印如下内容：
  //
  // { 'user-agent': 'curl/7.22.0',
  //   host: '127.0.0.1:8000',
  //   accept: '*/*' }
  
  // 4.查看请求头(数组形式)
  console.log(req.rawHeaders) // => 数组形式
  // 打印如下内容：
  //
  // [ 'user-agent',
  //   'this is invalid because there can be only one',
  //   'User-Agent',
  //   'curl/7.22.0',
  //   'Host',
  //   '127.0.0.1:8000',
  //   'ACCEPT',
  //   '*/*' ]

  res.writeHead(200, { 'Content-Type': 'application/json' })
  res.end(JSON.stringify({
    data: 'Hello world!'
  }))
})

server.listen(9090, () => {
  console.log('服务器启动成功，请访问:http://localhost:9090')
})
```



#### 2.3.3 serverResponse对象

文档地址：http://nodejs.cn/api/http.html#class-httpserverresponse

一、`serverResponse` 对象下常用的属性：

- `statusCode`：响应头状态码。默认是200。（可自己根据情况设置响应头状态码）
- `statusMessage`：响应头状态信息。（可自己根据情况设置响应头状态码）
- `writableEnded`：在 `response.end()` 执行完成后，该属性值为true。

使用示例：

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {

  res.statusCode = 200  // 设置响应头状态码
  res.statusMessage = 'OK' // 设置响应有状态信息 
  res.end(JSON.stringify({
    code: res.statusCode,
    message: res.statusMessage,
    id: 001,
    data: 'hello wold',
  })，'uft8',()=> {
    console.log(res.writableEnded) // => true 说明响应完成。
  })
})

server.listen(9090, () => {
  console.log('http://localhost:9090')
})
```

![image-20220205012753943](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220205012818.png)

二、`serverResponse` 对象下常用的方法：

##### 2.3.3.1 response.setHeader(name, value)

设置响应头信息，比如content-type。

参数描述：

- `name` <string>：响应头信息名称。
- `value` <any>：响应头信息值。

**==需要注意的：如果setHeader设置的头信息名称和writeHead设置的头信息名称一样，那么最终的结果是以writeHead设置的为准。==**

使用示例：

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {
  res.setHeader('Content-Type', 'text/html')
  res.setHeader('X-Foo', 'bar')
  res.setHeader('aa', 'bb')
  res.writeHead(200, {
    'Content-Type': 'application/json'
  })
  res.end('ok')
})

server.listen(9090, () => {
  console.log('listening on http://localhost:9090')
})
```

从结果图可以知道，设置的响应头信息都会存在。

![image-20220205021252459](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20220205021252459.png)



##### 2.3.3.2 response.writeHead(statusCode, statusMessage, headers)

设置响应头，同时可以设置状态码和状态信息。

**==注意：==**

>1. 必须先设置响应头，才能设置响应体。
>2. 该方法只调用一次。
>3. 如果setHeader设置的头信息名称和writeHead设置的头信息名称一样，那么最终的结果是以writeHead设置的为准。即writeHead设置的响应头信息会覆盖setHeader设置的响应头信息（名称相同的前提）。

参数描述：

- `statusCode` <number>：响应状态码。
- `statusMessage` <string> ：响应状态信息。(可选)
- `headers` <Object> | <Array>：响应头信息。注意：响应头可以是数组形式，其中键和值在同一个列表中。响应头信息可以直接设置。

**返回值：**返回对 `ServerResponse` 的引用，以便可以链式调用。

使用示例：

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {

  res.statusCode = 200  // 设置响应头状态码
  res.statusMessage = 'OK' // 设置响应有状态信息 
  // 设置响应头
  res.writeHead(res.statusCode, res.statusMessage, { 'Content-Type': 'application/json' })
  
  res.end(JSON.stringify({
    code: res.statusCode,
    message: res.statusMessage,
    id: 001,
    data: 'hello wold',
  }))
})

server.listen(9090, () => {
  console.log('http://localhost:9090')
})
```



##### 2.3.3.3 response.write(chunk, encoding, callback)

给浏览器发送请求体数据，可以调用多次，从而提供连续的请求体。

参数描述：

- `chunk` <string> | <Buffer>：响应体数据，该数据可以是字符串或缓冲区，同时该数据是返回给客户端的。
- `encoding` <string>：字符编码。默认是 'utf-8'。
- `callback` <Function> ：回调函数。该回调函数会在响应头数据返回成功后执行。

使用示例：

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {
  if (req.url === '/') {
    res.writeHead(200, {
      'Content-Type': 'text/html',

    })
    res.write('<h1>hello world</h1>', 'utf-8', () => {
      console.log('返回数据成功')
    })
    res.write('<h1>ni hao threey</h1>', 'uft8', () => {
      // 回调函数
      console.log('发送成功！')
    })
  }
  res.end()
})

server.listen(9090, () => {
  console.log('listening on http://localhost:9090')
})
```



##### 2.3.3.4 response.end(data, encoding, callback)

此API方法是通知服务器，所有响应头和响应体内容都已经发送，服务器认为此消息已完成。

参数描述：

- `data` <string> | <Buffer>：响应体中的数据。（参数可选）
- `encoding` <string>：字符编码。（参数可选）
- `callback` <Function>：回调函数，该回调函数在响应完成时调用。

**==注意：==**

>- 如果指定了 `data` ，则其效果类似于 `response.write(data, encoding)` 后跟 `response.end(callback)`。
>- 该API方法必须要使用，因为它时通知服务器响应内容是否发送完毕。

使用示例一：指定data。

```js
const { createServer } = require('http')

// 创建服务器
const server = createServer()

server.on('request', (req, res) => {
  if (req.url === '/') {
    // 设置响应头的内容类型为 'text/html'
    res.setHeader('Content-Type', 'text/html')

    res.end('<h1>hello world!</h1>', 'utf8', () => {
      console.log('响应完成')
    })
  }
})

server.listen('9090', () => {
  console.log('服务器已启动完成,服务器地址: http://localhost:9090')
})
```

使用示例二：没有指定data，这时如果想响应给客户一些数据，则需要write。

```js
const { createServer } = require('http')

const server = createServer()

server.on('request', (req, res) => {
  if (req.url === '/') {
    res.writeHead(200, {
      'Content-Type': 'text/html',
    })
    res.write('<h1>hello world</h1>', 'utf-8', () => {
      console.log('返回数据成功')
    })
    res.write('<h1>ni hao threey</h1>', 'uft8', () => {
      // 回调函数
      console.log('发送成功！')
    })
  }
  res.end()
})

server.listen(9090, () => {
  console.log('listening on http://localhost:9090')
})
```



### 2.4 url模块

`url模块` 是NodeJS官方提供的 用于网址URL的处理和解析 的模块。当我们要处理请求地址的URL的时候，可以借用这个模块进行处理。

`url模块` 有很多的属性和方法，这里只记录常用的几个。更多的可以查阅官方文档：[url模块](http://nodejs.cn/api/url.html)。



#### 2.4.1 查看url地址的所有信息

查看url地址的所有信息可以使用 `URL构造函数` 解析查看。

>URL构造函数：new URL(input[, base])

参数描述：

- `input` <string>：要解析的绝对或相对的输入网址。 如果 `input` 是相对的，则需要 `base`。如果 `input` 是绝对的，则忽略 `base`。
- `base` <string> | <URL>：如果 `input` 不是绝对的，则是要解析的基本网址。

==注意：URL构造函数既可以作为全局对象的属性直接使用，也可以从url模块中导入进来使用。==

使用示例一：作为全局属性或从模块导入进来使用。

```js
//URL构造函数既可以作为全局对象global的属性直接使用
const URL1 = new URL('/foo', 'https://example.org/');
console.log(URL1)

// 也可以从url模块中导入进来使用
const { URL } = require('url')

//判断全局对象属性的URL和从模块中导入进来的URL是否一样:
console.log(URL === globalThis.URL); // 打印 'true'.

const urlString = 'https://www.baidu.com:443/path/index.html?id=2&name=yyy#tag=3'

console.log(new URL(urlString))

//url地址的所有信息(如下):
URL {
  href: 'https://www.baidu.com/path/index.html?id=2&name=yyy#tag=3',
  origin: 'https://www.baidu.com',
  protocol: 'https:',
  username: '',
  password: '',
  host: 'www.baidu.com',
  hostname: 'www.baidu.com',
  port: '',
  pathname: '/path/index.html',
  search: '?id=2&name=yyy',
  searchParams: URLSearchParams { 'id' => '2', 'name' => 'yyy' },   
  hash: '#tag=3'
}
```

**从结果可以知道url地址解析后的所有信息相当于浏览器下的location对象里的所有属性。**



#### 2.4.2 解析后的url对象属性

`URL构造函数` 解析url地址后会得到一个`URL实例`，该实例中包含有该url地址相关信息的属性。

各属性总结：

- `href`：获取和设置完整的url网址。
- `origin`：获取url网址的源的序列化（可读）。
- `protocol`：获取和设置url网址的协议部分。
- `username`：获取和设置url网址的用户名部分。

- `password`：获取和设置url网址的密码部分。
- `host`：获取和设置url网址的服务器名及端口号。
- `hostname`：获取和设置url网址的服务器名部分。
- `post`：获取和设置url网址的端口部分。各协议的端口号："ftp"=>21，"http"=>80，"https"=>443，"ws"=>80，"wss"=>443。
- `pathname`：获取和设置url网址的路径部分。
- `search`：获取和设置url网址的查询参数部分。该查询部分是以问号开头。
- `searchParams`：获取表示url网址查询参数部分（对象形式），此属性是只读的。
- `hash`：获取和设置url的哈希值。

示例：

```js
const urlString = 'http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=javascript#contents'

const myURL = new URL(urlString)

console.log(myURL)
//各属性的结果如下
URL {
  href: 'http://foouser:barpassword@www.wrox.com/WileyCDA/?q=javascript#contents',
  origin: 'http://www.wrox.com',
  protocol: 'http:',
  username: 'foouser',
  password: 'barpassword',
  host: 'www.wrox.com',
  hostname: 'www.wrox.com',
  port: '',
  pathname: '/WileyCDA/',
  search: '?q=javascript',
  searchParams: URLSearchParams { 'q' => 'javascript' },
  hash: '#contents'
}
```



#### 2.4.3 url模块常用的方法

`url模块` 常用的方法基本跟处理字符串的方法很多，这里就建议查阅官方文档使用。

