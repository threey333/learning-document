# ECMAScript6后部分内容

## 一、promise对象

### 1.1 Promise的含义

#### #Promise的含义

- Promise是异步编程的一种解决方案，相比于传统的解决方案`---回调函数和事件---`，Promise更合理和更强大。
- Promise简单来说就是一个容器，这个容器里面保存着某个未来才会结束的事件结果。（这个事件一般是异步操作）
- 从语法上看，Promise是一个对象，从它可以获取异步操作的消息。
- **Promise提供了统一的API，各种异步操作都可以用同样的方法进行处理。**

#### #Promise对象的特点

特点一：Promise对象有三个状态：`pending(进行中)`、`fulfilled(已成功)`、`rejected(已失败)`。**最重要的是Promise对象的状态不受外界影响。**

特点二：一旦状态改变，就不会在再变，任何时候都可以得到这个结果。这个结果就是状态的结果。

>有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。
>
>此外，Promise对象提供统一的接口，使得控制异步操作更加容易。



#### #Promise对象的缺点

- 一旦建立了Promise对象，它就会立即执行，无法中途取消。
- 在Promise构造函数内，要设置异步的回调函数，不然Promise内部抛出的错误，不会反应到外部。
- 当处于penging状态时，你无法知道目前进展到哪一个阶段。



### 1.2 Promise的基本用法

#### # 基本用法

ES6规定，==Promise对象是一个构造函数，用来生成Promise实例==。

示例：

```js
new Promise((resolve, reject) => {
  // Promise构造函数里写异步操作
  setTimeout(() => {
    // console.log('成功了');
    reject('失败了')
  })
  // resolve('延时器成功执行')
}).then(val => {
  console.log(val);
}).catch(reason => {
  console.warn(reason);
})
```

#### # 新建后就会立即执行

Promise 新建后就会立即执行。

怎么知道它是会立即执行的？回答是比较就知道了。

```js
const p1 = new Promise((resolve, reject) => {
  console.log('Promise构造函数不是异步函数');
  resolve()
})

p1.then(() => {
  console.log('resolved.');
})

console.log('外部的同步任务');

//结果：Promise构造函数不是异步函数
//     外部的同步任务
//     resolved.
```

#### # then方法指定不同状态的回调函数

```js
promise.then((value) => {
    success
}, (error) => {
    failure
})
```



### 1.3 使用Promise封装ajax

接口地址：https://api.apiopen.top/getJoke

#### #原生封装

```js
function loadGetJoke(url) {
  const p = new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest()
    // 对象初始化,定义请求类型和请求地址
    xhr.open('get', url)
    // 发送请求
    xhr.send()
    xhr.onload = function () {
      if (xhr.status >= 200 && xhr.status < 400) {
        // 如果成功调用resolve函数
        resolve(xhr.response)
      } else {
        reject(xhr.response)
      }
    }
  })
  return p
}

loadGetJoke('https://api.apiopen.top/getJoke')
  .then(res => {
  console.log(JSON.parse(res))
})
  .catch(err => {
  console.log(JSON.parse(err))
})
```

#### #JQuery封装

```js
function fetch(options) {
  const p = new Promise((resolve, reject) => {
    $.ajax({
      // 接口
      url: options.url,
      // 类型
      type: options.type || 'GET',
      // 参数
      data: options.data || {},
      // 请求头
      headers : options.headers || {},
      // 返回类型
      dataType: 'json',
      // 成功回调
      success: resolve,
      // 失败回调
      error: reject
    })
  })
  return p
}

fetch({
  url:'http://localhost:8888/api/private/v1/login?username=admin&password=123456'
}).then(res => {
  console.log(res)
})
```



### 1.4 resolve函数的参数情况

**resolve函数的参数除了正常的值以外，还可能是另一个Promise实例**

```js
const p1 = new Promise((resolve, reject) => {
  // .....
})
const p2 = new Promise((resolve, reject) => {
  resolve(p1)
})
```

如果是以上这种情况就要分清楚情况：

1. 如果p1的状态是pending，那么p2的回调函数就会等待p1的状态改变，如果p1的状态改变为rejected则会调用catch的回调函数，如果是fulfilled则会调用then的回调函数。
2. 如果p1的状态是fulfilled或者rejected，那么p2的回调函数就会立即执行。

示例：

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    // resolve('等了2秒终成功了')
    reject(new Error('等了2秒失败了'))
  }, 2000)
})
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    // resolve的参数是另一个promise对象
    resolve(p1)
  }, 1000)
})

p2.then(res => {
  console.log(res)
}).catch(err => {
  console.log(err)
})
//结果：Error: 等了2秒失败了
```

以上代码，p2等待p1的状态改变，然后执行相关的异步操作回调函数。这里p1状态改变为了`reject`，从而执行了`catch`异步操作的回调函数。

**这种情况叫做==状态依赖==。**



### 1.5 Promise的then方法

1.Promise实例的then方法是定义在原型对象上的。它的作用是当Promise实例对象的状态改变时为其添加回调函数。

2.then方法有两个参数，一个是resolved状态的回调函数，另一个是rejected状态的回调函数。

```js
const p = new Promise((res, rej) => {
  setTimeout(() => {
    res('哈哈')
  }, 0)
})
const i = p.then(value => {
  console.log(value);
}).catch(err => console.log(err))
```

> **3.then方法返回的是一个promise实例对象(注意：不是原来的Promise实例对象。)因此可以采用链式调用写法，即then方法后面再调用另一个then方法。**

示例：

```js
p.then(value1 => {
  console.log('then1:' + value1);
  return p
}).then(value2 => {
  console.log('then2:' + value2);
  return p
}).then(value2 => {
  console.log('then2:' + value2);
  return p
}).catch(err => {
  console.log(new Error('出错了'));
  return p
})
```



### 1.6 Promise的catch方法

一、`Promise.prototype.catch()` 方法是 `.then(null, rejection)`或 `.then(undefined, rejection)` 的别名，用于指定发生错误时的回调函数。

```js
const p = new Promise((resolve, reject) => {
  // 这里放置异步操作
  setTimeout(() => {
    reject(new Error('1秒后出错啦.....'))
  }, 1000);
})
// p.then(res => {
//   console.log(res);
// }, err => {
//   console.log(err);
// })
// 上面等同于
p.then(res => {
  console.log(res);
}).catch(err => {
  console.log(err);
})
```

二、如果Promise状态已经变成resolved，这时再抛出错误是无效的。

```js
const promise = new Promise(function (resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function (value) { console.log(value) })
  .catch(function (error) { console.log(error) });
//结果： ok
```



三、==**一般来说，不要在then()方法里面定义 Reject 状态的回调函数（即then的第二个参数），使用catch方法才是主流的做法。**==

所以以后在then的回调函数中，不要再定义第二个参数，而是采用catch方法,理由是这种写法，catch可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch）。

```js
promise
  .then(function (data) { //cb
  // success
})
  .catch(function (err) {
  // error
});
```



四、**Promise内部的错误不会影响到Promise外部的代码**，通俗的说法就是”Promise会吃掉错误“

```js
const p = new Promise((resolve, reject) => {
  // 由于x是未声明的，以至于会报错
  resolve(x + 2)
})
p.then(() => {
  console.log('哈哈哈')
})

setTimeout(() => {
  console.log(123)
}, 1000)
//结果：Uncaught (in promise) ReferenceError: x is not define
//123
```

简单的来说就是：==Promise内部发生错误时，同时又没有catch捕获错误，这时这个内部错误不会影响Promise外部代码的执行==，就像上面过了1秒依旧可以打印出123。

>- 如果这个脚本放在服务器执行，退出码就是0(即表示执行成功)。不过，Node.js有一个 `unhandledRejection事件`，专门监听未捕获的reject错误。利用这个事件可以监听函数里面抛出的错误。
>
>  ```js
>  process.on('unhandledRejection', function (err, p) {
>    throw err;
>  });
>  ```
>
>  结果就是：Promise外部的代码被影响了，123并不会输出。
>
>- 注意，Node 有计划在未来废除unhandledRejection事件。如果 Promise 内部有未捕获的错误，会直接终止进程，并且进程的退出码不为 0。



五、一般总是建议，Promise对象后面要跟catch方法，这样可以处理Promise内部发生的错误。

同时**catch方法返回的还是一个Promise对象，因此后面还可以接着调用then方法。**

```js
const p = new Promise((resolve, reject) => {
  resolve(x + 2)
})
p.catch(err => {
  console.log('oh on', err);
}).then(() => {
  console.log('继续进行');
})
//结果：
//     oh no [ReferenceError: x is not defined]
//     继续进行
```



六、当Promise的异步操作catch方法和then方法都存在时，如果没有报错，则会跳过catch方法

```js
new Promise((resolve, reject) => {
  resolve()
}).catch(err => {
  console.log('oh no', error);
}).then(() => {
  console.log('carry on');
})
```



七、`catch()` 方法无法捕获异步函数中抛出的错误。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    throw new Error('出错了')
  }, 1000)
})
p1.catch(e => {
  console.log(e)
})
// 结果: .catch无法捕获setTimeout()抛出的错误。即->Uncaught Error: 出错了
```

解决的办法：在 `setTimeout` 回调函数中将该错误 `reject` 出去，这样 `catch()` 方法就能捕获该错误。

```js
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error('出错了'))
  }, 1000)
})
// 捕获错误成功
p1.catch(e => {
  console.log(e.message) // => '出错了'
})
```

**结论：**

>**宏任务**只针对可控的 `reject` 错误捕获，`throw Error` 接不住。



八、`catch()` 方法之中，还能再抛出错误

```js
const someAsyncThing = function () {
  return new Promise((resolve, reject) => {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2)
  })
}
someAsyncThing().then(() => {
  return someAsyncThing()
}).catch((err) => {
  console.log('oh no', err);
  y + 2
}).then(() => {
  console.log('carry on');
})
//结果：oh no [ReferenceError: x is not defined]，
```

上面代码中，catch()方法抛出一个错误，因为后面没有别的catch()方法了，导致这个错误不会被捕获，也不会传递到外层。如果改写一下，结果就不一样了。

修改后：

```js
someAsyncThing()
  .then(function () {
  return someOtherAsyncThing()
})
  .catch(function (error) {
  console.log('oh no', error)
  // 下面一行会报错，因为y没有声明
  y + 2
})
  .catch(function (error) {
  console.log('carry on', error)
})
//结果：   ReferenceError:参考错误
// oh no [ReferenceError: x is not defined]
// carry on [ReferenceError: y is not defined]
```

上面代码中，第二个catch()方法用来捕获前一个catch()方法抛出的错误。



### 1.7 Promise的finally方法

1. finally()方法用于指定==不管Promise对象最后状态如何，都会执行操作==。该方法是ES2018引入标准的。

2. finally()方法的回调函数不接受任何参数，这意味着没有办法知道，前面的Promise状态到底是fulfilled。

3. finally方法本质上还是then方法的特例。示例：

   ```js
   new Promise((resolve, reject) => {
     console.log(1)
   }).finally(() => {
     console.log('第一：执行了')
   })
   // 等同于
   new Promise((resolve, reject) => {
     console.log(1)
   }).then(
     result => {
       console.log(result)
       return result
     },
     err => {
       console.log(err)
       throw err
     }
   )
   ```

4. finally方法总是会返回原来的值。

   ```js
   const p = new Promise((resolve, reject) => {
     resolve(2)
   })
   console.log(p.then(() => {}))
   // 结果：[[PromiseResult]]: undefined
   console.log(p.finally(() => {}))
   // 结果：[[PromiseResult]]: 2
   ```

   原因是：**finally方法内部永远是 返回成功的值 或者 抛出错误的结果。**



### 1.8 Promise.resolve方法

#### #将现有对象转Promise对象

有时需要将现有对象转为Promise对象，而 `Promise.resolve()`方法就能起到这个作用。

示例：

```js
const jsPromise = Promise.resolve($.ajax('/whatever.json'))
```

上面代码将 jQuery 生成的deferred对象，转为一个新的 Promise 对象。



#### #resolve方法参数的四种情况

**第一种情况：`参数是一个Promise实例`：**

如果参数是Promise实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。

**第二种情况：`参数是一个thenable对象`：**

==Promise就是一个thenable对象。==

`thenable对象` 指的是具有then方法的对象，比如：

```js
const thenable = {
  then(resolve, reject) {
    resolve(42)
  },
}
```

`Promise.resolve() `方法会将这个对象转为Promise对象,然后立即执行thenable对象的then()方法。

```js
const p1 = Promise.resolve(thenable)
p1.then(value => {
  console.log(value)
})
//结果：42
```

**注意：**

>因为对 `thenable` 的处理需要在一个微任务中完成，所以调用 `thenable的then方法` 也是异步微任务。



**第三种情况：`参数不具有then()方法的对象，或者根本不是对象`**

如果参数是一个原始值，或者是一个不具有then()方法的对象，则Promise.resolve()方法会返回一个新的Promise对象，状态为`resolved`。

```js
const p = Promise.resolve('yyy')
p.then(result => {
  console.log(result)
})
//结果：yyy
```

Promise.resolve()方法的参数会同时传给回调函数的then方法，所以这里打印的是 yyy

**第四种情况：`不带有任何参数`**

Promise.resolve()方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。

所以，==如果希望得到一个 Promise 对象，比较方便的方法就是直接调用Promise.resolve()方法==。

```js
const p = Promise.resolve()

p.then(function () {
  console.log(p)
})
//结果：Promise {<fulfilled>: undefined}
```



#### #resolve方法的等价写法

Promise.resolve()等价于下面的写法。

```js
Promise.resolve('foo')
//等价于
new Promise(resolve => resolve('foo'))
```

**注意：这种等价写法只存在于`不是对象` 或者 `不具有then()方法的对象`。**



### 1.9 Promise.reject方法

reject函数的参数通常是`Error对象的实例`，表示抛出的错误；

一、`Promise.reject(reason)`方法也会返回一个新的Promise实例，该实例的状态为rejected

```js
const p = Promise.reject('出错了')
//等同于
const p = new Promise((resolve, reject) => {
  reject('出错了')
})

p.then(undefined, err => {
  console.log(err)
})
// 结果：出错了
```

二、Promise.reject()方法的参数，会原封不动地作为reject的理由，变成后续方法的参数。

```js
Promise.reject('出错了').catch(e => {
  console.log(e === '出错了')
})
// true

上面代码中，`Promise.reject()方法`的参数是一个字符串，后面catch()方法的参数e就是这个字符串。



### 1.10 事件循环

事件循环案例：

​```js
setTimeout(() => {
  console.log(333)
}, 0)

Promise.resolve().then(() => {
  console.log(222)
})

console.log(111)
// 结果：111
//      222
//      333
```

原因：etTimeout(fn, 0)在下一轮“事件循环”开始时执行，Promise.resolve()在本轮“事件循环”结束时执行，console.log('one')则是立即执行，因此最先输出。



### 1.11 Promise.all方法

> Promise.all方法用于将多个Promise实例，包装成一个新的Promise实例。

一、`Promise.all()` 方法可以接受一个数组作为参数，如下情况：

这里p1,p2,p3都当作是Promise实例

```js
const p = Promise.all([p1, p2, p3])
```



二、如果不是Promise实例对象，就会先调用 `Promise.resolve()` 方法将其转为Promise实例对象，再进一步处理。

示例：

```js
const p1 = [1, 2, 3, 4]
const p2 = 'threey'
const p3 = { age: 18, title: '学习promise.all' }
const resArr = Promise.all([p1, p2, p3])
resArr.then(res => {
  console.log(res) // => [Array(4), 'threey', {…}]
})
```

三、`Promise.all()` 方法的参数可以不是数组，但 ==**必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例**==。



四、Promise.all()方法返回的Promise实例对象的状态由**方法中的各Promise实例对象的状态决定**。

这里可以分为两种情况：(以 `const p = Promise.all([p1, p2, p3])`为例子，其中p1、p2、p3都当作是Promise实例对象。)

- 第一种情况：只有p1、p2、p3的状态都变成 `fulfilled`，p的状态才会变成`fulfilled`，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
- 第二种情况：只要p1、p2、p3之中有一个被 `rejected`，p的状态就变成 `rejected`，此时第一个被 `reject` 的 `promise实例` 的返回值，会传递给 `p`。 

示例：

```js
// 情况一: p1、p2、p3的状态都为fulfilled，则p的状态就为fulfilled。
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([1, 2, 3, 4])
  }, 30)
})
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('yyy')
  }, 1000)
})
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('zzz')
  }, 600)
})

const p = Promise.all([p1, p2, p3])
p
  .then(res => {
  console.log(res)  // 结果: [Array(4), 'yyy', 'zzz']
})
  .catch(e => {
  if (e) console.log('捕捉到了错误', e)
})

// 情况二: p1、p2、p3其中有一个被reject，p的状态就为rejected。
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve([1, 2, 3, 4])
  }, 30)
})
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('yyy')
  }, 1000)
})
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('111')
  }, 600)
})

const p = Promise.all([p1, p2, p3])
console.log('p的状态', p) // p的状态为rejected。
p.then(res => {
  console.log(res)
}).catch(e => {
  if (e) console.log('捕捉到了错误', e)
})
```



五、如果作为参数的Promise实例，自己定义了 `catch` 方法，那么它一旦被 `rejected`，并不会触发`Promise.all()` 的 `catch()`方法。

示例：

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello')
})
.then(result => result)
.catch(e => e)

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了')
})
.then(result => result)
.catch(e => e.message)

Promise.all([p1, p2])
  .then(result => console.log(result))
  .catch(e => console.log(e))

//结果:['hello', '报错了']
```

这里说明下：p1会resolved，p2会rejected,但是p2有自己的catch方法，这样它就会捕获它内部的错误，**同时catch方法返回新的promise实例**，这个实例的状态最终会变为resolved，最终导致Promise.all方法参数里面的两个实例都会resolved,因此会调用then方法指定的异步操作回调函数，而不是catch指定的回调函数。



六、如果p2没有自己的catch方法，那就会调用Promise.all()的catch方法。

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello')
}).then(result => result)

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了')
}).then(result => result)

Promise.all([p1, p2])
  .then(result => console.log(result))
  .catch(e => console.log(e))
// Error: 报错了
```



### 1.12 Promise简单的应用

使用Promise异步读取多个文件：

```js
// 异步读取文件
const fs = require('fs')
const { read } = require('fs/promises')

// 封装Promise
function readFileWay (url) {
  const p = new Promise((resolve, reject) => {
    fs.readFile(url, (err, data) => {
      if (err) {
        return reject('读取文件失败！')
      } else {
        return resolve(data)
      }
    })
  })
  return p
}

readFileWay('./读取文件1.txt').then(resolve1 => {
  console.log('文件1的内容:' + resolve1);
  return readFileWay('./读取文件22.txt')
}).then(resolve2 => {
  console.log('文件2的内容:' + resolve2);
  return readFileWay('./读取文件3.txt')
}).then(resolve3 => {
  console.log('文件3的内容:' + resolve3);
}).catch(err1 => {
  console.log('错误了' + err1);
}).catch(err2 => {
  console.log(err2);
})
```



## 二、async异步函数

### 2.1 async函数的含义

async和await是ES8提出来的。

作用：想编写同步代码的形式处理异步操作。这种处理方式更加彻底。

async 函数是什么？一句话，它就是 Generator 函数的语法糖。

其中 **`await命令`就是内部`then`命令的语法糖。**

**代码对比说明：**

>```js
>// promise版本：
>new Promise((resolve, reject) => {
>  const result = '我是promise中的值'
>  // resolve()回调方法改变promise状态为resolved
>  resolve(result)
>}).then(res => {
>  console.log(res) //我是promise中的值
>})
>
>//async/await版本：
>const demo3 = async () => {
>  const result = new Promise((resolve, reject) => {
>    setTimeout(() => {
>      resolve('我是promise内的值')
>    }, 1000)
>  })
>  // await命令内置了then微任务回调方法
>  const res = await result
>  console.log(res)  
>}
>demo3() //我是promise内的值
>```

`await命令` 相当于内置了 `Promise.then()方法`，以至于它可以获取 `Promise成功状态后的值`。



### 2.2 基本用法

**`async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。**

- async修饰一个(内部有异步操作的)函数  格式：async+函数
- await等待(等待一个异步处理的结果值)  格式：await+异步操作(Promise实例)

示例：异步读取文件信息

```js
const fs = require('fs')

function readFileWay (url) {
  const p = new Promise((resolve, reject) => {
    // 读取文件是异步操作
    fs.readFile(url, (err, data) => {
      if (err) {
        return reject('读取文件失败！')
      } else {
        return resolve(data)
      }
    })
  })
  return p
}
// 利用async和await改良代码
async function fn () {
  let res = await readFileWay('./文件内容1.txt')
  console.log(res.toString());
}
fn()
// 结果：哈哈
```



### 2.3 async函数的多种使用形式。

```js
// 函数声明
async function foo() {}

// 函数表达式
const foo = async function () {};

// 对象的方法
let obj = { async foo() {} };
obj.foo().then(...)

// Class 的方法
class Storage {
  constructor() {
    this.cachePromise = caches.open('avatars');
  }

  async getAvatar(name) {
    const cache = await this.cachePromise;
    return cache.match(`/avatars/${name}.jpg`);
  }
}

const storage = new Storage();
storage.getAvatar('jake').then(…);

// 箭头函数
const foo = async () => {};
```



### 2.4 async函数的语法

`async`函数的语法规则总体上比较简单，**难点是错误处理机制**。

#### 2.4.1 async函数内部的return语句情况问题

async函数返回一个Promise对象。同时async函数内部return语句返回的值，会成为then方法回调函数的参数。

示例：

```js
async function fn() {
  return 'hello world'
}
//由于async函数返回的是一个Promise对象，所以可以使用then回调方法。
//return语句返回的值会成为then方法回调函数的参数。
fn().then(result => {
  console.log(result)
})
结果是：hello world
```

> ==注意：==
>
> - 为什么async异步函数的return语句返回值会成为then方法异步操作回调函数的参数?
> - 原因是return语句使得async函数返回的Promise实例对象的状态变为了`resolved`，从而调用了then方法指定的回调函数，同时返回值成为该回调函数的参数。



#### 2.4.2 async函数内部抛出的错误问题

async函数内部抛出的错误，会导致返回的Promise实例的状态变为reject状态，并且抛出的错误对象会被catch方法回调函数接受。

```js
async function fn1() {
  throw new Error("出错了！");
}
fn1()
  .then((result) => {
  console.log(result);
})
  .catch((err) => {
  console.log(err);
});
// 结果：Error: 出错了！
```



#### 2.4.3 async函数返回的Promise 对象的状态变化

async函数返回的Promise对象，**必须等到内部所有的await命令后面的异步操作执行完，才会发生状态改变**。

也就是说，==只有async函数内部的异步操作都执行完，才会执行then方法的回调函数==。

示例：

```js
async function getData(url) {
  // await返回的还是Promise实例
  // 因为await命名后面跟的是Promise实例，所有await返回的是该实例的结果
  let result = await axios.get(url)
		return result
  }
console.dir(getData('https://api.apiopen.top/getJoke'))
// 上面的结果：
// [[PromiseState]]: "fulfilled"
// [[PromiseResult]]: Object
getData('https://api.apiopen.top/getJoke').then(result => {
  console.log('执行了！')
})
```



#### 2.4.4 async函数改变状态方式二。

async函数遇到`return语句`或者`抛出错误`会改变状态。

示例1：`return语句`会改变 async返回的Promise实例的状态变`resolved（fulfilled）`

```js
async function fn3() {
  return 'Promise对象改变了！'
}
console.log(fn3())
//结果：Promise { <state>: "fulfilled", <value>: "Promise对象改变了！" }
```

示例2：`抛出错误`会改变 async返回的Promise实例的状态变`rejected`

```js
async function fn3() {
  throw new Error
}
console.log(fn3())
//结果：[[PromiseState]]: "rejected" [[PromiseResult]]: Error: 出错啦!
```



### 2.5 await命令

#### 2.5.1 await命令后面是/不是Promise对象

- 正常情况下，**await命令后面是一个Promise对象，返回该对象的结果**。
- 如果不是Promise对象，则直接返回对应的值。

**注意：**

>`await命令`后面跟一个非 Promise 的值所以会自动包上一层 Promise.resolve。

示例：

```js
async function f() {
  return await 123 
  // 相当于 return await Promise.resolve(123)
  // 上面相当于return 123
}
f().then(result => {
  return result
})
//结果：123
```

**`await命令 `相当于内置了 `Promise.then`异步微任务回调函数 。**



#### 2.5.2 await命令后面是一个thenable对象

await命令后面是一个`thenable对象`(即定义了then方法的对象)，那么**await会将其等同于Promise对象**。

示例：

```js
class Sleep {
  constructor(timeout) {
    this.timeout = timeout
  }
  //Sleep类中的then方法。
  then(resolve, reject) {
    // 1.定义初始时间
    const startTime = Date.now()
    // 2.用延时器经过了多长时间
    setTimeout(() => {
      resolve(Date.now() - startTime)
    }, this.timeout)
  }
}
// 立即执行函数
;(async () => {
  // await返回该thenable对象的结果。虽然这个await命令后面的实例对象不是Promise对象，但是有then方法，于是await会将其视为Promise对象。
  const sleepTime = await new Sleep(1000)
  console.log(sleepTime) //结果是：1000附近浮动
})()
```

**`await命令 `相当于内置了 `Promise.then`异步微任务回调函数。** 

借助await命令就可以让程序停顿指定的时间:

```js
function sleep(interval) {
  return new Promise(resolve => {
    setTimeout(resolve, interval)
  })
}
// 用法
async function one2FiveInAsync() {
  for (let i = 1; i <= 5; i++) {
    console.log(i)
    await sleep(1000)
  }
}

one2FiveInAsync()
//结果：每1秒打印一个数
```



#### 2.5.3 await命令易错地方

首先要明确：==**await命令要想起到等待效果，后面必须跟上Promise对象的异步回调函数。**==

什么意思？来用代码说明：

```js
const demo = async () => {
  let result = await setTimeout(
    () => console.log('我推迟了1秒执行'),
    1000
  )
  console.log('我在等待上面的任务执行后才执行')
}
demo()

//结果：1.我在等待上面的任务执行后才执行
//     2.我推迟了1秒执行
```

从上面的结果你会发现，`await命令`根本没有起到等待的作用。原因就是：`await命令` 后面跟的不是一个Promise对象的异步回调函数。

代码改进：

```js
const demo = async () => {
  let result = await new Promise(resolve => {
    setTimeout(() => {
      resolve('我推迟了1秒执行')
    }, 0)
  })
  console.log('result')
  console.log('我在等待上面的任务执行后才执行')
}
demo()
//结果：1.我推迟了1秒执行
//      2.我在等待上面的任务执行后才执行
```



#### 2.5.4 async/await等价转换promise写法

因为 `await命令` 内置了 `then微任务回调函数`，所以可以类似转换：

转换一：`await命令` 后面不是 `Promise对象的异步回调函数`，**则会自动包上一层 Promise.resolve()**。

>async/await版本：
>
>```js
>const fun1=async()=>{
>      const res=await 123
>      console.log(res)
>      console.log('微任务中的同步代码')
>}
>fun1()
>```
>
>**上面等价于：**
>
>```js
>const fun1 = async () => {
>      const res = await Promise.resolve(123)
>      console.log(res)
>      console.log('微任务中的同步代码')
>}
>fun1()
>```
>
>**还可以等价Promise：**
>
>```js
>const fun1 = () => {
>      return Promise.resolve(123).then(res => {
>         console.log(res)
>        console.log('微任务中的同步代码')
>       })
>     }
>    fun1()
>```

转换二：`await命令`后面是 `Promise对象的异步回调函数`。

>async/await版本：
>
>```js
>const fn1 = () => {
>    console.log('A') 
>    return 'B'
>}
>const fn2 = async () => {
>    console.log('C') 
>    return Promise.resolve('D')
>}
>const result = async () => {
>    console.log('start....') 
>    let r1 = await fn1()
>    console.log(r1) 
>    let r2 = await fn2()
>    console.log(r2)
>}
>
>result()
>console.log('Z')
>
>new Promise((resolve, reject) => {
>    console.log('E') 
>    resolve('F')
>})
>    .then(res => {
>    console.log(res) 
>    return Promise.resolve('G')
>})
>    .then(res => {
>    console.log(res) 
>})
>//
>//结果：  先执行start....
>//       当指针指向let ri=await fn1()，然后调用fn1函数，打印A后，返回一个B给await命令。
>//       但是await命令是异步微任务，这时它进入微任务队列，同时因为await的存在，其后面的代码进行等待不执行，继而跳出
>//       async函数执行其它的同步代码。
>//       打印Z、E。当执行resolve(’F')，Promise状态改变，同时有then异步微任务，将其放入微任务队列中。
>//       此时，第一个宏任务中的同步代码执行完了，进而执行微任务。因为微任务队列中第一个是await 'B',进而打印B，然后执行往下执行，然后执行fn2函数，打印c、返回一个Promise对象给await命令。因为await命令是异步微任务，进入将其放入微任务队列中等待执行。
>//       因为await的存在，又开始跳出async函数进而执行其它任务，因为发现执行微任务再等待执行，所以取第二个微任务到执行栈中执行，打印F，同时返回Promise对象给下一个then异步微任务。
>//       该任务也是被推入微任务队列中等待执行。
>//       最好打印D、G
>
>
>//总顺序：start...、A、Z、E、B、C、F、D、G
>```
>
>**上面等价转换Promise：**
>
>```js
>const fn1 = () => {
>    console.log('A')
>    return 'B'
>    }
>    const fn2 = async () => {
>    console.log('C')
>      return Promise.resolve('D')
>    }
>  const result = async () => {
>  console.log('start....')
>    Promise.resolve(fn1()).then(result => {
>        console.log(result)
>      Promise.resolve(fn2()).then(result => {
>          console.log(result)
>        })
>    })
>    }
>result()
>console.log('Z')
>new Promise((resolve, reject) => {
>  console.log('E')
>  resolve('F')
>  })
>    .then(res => {
>  console.log(res)
>    return Promise.resolve('G')
>  })
>    .then(res => {
>  console.log(res)
>  })
>  
>//总顺序：start...、A、Z、E、B、C、F、D、G
>```

**等价转换总结：**

- 在 ` async/await函数` 中的 `await命令` **现在语义上等价于 `Promise.resolve(XXX)`**，而不是 `new Promise(resolve=>resolve(XXX))`。

- `await命令` 后面是 `async函数并且返回Promise.resolve(XXX)`（如下图），那么**await异步微任务要等两轮时序微任务才进入微任务队列中等待执行。**

  ![image-20210909223051102](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210909223103.png)



#### 2.5.5 await命令后面的Promise对象状态为rejected

await命令后面的Promise对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到。

原因是：async函数遇到return语句或者抛出错误会改变状态，这时async函数返回的Promise对象的状态为reject，错误对象会被catch回调函数捕获到。

```js
async function fn6() {
  await Promise.reject('出错了！')
}
fn6()
  .then(result => console.log(result))
  .catch(err => {
  console.log(err)
})
//结果：出错了！
```



#### 2.5.6 状态为reject会中断async函数的执行

任何一个`await命令`后面的Promise对象的状态变为`reject状态`，那么整个async函数都会中断执行。

```js
async function fn7() {
  await Promise.reject('出错了!')
  await Promise.resolve('Hello world') //不会执行
}
fn7()
  .then(result => {
  console.log(result)
})
  .catch(err => {
  console.log(err)
})
//结果：出错了!
```

- 那有时我们希望即使有一处的异步操作失败，也不要中断后面的异步操作。
- 做法是将其放在`try...catch结构`里面。这样不管这个异步操作是否成功，都不会影响其它的await语句的执行。

示例：

```js
async function fn8() {
  try {
    await Promise.reject('出错了!')
  } catch (e) {
    console.log(e)
  }
  return await Promise.resolve('hello world')
}
fn8().then(v => console.log(v))
```

- 另一种方法是：await命令后面的Promise对象再跟一个catch方法，处理前面可以出现的错误。

示例：

```js
async function fn9() {
  await Promise.reject('出错了').catch(e => console.log(e))
  return await Promise.resolve('hello world')
}

fn9().then(v => console.log(v))
// 出错了
// hello world
```



### 2.6 错误处理

#### 2.6.1 错误处理

一、前面也说了，async函数的状态会随着函数内部的异步操作出错而状态改变为reject，此时就约等同于async函数返回的Promise对象被reject。

```js
async function fn10() {
  await new Promise((resolve, reject) => {
    reject(new Error('出错了'))
  })
}
fn10()
  .then(v => {
  console.log()
})
  .catch(e => {
  console.log(e)
})
```

二、防止出错的方法，也是将其放在try...catch代码块之中

```js
async function fn11() {
  try {
    await new Promise(function (resolve, reject) {
      throw new Error('出错了')
    })
  } catch (e) {}
  return await 'hello world'
}
```

三、如果有多个await命令，可以统一放在try...catch结构中。

```js
async function main() {
  try {
    const val1 = await firstStep()
    const val2 = await secondStep(val1)
    const val3 = await thirdStep(val1, val2)
    console.log('Final: ', val3)
  } catch (err) {
    console.log(err)
  }
  console.log('Final: ', 1)
}
main().then(v => console.log(v))
//结果：1
```

上面的代码，即使val1报错也不影响`try...catch结构外的`代码执行。



#### 2.6.2 使用注意点

- 一、前面已经说过了，await命令后面的Promise对象，状态可能是rejected,所以最好把await命令放在try...catch代码块中。
- 二、多个await命令后面的异步操作，如果不存在`继发关系`，最好让它们同时触发。

这里有个问题？什么是继发关系？用个例子就会明白了！

```js
async function fn12() {
  // 这里的firstStep、secondStep、thirdStep当作异步操作函数。
  const val1 = await firstStep()
  const val2 = await secondStep(val1)
  const val3 = await thirdStep(val1, val2)
}
```

al1在secondStep异步操作中要用到，而val2在异步操作thirdStep中要用到，这种就是继发关系（之间存在依赖）

- 为什么不是继发关系的最好让它们同时触发？因为不同时触发会比较耗时。
- 就像上面的情况一样，只有firstStep完成以后才会执行secondStep，当secondStep完成后也才会执行thirdStep。

**所以互相不依赖的完全可以让它们同时触发。**

同时触发的方式一：利用`Promise.all方法`

```js
let [foo, bar] = await Promise.all([getFoo(), getBar()])
```

同时触发的方式二：

下面代码中的`getFoo()`和`getBar()`当作异步操作回调函数

```js
let fooPromise = getFoo()
let barPromise = getBar()
let foo = await fooPromise
let bar = await barPromise
```

上面两种写法，getFoo和getBar都是同时触发，这样就会缩短程序的执行时间。



三、await命令只能用在async函数之中，如果用在普通函数，就会报错。

```js
async function dbFuc(db) {
  let docs = [{}, {}, {}];
  // 报错
  docs.forEach(function (doc) {
    await db.post(doc);
  });
}
```

但是如果将forEach方法的参数改成async函数，也会有问题。

```js
async function dbFuc(db) {
  let docs = [{}, {}, {}]
  // 可能得到错误结果
  docs.forEach(async function (doc) {
    await db.post(doc)
  })
}
```

上面代码可能不会正常工作，原因是这时三个db.post()操作将是并发执行，也就是同时执行，而不是继发执行

> 正确的写法是采用for循环:
>
> ```js
> async function dbFuc(db) {
>   let docs = [{}, {}, {}]
> 
>   for (let doc of docs) {
>     await db.post(doc)
>   }
> }
> ```



四、如果确实希望多个请求并发执行，可以使用Promise.all方法。当三个请求都会resolved时，下面两种写法效果相同。

写法一：

```js
async function dbFunc(db) {
  let docs = [{}, {}, {}]
  let promises = docs.map(doc => db.post(doc))

  let results = await Promise.all(promises)
  console.log(results)
}
```

写法二：

```js
async function dbFunc1(db) {
  let docs = [{}, {}, {}]
  let promises = docs.map(doc => db.post(doc))

  let results = []
  for (let promise of promises) {
    result.push(await promise)
  }
  console.log(results)
}
```



### 2.7 async函数可以保留运行堆栈。

一、没有用async函数的情况：

下面代码中，假设b()是一个异步任务。

```js
const a=()=>{
  b().then(()=>{
    c()
  })
}
```

`函数a() `内部运行一个 `异步任务b()` 。而异步任务它会开启异步线程执行该任务，等该任务完成后会返回一个异步回调函数回来到消息队列中，等待主线程处理执行。

所以问题就来了：

- `函数a()` 是同步任务，它并没有中断，而是继续执行。也就是说，`异步任务b()` 执行完毕后，可能 `函数a()` 已经执行完了。
- 这样 `异步任务b()` 所在的上下文环境已经消失了。如果`b()`或`c()`报错，错误堆栈将不包括`a()`。

图示：

![image-20210622233437026](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021235.png)

![image-20210622233728109](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021238.png)



二、使用async函数：

```js
const a=async()=>{
  await b()
  c()
}
```

结果：上面的代码b()运行的时候，a()是涨停执行，上下文环境都保存着。一旦b()或c()报错，错误堆栈包括a()。

图示：

![image-20210622234017469](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021249.png)



### 2.8 async函数的实现原理

async函数的实现原理，将Generator函数和自动执行器，包装在一个函数里。

```js
async function fn(args) {
  // ...
}
//等同于
function fn(args) {
  return spawn(function* () {
    // .....
  })
}
```

所有的async函数都可以写成上面的第二种形式，其中的spawn函数就是自动执行器。



### 2.9 按顺序完成异步操作

在此之前先要认识一下`fetch方法`

#### 2.9.1 认识fetch方法

fetch是用来取代传统的XMLHttpRequest的。 它的优点很多，包括链式调用的语法、返回promise等。

一、fetch方法返回的是一个Promise对象

```js
let i = fetch('https://api.apiopen.top/getJoke', { method: 'get' })
console.log(i)
// 结果：
//[[PromiseState]]: "fulfilled"
// [[PromiseResult]]: Response
```

二、fetch方法的then会接收一个Response实例，第二个then接收的才是后台传过来的真正数据。一般第一个then的异步操作是对数据进行处理。

示例：

```js
fetch('https://api.apiopen.top/getJoke', { method: 'get' })
  .then(res => {
  return res.json()
})
  .then(data => {
  console.log(data.result)
})
```



#### 2.9.2 按顺序完成异步操作

一、利用async函数实现 按顺序完成异步操作

```js
async function logInOrder(urls) {
  for (const url of urls) {
    const response = await fetch(url)
    .then(res => res.json())
    .then(data => data.result)
    console.log(await response)
  }
}
logInOrder(['https://api.apiopen.top/getJoke'], { method: 'get' })
```

上面的代码虽然简化了很多，但是问题来了，所有的远程操作都是继发的。只有前一个URL返回结果才会去读取下一个URL，这样做效率很差，非常浪费时间。

二、这时我们可以采用并发发出请求。

```js
async function logInOrder(urls) {
  // 并发读取远程URL
  const textPromises = urls.map(async url => {
    // fetch方法返回的是一个Promise对象
    const response = await fetch(url)
    .then(res => {
      return res.json()
    })
    .then(data => data.result)
    return response
  })
  // 按次序输出
  for (const textPromise of textPromises) {
    console.dir(await textPromise)
  }
}
logInOrder([
  'https://api.apiopen.top/getJoke',
  'https://api.apiopen.top/getJoke',
  'https://api.apiopen.top/getJoke',
])
```

- 虽然map方法的参数是async函数，但它是并发执行的，因为只有async函数内部是继发执行，外部不受影响。
- 后面的for...of循环内部使用了await命令，直接把promise对象的值输出，因此实现了按顺序输出。



## 三、class类

### 3.1 认识class类

一、ES6的 `class` 可以看作是一个语法糖，它的绝大部分功能，ES5都可以做到。

示例：

```js
// ES5写法
function Point(x, y) {
  this.x = x
  this.y = y
}
Point.prototype.toString = function () {
  return `(${this.x},${this.y})`
}
const p1 = new Point(2, 3)
console.log(p1) //=> Point {x: 2, y: 3}

//ES6写法
class Point {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
  toString() {
    return `(${this.x},${this.y})`
  }
}
const p2 = new Point(1, 3)
console.log(p2) //=> Point {x: 1, y: 3}
```

二、ES6的 `class类` 完全可以看作是构造函数的另一种写法。

```js
//检测ES6class类

class Point {}
// 使用typeof检测
console.log(typeof Point) //=> "function"

// 使用Object.prototype.toString.call()方法检测
console.log(Object.prototype.toString.call(Point)) //=>"[object Function]"

// 使用constructor检测，该检测结果是：返回创建实例对象的构造函数的引用
console.log(Point === Point.prototype.constructor)
```

三、ES6的class类的所有方法都定义在类的prototype属性上。

```js
class Foo {
  constructor() {}
  toString() {}
  toValue() {}
}
console.dir(Foo)
```

结果示意图：

![image-20211114200816699](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211114200843.png)

四、在class类中定义的方法，都是不可枚举的（non-enumerable）。而ES5中通过prototype对象添加属性是可枚举的。

```js
//检测class类内部定义的方法是否可枚举的
//方法：可以使用Object.keys()方法检测。该返回自身对象的所有可枚举的属性的键名。

class Foo {
  constructor() {}
  toString() {}
  toValue() {}
}
console.log(Object.getOwnPropertyNames(Foo.prototype)) //=>['constructor', 'toString', 'toValue']
// Object.getOwnPropertyNames()方法返回的指定对象中的所有自身属性（包含可枚举和不可枚举的属性）组成的数组

console.log(Object.keys(Foo.prototype)) //[]
// 从上面的两个方法进行比较知道，在class类中定义的方法都是不可枚举的属性。
```

五、`class类` 必须使用new调用执行，否则会报错。

```js
class Foo {
  constructor() {}
}
Foo() //报错
```



### 3.2 class类中的constructor构造方法

一、`constructor方法` 是类的默认方法，它相当于是类的构造函数；通过new命令生成对象实例时，自动调用该方法。

二、一个类必须要有 `constructor方法` ，如果没有显式定义，则一个 `空的constructor方法` 会被默认添加。

```js
//constructor方法会默认添加:

class Bar {} //没有显式定义constructor构造函数，则会默认添加。
console.dir(Bar)

// 上面等同于下面
class Bar {
  constructor() {}
}
```

三、`constructor方法` 默认返回的是实例对象，但是可以通过修改，使其返回其它的对象。

```js
class Bar {
  constructor() {
    return new Array() //返回对象被修改了。
  }
}
const bar = new Bar()
//检测方法一：
console.log(bar instanceof Bar) //=>fasle
console.log(bar instanceof Array) //=>true
//检测方法二：
console.dir(bar.constructor) //=>Array()
console.log(bar.constructor === Bar) //=>"false"
```



### 3.3 类的实例

和ES5一样，类的所有实例都共享一个原型对象。

```js
class Point {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
  show() {
    console.log(`(${this.x},${this.y})`)
  }
}
const p1 = new Point(1, 2)
const p2 = new Point(2, 1)
console.log(p1.__proto__ === p2.__proto__) //=>true
```



### 3.4 类的取值方法和存值方法

与ES5一样，在类的内部可以使用 `get关键字` 和 `set关键字`。

该关键字是对某个属性设置存值方法和取值方法，**简单来说就是拦截该属性的存取行为**。

```js
class MyClass {
  constructor(value) {
    this.message = value
  }
  get prop() {
    return this.message
  }
  set prop(value) {
    console.log('setter:', value)
  }
  show() {
    console.log('hahaha....')
  }
}
const inst = new MyClass('Hello world')

inst.prop = 123 //=>"setter: 123"

console.log(inst.prop) //=>"getter"
```



### 3.5 属性表达式

类的属性名，可以使用表达式的形式。

```js
let methodName = 'getArea'

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```



### 3.6 class类中要注意地方

一、类和模块内部默认采用的是严格模式，所以不需要使用 `use strict`指定运行模式。

二、`class关键字` 不存在变量提升。

```js
const foo = new Foo() //报错
class Foo {}
```

三、`name属性` 总是返回紧跟在class关键字后面的类名。

```js
class Point {}

console.log(Point.name) //=>Point
```

四、如果在某个方法之前加上星号（`*`），表示改方法是一个 `Generator函数`。

```js
class Foo {
  constructor(...args) {
    this.args = args;
  }
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}

for (let x of new Foo('hello', 'world')) {
  console.log(x);
}
// hello
// world
```

五 、this指向问题。

类的方法内部的this默认指向的是类的实例，但是，一旦单独使用类实例的方法，很可能会报错。

这是因为函数的独立调用this指向的是全局对象，而类内部采用的是严格模式，所以指向的是undefined。

```js
class Logger {
  printName(name = 'there') {
    console.log(this)
    this.print(`Hello ${name}`)
  }

  print(text) {
    console.log(text)
  }
}

const logger = new Logger()
logger.printName() //类的实例对象调用该方法，this指向的是logger

const { printName } = logger //当从对象解构一个方法出来独立调用时，this的指向为undefined,以至于很可能会报错。

printName() // => undefined
```



### 3.7 静态方法

一、静态方法：方法不会被实例对象享有，而是**直接通过类来调用。**

类的实例对象不能调用该类的静态方法，这是因为实例对象没有享有到类的静态方法，结果就会报错。

```js
class Foo {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
  static toTitle() {
    return '我是静态方法'
  }
  show() {
    return `(${this.x},${this.y})`
  }
}
const foo1 = new Foo(2, 3)

console.log(foo1) //=>{x: 2, y: 3}

foo1.toTitle() //=>Uncaught TypeError: foo1.toTitle is not a function

console.log(Foo.toTitle()) //=> "我是静态方法"
```

二、静态方法中的this指向的是类而不是实例。

```js
class Bar {
  static bar() {
    this.baz()
  }
  static baz() {
    console.log('类的静态方法')
  }
  baz() {
    console.log('实例对象上的baz()方法')
  }
}
Bar.bar() //=>"类的金泰方法"
```

三、父类的静态方法可以被子类继承：

```js
class Father {
  static show() {
    return 'hello'
  }
}
class Son extends Father {}

console.log(Son.show()) //hello
```

四、子类继承父类的静态方法也可以从 `super对象` 上进行调用

```js
class Father {
  static show() {
    return 'hello'
  }
}
class Son extends Father {
  static show() {
    return super.show() + ' world'
  }
}
console.log(Son.show()) // => "hello world"
```



### 3.8 实例属性的新写法

实例对象的属性除了定义在constructor()方法里面的this上面，还可以定义在类的最顶层。

```js
//写法一：实例对象属性定义在constructor方法里面的this上面
class IncreasingCounter {
  constructor() {
    this._count = 0
  }
  get value() {
    console.log('Getting the current value!')
    return this._count
  }
  increment() {
    this._count++
  }
}

//写法二：直接定义在类的最顶层，其它不变
class IncreasingCounter {
  _count = 0
  get value() {
    console.log('Getting the current value!')
    return this._count
  }
  increment() {
    this._count++
  }
}
const p1 = new IncreasingCounter()
console.log(p1)
```

**总结：**

>- 当不需要传递什么参数的时候，可以直接在类的最顶层定义属性。
>- 其它情况的时候，还是推荐使用 `constructor构造函数` 的形式。



### 3.9 静态属性

一、静态属性指定是Class本身的属性，即Class.propName，而不是定义在实例对象(this)上的属性。

**==注意：目前下面这种写法可行，因为ES6明确规定，class内部只有静态方法而没有静态属性。==**

```js
class Foo {}
Foo.name = '静态属性'
console.dir(Foo)
```

二、提案：类的静态属性通过在实例属性前面，加上 `static关键字`。

```js
class MyClass {
  static name = '静态属性'
  constructor() {
    console.log(MyClass.name)
  }
}
const myclass1 = new MyClass() //=>"静态属性"
```

**注意：**

> - 目前这个提案在某些浏览器中可以使用，但在某些浏览器中是不可以使用的。
> - 如果这个提案通过，那么这种新写法会大大方便了静态属性的表达。

 

三、静态属性的新老写法的比较：

```js
// 老写法
class Foo {
  // ...
}
Foo.prop = 1

// 新写法
class Foo {
  static prop = 1
}
```



### 3.10 私有属性和私有方法

私有属性和私有方法：只能在类的内部访问的属性和方法，外部不能访问。

但是ES6不提供私有属性和私有方法的直接实现办法，只能通过变通的方式模拟实现。

#### 3.10.1 私有属性模拟办法一

办法一：在命名上加以区别。

```js
class Point {
  // 公有方法
  foo(x) {
    this._bar(x)
  }
  // 私有方法
  _bar(y) {
    console.log(y)
    return (this.name = y)
  }
}
const p1 = new Point()
console.log(p1.foo('hello world'))

//有权访问私有属性和私有方法的公有方法，我们可以称为特权方法。
```

#### 3.10.2 私有属性模拟办法二

办法二：将私有方法移出类。

```js
class Widget {
  //公有方法
  foo(baz) {
    bar.call(this, baz)
  }

  // ...
}

function bar(baz) {
  return (this.snaf = baz)
}
const widget = new Widget()
widget.foo('hello world')
```

上面代码中，foo是公开方法，内部调用了bar.call(this, baz)。这使得bar()实际上成为了当前类的私有方法。



#### 3.10.3 私有属性模拟办法三

办法三：利用Symbol值的唯一性，将私有方法的名字命名为一个Symbol值。

```js
const bar = Symbol('bar')
const snaf = Symbol('snaf')

class MyClass {
  // 公有方法
  foo(baz) {
    this[bar](baz)
  }

  // 私有方法
  [bar](baz) {
    return (this[snaf] = baz)
  }
}
const foo1 = new MyClass()
foo1.foo('hello world')
console.log(foo1)
```

上面代码中，bar和snaf都是Symbol值，一般情况下无法获取到它们，因此达到了私有方法和私有属性的效果。

但是也不是绝对不行，`Reflect.ownKeys() `依然可以拿到它们。

```js
const inst = new MyClass()

console.log(Reflect.ownKeys(MyClass.prototype))
// => [ 'constructor', 'foo', Symbol(bar) ]
```



## 四、class的继承

### 4.1 class通过extends实现继承

一、class可以通过 `extends关键字` 实现继承。

```js
class Animals {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  show() {
    return `my name is ${this.name},I'm ${this.age} old`
  }
}
class Person extends Animals {
  constructor(name, age, height) {
    super(name, age)
    this.height = height
  }
  show() {
    return this.height + ' ' + super.show()
  }
}

const tom = new Person('tom', 18, 170)

console.log(tom.show()) //=>"170 my name is tom,I'm 18 old"
```



二、如果子类没有定义constructor方法，那么这个方法会被默认添加，代码示例如下：

```js
class ColorPoint extends Point {}
// 等同于
class ColorPoint extends Point {
  constructor(...args) {
    super(...args)
  }
}
```

也就是说，**不管有没有显式定义constructor方法，任何一个子类都有constructor方法。**



三、在子类的构造函数中，只有调用 `super方法` 之后，才可以使用this关键字，否则会报错。

```js
class Point {
  constructor(x, y) {
    this.x = x
    this.y = y
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color //=> ReferenceError
    super(x, y)
    this.color = color //=> 正确
  }
}
```

之所以要先调用 `super方法` 才能使用this关键字，是因为子类实例的构建，是基于父类的实例的，只有 `super方法` 才能调用父类实例。



四、父类的静态方法，会被子类继承。

**==注意：子类的实例是不会继承父类的静态方法的，会继承的是子类。==**

```js
class A {
  // 静态方法
  static show() {
    console.log('hello world')
  }
}
class B extends A {}
B.show() //=>"hello world"
```



### 4.2 检测子类继承父类的方法

`Object.getPrototypeOf()方法` 返回指定对象的原型对象。

根据这一特性，我们可以从子类上获取父类，同时也可以使用这个方法来判断，一个类是否继承了另一个类。

```js
//检测子类是否继承父类
class Foo {
  constructor(name) {
    this.name = name
  }
}

class Bar extends Foo {
  constructor(name) {
    super(name)
  }
}
//检测子类的父类
console.log(Object.getPrototypeOf(Bar) === Foo) //=>true
```



### 4.3 super关键字

`super关键字` 既可以当中函数使用，也可以当中对象使用。

**`super关键字` 注意的地方：**

>- **ES6规定，子类的构造函数必须执行一次super方法。**
>- **super作为函数调用时，必须是在子类的构造函数之中，如果用在其它地方就会报错。**

示例：子类继承父类，那么子类的构造函数必须执行一次super方法。

```js
class A {}
class B extends A {
  constructor() {
    super()
  }
}
const b=new B() //=>B {}

//如果子类的构造函数不执行super方法，那么子类实例对象的时候就会报错。
class A {}
class B extends A {
  constructor() {
    // super()
  }
}
const b = new B() //=>报错
```



#### 4.3.1 super关键字作为函数调用

一、`super关键字` 作为函数调用的时候，其调用的地方一定是在子类的构造函数中，其它地方的调用会报错。

```js
class A {}
class B extends A {
  show() {
    super() //=>报错
  }
}
```

二、在子类中，`super函数` 虽然代表的是父类的构造函数，但是其返回的是子类的实例。

它的操作相当于是：`父类.prototype.constructor.call(this)`。

```js
class Bar {
  constructor() {
    console.log(new.target.name) //new.target属性可以检测构造函数是谁调用的。
  }
}
class Foo extends Bar {
  constructor() {
    super()
  }
}
const bar = new Bar() //=>"Bar"
const foo = new Foo() //=>"Foo"
```



#### 4.3.2 super关键字作为对象调用

一、当 `super关键字` 作为对象调用时，也要看情况。

- 如果在普通方法中，`super` 指向的是父类的原型对象。
- 如果在静态方法中，`super` 指向的是父类。

```js
//示例一、super作为对象在普通方法中使用：
class A {
  show() {
    return 'AAA'
  }
}
class B extends A {
  constructor() {
    super()
    console.log(super.show()) //这里的super相当于是A.prototype。而show方法又是定义在父类的原型对象上。
  }
}
const b = new B() // => "AAA"
```

二、定义在父类实例对象上属性和方法，是无法通过 `super` 来调用的。原因是`super`指向的是父类的原型对象。

```js
//示例、无法通过super来调用父类的实例对象上的属性和方法：
class B extends A {
  get m() {
    return super.p
  }
  get showName() {
    return super.name
  }
}

let b = new B()
console.log(b.m) //=>undefined
```



三、**ES6规定，在子类普通方法中通过 `super` 调用父类的方法时，该父类方法内部的this，指向的是当前子类的实例对象。**

```js
//示例、子类通过super调用父类方法，其父类方法内部的this指向问题：

class Father {
  constructor() {
    this.name = 'goole'
  }
  print() {
    console.log(this.name)
  }
}
class Son extends Father {
  constructor() {
    super()
    this.name = 'Tom'
  }
  show() {
    super.print()
  }
}
const son1 = new Son()
son1.show() //=>"Tom"
```



四、当在子类构造函数中通过 `super` 对某个属性赋值，这时super就是this，赋值的属性会变成子类实例对象的属性。

```js
class Foo {
  constructor() {
    this.age = 100
  }
}
class Bar extends Foo {
  constructor() {
    super()
    this.age = 20
    super.age = 30
  }
  show1() {
    console.log(this.age)
  }
  show2() {
    console.log(super.age)
  }
}
const bar = new Bar()
bar.show1() //=>30
bar.show2() //=>undefined
```

从上面的结果可以看出，在子类构造函数中使用 `super` 对某个属性进行赋值，则是在子类的实例对象上进行赋值。也就是说此时的super就是this。



五、使用 `super` 的时候，必须显式指定是作为函数使用还是作为对象使用，不明示指定会报错。

```js
class A {}
class B extends A {
  constructor() {
    super()
    console.log(super) // 报错
  }
}
```



### 4.4 探究class的继承链和原型链

一、class作为构造函数的语法糖，其同时拥有 `__proto__属性 `和 `prototype属性` 。因此它同时存在两条继承链。

#### 4.4.1 探究class的两条继承链

二、子类的 `__proto__属性`指向和 `prototype属性` 指向：

```js
class A {} //父类
class B extends A {} //子类
const a = new A()
const b = new B()

console.log(B.__proto__ === A) //=>true
console.log(B.prototype === b.__proto__) //=>true
```

 从上面的结果可以得出，子类的 `__proto__属性`指向的是父类，子类的 `prototype属性` 指向的是子类的原型对象。



三、子类的原型对象的 `__proto__属性` 指向：

```js
class A {} //父类
class B extends A {} //子类
const a = new A()
const b = new B()

console.log(B.prototype.__proto__ === A.prototype) //=>true
```

 从上面的结果可以得出，子类的原型对象的 `__proto__属性` 指向的是父类的原型对象。



四、父类的 `__proto__属性` 指向和 `prototype属性` 指向:

```js
class A {} //父类
class B extends A {} //子类
const a = new A()
const b = new B()

console.log(A.__proto__ === Function.prototype) //=>true
console.log(A.prototype === a.__proto__) //=>true
```

 从上面的结果可以得出，父类的 `__proto__属性` 指向的是Function的原型对象，`prototype属性` 指向的是父类的原型对象。

#### 4.4.2 子类和父类之间的继承链实现

子类和父类之间的继承链模拟实现：

```js
class C {} //父类
class D {} //子类

D.prototype = Object.create(C.prototype) //父子的原型对象上的连接
// 上面等同于
//D.prototype.__proto__ = C.prototype

D = Object.create(C)
// 上面等同于
// B.__proto__ = C
// 检测
console.log(D.__proto__ == C) //=>true
```

#### 4.4.3 class原型链图

![image-20211116013944143](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015441.png)



## 五、Module的语法

### 5.1 概述

CommonJS和AMD模块都是动态加载，动态加载也称为运行时加载。

CommonJS模块加载实质是整体加载模块，然后再从该模块中取出部分方法。

示例：

```js
let { stat, exists, readfile } = require('fs')
// 等同于
let _fs = require('fs')
let stat = _fs.stat
let exists = _fs.exists
let readfile = _fs.readfile
```

ES6模块不是对象，它通过`export`命令指定模块的对外接口，通过`import`命令输入

```js
import { name, id, speak } from './src/first.js'
console.log(name, id)
console.log(speak)
```

ES6模块的输入实质是加载你所需要的方法，如果该模块还有多个方法，它也是不会加载的。这种加载方式称为**“编译时加载”**或**静态加载**。



### 5.2 严格模式

ES6的模块自动采用严格模块，不管你有没有在模块头部加上`"use strict"`。

示例：

ES6模块自动添加了严格模式，其中有这么一个限制，`this`关键字不能指向全局对象,即不应该在顶层代码使用this。

```js
import { name } from './src/first.js'
console.log(name) //Threey
console.log(this) //undefined
```



### 5.3 export命令

一、`export `命令规定了模块的对外接口，`import` 命令输入其它模块提供的功能。

二、**一个模块就是一个独立的文件。该文件内部的所有变量，外部无法获取。**

三、`export `命令有两种方式制定对外接口，一种是 `分别暴露对外接口`，一种是 `统一暴露接口`。

**分别暴露接口：**

```js
export let name = 'Threey'
export let id = 2181600
```

**统一暴露接口(推荐使用这种方式)：**

```js
let name = 'Threey'
let id = 218600
export { name, id }
```

四、**`export` 命令除了输出变量，还可以输出函数和类(class).**

示例：对外输出add函数

  ```js
  export function add(x, y) {
    return x + y
  }
  ```

五、`export` 命令输出的变量还可以使用as关键字来对变量重命名

```js
function fn1() {
  console.log('函数1')
}
function fn2() {
  console.log('函数2')
}
export { fn1 as f1, fn2 as f2 }
```

六、`export` 命令规定的对外接口必须与模块内部变量建立一一对应关系

```js
export 1; ---会报错
var m = 1
export m  ----也会报错
// 上面两种写法报错的原因是：没有提供对外的接口。上面两种写法都是直接输出1,但1只是一个值，不是一个接口。

正确写法:
export var n = 2  ---正确
var n = 3
export { n }   ---正确
var n = 4
export {n as m}  ---正确
```

七、同样，`function ` 和 `class` 的输出，也必须遵守上面第六点的写法：

```js
function f() {}
export f   ---报错

正确
export function f(){}

正确
function f() {}
export { f }
```

八、`export`命令可以出现在模块的任何位置，只要处于模块的顶层就可以了。但是如果出现在块级作用域内，就会报错，`import`命令也是如此。

```js
function foo() {
  export default 'bar' //出错。
}
foo()
```



### 5.4 import命令

一、使用 `export` 命令规定了该模块的对外接口后，其它js文件就可以通过 `import` 命令输入该模块。

```js
// 在first.js中已经制定了模块的对外接口
import { name, id } from './src/first.js'
console.log(name, id)

// 使用输入进来的变量
function setName() {
  return name + '' + id
}
console.log(setName()) //Threey2181600
```

二、如果想为输入的变量重新取一个名字，`import` 命令要使用 `as关键字`。

```js
import { name as newNmae } from './src/first.js'
```

三、**`import` 命令输入的变量只能读的，说明不允许在加载模块的脚本里改写接口。**

```js
import { name, id, show } from './src/second.js'
console.log(name, id, show)
console.log(show())
show = {}  //报错
```

四、`import `命令后面的 from 指定模块文件的位置，可以是相对路径，也可以是绝对路径。**如果不带路径，就只是一个模块名，那么就必须要有`package.json配置文件`，告诉javascript引擎模块的位置。**

```js
 import { myMethod } from 'util'  ---util模块名
```

五、`import` 命令具有提升效果，它会提升到整个模块的头部，然首先执行。

```js
console.log(show()) //{name: "Threey", id: 218600}
说明import命令具有提升效果
import { show } from './src/second.js'
```

六、由于`import` 命令是静态解析执行的，所以不能使用表达式和变量。

下面的例子都是出错的:

```js
import {'s'+'how'} from './src/second.js'  ---报错
let showmodule = 'show'
import {show} from showmodule    ---报错
if (x === 1) {
  import { name } from './src/first.js'
} else {
  import { name } from './src/first.js'
}      ---报错
```

七、`import` 语句会执行所加载的模块，所以下面的写法也是正确的。

```js
import './src/first.js'   //正确
```

八、目前阶段，通过Babel转码，CommonJS模块的require命令和ES6模块的import命令，可以写在同一个模块里面，但是最好不要这样做。**（不推荐两种不同模块的命令写在一起。）**



### 5.5 模块的整体加载

一、ES6加载模块还可以使用整体加载,即用 `*` 指定一个对象，然后该接口的输出值都会加载在这个对象上面

```js
// 示例:模块整体加载
import * as circle from './src/circle.js'
console.log(circle)
console.log('圆的面积：' + circle.area(4))
//结果：圆的面积：50.26548245743669
console.log('圆的周长：' + circle.circumference(100))
结果：圆的周长：628.3185307179587
```

二、模块的整体加载所在的那个对象，是静态解析的，不可以在运行时发生改变。

下面的写法是不允许的：

```js
// ./src/circle.js文件资源信息
export function area (radius) {
  return Math.PI * radius * radius
}

export function circumference (radius) {
  return 2 * Math.PI * radius
}

// 导入文件
import * as circle from './src/circle.js'
// circle.foo = 'hello'  ---不能添加新的属性
// circle.area = function () {}  ---不能修改原来的属性
```



### 5.6 export default命令

一、使用 `export default` 命令可以为模块默认输出，这个用法在Vue和react中会经常出现。

示例：

```js
// 默认暴露
export default {
  name: "threey",
  id: 2181600,
  age: 20,
  hobby: "喜欢敲代码和前端",
  show () {
    return this;
  },
};
```

二、其它模块在加载这个模块的时候，`import ` 命令可以为该匿名函数指定任意名字。（**仅限于export default默认导出**）

导出匿名函数：

```js
// defaultMoudle.js
export default function () {
  console.log('foo');
}
```

```js
import defaultModule from './src/default.js'
console.log(defaultModule)
```

三、`export default` 命令用在非匿名函数前，也是可以的。

```js
// export-default.js
export default function foo() {
  console.log('foo');
}

// 或者写成
function foo() {
  console.log('foo');
}

export default foo;
```

注意：==上面代码中，foo函数的函数名foo，在模块外部是无效的。加载的时候，视同匿名函数加载。==



三、**比较默认输出和正常输出：**

```js
// 导出的资源文件crc.js
export default function crc32 () {
  return '默认导出模块'
}

export function crc323 () {
  return '正常导出模块'
}
```

```js
import crc32 from './src/crc.js'
console.log(crc32())
//结果：默认导出模块

import { crc323 } from './src/crc.js'
console.log(crc323())
```

>**两者比较发现，默认导出模块时不需要大括号，而正常导出模块则需要大括号。**

四、`export default` 命令默认导出的模块只能有一个，而 `export` 命令可以有多个。

五、`export default  ` 命令其实是输出一个叫做 `default` 的变量，所以==它后面不能跟变量声明语句==。

```js
export var a = 1    //正确

var a = 2
export default a    //正确

export default var a = 4    //错误
```

六、因为`export defalu`t命令本质是将后面的值赋值给`default`变量，所以可以直接将一个值写在`export default`之后。

```js
export default 42 ---正确
export 42 ---报错
```

七、默认导入和正常导入可以写在一起

```js
import crc32, { crc323 } from './src/crc.js'
console.log(crc32)
console.log(crc323)
```

八、`export default` 也可以用来输出类。

```js
// 以MyClass.js示例
export default class {}

// main.js
import MyClass from 'MyClass.js'
let o = new MyClass()
```



### 5.7 export与import的复合写法

一、如果一个模块之中，先输入后输出同一个模块，`import语句` 可以与 `export语句` 写在一起的。

```js
export { foo, bar } from 'my_module';

// 可以简单理解为
import { foo, bar } from 'my_module';
export { foo, bar };
```

上面代码中，`export`和`import`语句可以结合在一起，写成一行。

**这种写在一起的方式只是相当于在该模块中转了一下，导致的问题是当前模块并不能直接使用foo和bar。**



### 5.8 跨模块变量

一、`const`声明的常量只能在当前代码块中有效。

如果想设置跨模块的常量（即跨多个文件），或者说一个值要被多个模块共享，可以采用下面的写法：

```js
// xxxx.js模块
export const A = 1
export const B = 2
export const C = 3

// test1.js模块
import * as test from 'xxx.js'
console.log(test.A) //1
console.log(test.B) //2
console.log(test.C) //3
```

二、如果要使用的常量非常多，可以建一个专门的文件夹目录，将各种常量写在不同的文件里，然后将 这些文件保存到该目录下。

示例：

```js
// constants/db.js
export const db = {
  url: 'xxxxxxxx',
  admin_username: 'admin',
  admin_password: 'admin password',
}

// constants/user.js
export const users = [
  'root',
  'admin',
  'staff',
  'ceo',
  'chief',
  'moderator',
]
```

然后，将这些文件输出的常量，合并在`index.js`里面。

**`index.js`文件可以理解为整合了很多资源的中间商，你想要资源就尽管问它拿。**

```js
// constants/index.js
export { db } from './db'
export { users } from './users'
```



### 5.9 import()函数

ES2020 引入`import()`函数，支持动态加载模块。(==之前import和export命令都是静态加载，不能用在表达式和变量中==)。

```js
import(specifier)
```

上面代码中，`import`函数的参数`specifier`，指定所要加载的模块的位置。

`import`命令能够接受什么参数，`import()`函数就能接受什么参数，两者区别主要是后者为动态加载。



`import()`返回一个 Promise 对象。

```js
const main = document.querySelector('main');

import(`./section-modules/${someVariable}.js`)
  .then(module => {
  module.loadPageInto(main);
})
  .catch(err => {
  main.textContent = err.message;
});
```

**对import()函数的说明：**

>1. `import()`函数可以用在任何地方，不仅仅是模块，非模块的脚本也可以使用。它是运行时执行，也就是说，什么时候运行到这一句，就会加载指定的模块。
>2. `import()`函数与所加载的模块没有静态连接关系，这点也是与`import`语句不相同。
>3. `import()`函数类似于ComonJs模块的`require()`方法，区别在于`Import()`是异步加载，`require()`方法是同步加载。



## 六、Module的加载实现

### 6.1 浏览器加载

#### 6.1.1 前言

- 默认情况下，浏览器是同步加载Javascript脚本，即渲染引擎遇到`<script>标签`就会停下来，等脚本执行完再继续向下渲染。

- 如果外部脚本文件体积很大，那么浏览器执行加载时间就会变得很长，这样会造成浏览器堵塞，即浏览器"卡死",没有任何响应。

为了解决这一问题，**浏览器允许脚本异步加载**。

#### 6.1.2 浏览器异步加载脚本文件

浏览器异步加载脚本文件语法有两种方式：

1. 方式一：`<script>标签`添加`async`属性

   ```html
   <script type="module" async>
     import Module from './加载模块资源/Module1.js'
     console.log(Module)
   </script>
   ```

2. 方式二：`<script>`标签添加`defer`属性

   ```html
   <script type="module" defer>
     import Modules from './加载模块资源/Module2.js'
     console.log(Modules)
   </script>
   ```

   ==注意：当添加type="module"属性时，就默认是添加defer属性的方式==

**两者的区别：**

>**`defer`是渲染完再执行，而`async`是下载完就执行。**

另外，如果有多个defer脚本，它们在页面会按顺序加载，而async脚本则不能保证加载顺序。

**示例：**

当存在async属性的script脚本：

![image-20210801201228601](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015514.png)

![image-20210803042629151](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015518.png)

可以发现当存在`async`属性的时候，模块的加载顺序不能得到保证！！

#### 6.1.3 浏览器加载ES6模块规则

一、浏览器要加载ES6模块，则需要在`script标签`中添加 `type="module"` 属性。

```html
<script type="module" src="./加载模块资源/Module1.js"></script> 
```

二、浏览器对于带有type="module"属性的script标签，都是异步加载，不会造成堵塞浏览器，即等页面渲染完再执行脚本。等同于在`<script>标签`中添加了`defer`属性

```html
<script type="module" src="./加载模块资源/Module1.js"></script>
等同于
<script type="module" src="./加载模块资源/Module1.js" defer></script>
```

三、如果网页中出现多个`<script type="module">`，则会按顺序执行脚本。

四、`<script>标签`的`async属性`也可以打开，这时只要脚本加载完成，渲染引擎就会中断渲染，进而执行该脚本文件，等脚本执行完后再恢复渲染。

```html
<script type="module" src="./加载模块资源/Module1.js" async>
```

五、ES6模块也允许内嵌网页中，语法行为与加载外部脚本资源完全一样。

```html
//这里采用的是具名模块加载。
<script type="module">
  import { p } from './加载模块资源/Module3.js'
  console.log(p)
</script> 
```

**针对ES6模块内嵌网页中说明：**

>- 代码是在模块作用域中进行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
>
>- 模块之中，可以使用`import命令`加载其它模块的资源(.js后置名不能少，同时需要提供相对路径和绝对路径。)，也可以使用`export命令`输出对外接口。
>
>- 模块脚本内部自动采用严格模式，不管有没有声明`use strict`。
>
>- 同一个模块如果加载多次，那么只会执行一次。
>
>- 模块中，顶层的`this关键字`指向的`undefined`，而不是`window`。
>
>  对于这一点，可以验证一下。
>
>  ```html
>  <script type="module" defer>
>    import { p } from './加载模块资源/Module3.js'
>    console.log(p)
>    console.log(this === undefined) //结果：true
>  </script>
>  ```
>
>  ==**利用顶层的this等于undefined这一特点，可以检测当前代码是否在ES6模块中。**==



### 6.2 ES6模块与CommonJS模块的差异

`ES6模块`与`CommonJS模块`有三个重大的差异：

1. CommonJS模块输出的是一个值的拷贝，而ES6模块输出的是值的引用。
2. CommonJS是动态解析，也就是运行时加载，而ES6模块是静态解析。
3. **CommonJS模块的 `require()` 是同步加载模块**，而**ES6模块的 `import命令` 是异步加载模块**，有一个独立的模块依赖解析阶段。



对于第二个差异的原因是：CommonJS加载的是一个对象（即module.exports属性），该对象只有在脚本运行完后才会生成。而ES6模块加载的不是一个对象，它的对外接口只是一个静态规定，在代码解析阶段才会生成。

#### 6.2.1 重点解释第一个差异

一、**CommonJS模块输出的是值的拷贝，模块内部的变化影响不到已输出的值。**

代码示例：

```js
//在lib.js文件中
var counter = 3
function increment () {
  counter++
}
module.exports = {
  counter,
  increment
}

//在main.js文件中
var number = require('./lib')
console.log(number)
console.log(number.counter)  //结果：3
number.increment() 
console.log(number.counter)  //结果：3
```

从结果中可以发现，它的内部变化影响不到`已输出的number.counter`。**这是因为`number.counter`是一个原始的值，会被缓存。**

> 解放方法：==写成一个函数，这样能得到模块内部变化后的值。==

代码改进：

```js
//在lib.js文件中
var counter = 3
function increment () {
  counter++
}
module.exports = {
  get counter () {
    return counter
  },
  increment
}

//在main.js文件中
var number = require('./lib')
console.log(number)
console.log(number.counter)  //结果：3
number.increment() 
console.log(number.counter)  //结果：4
```

此时可以发现，模块内部输出的值发生了改变。



二、由于ES6模块的输出是`值的引用`，也就是说它不会有缓存值，模块内部的变量绑定在其所在的模块中。

**`export命令`规定模块的对外接口，输出的是同一个值，不同的脚本模块加载这个接口，得到的都是相同的实例。**

代码示例：

```js
//在lib2.mjs文件中
export let num = 4
export function increment () {
  num++
}

//在main.js文件中
import { num, increment } from './lib2.mjs'
console.log(num, increment);
```



### 6.3 Node.j环境的模块加载方法

- JavaScript现在有两种模块。一种是ES6模块简称`ESM`；另一种是CommonJS模块，简称`CJS`。

- CommonJS模块使使用`require()`和`module.exports` ,ES6模块使用的是`export`和`import`。
- CommonJS模块是Node.js专用的，与ES6模块不兼容。
- Node.js从v13.2版本开始已经默认开启了ES6模块支持。

#### 6.3.1 Node.js环境加载ES6模块

- Node.js要求ES6模块采用`.mjs`后缀文件名，也就是说，**只要脚本文件中使用到了`import`和`export命令`**，那么就**必须使用.mjs后缀名**。这样Node.js遇到`.mjs`文件就会认为它是ES6模块，并且默认启用严格模式。

  代码示例：

  ```js
  //file.mjs
  export let num = 4
  export function increment () {
    num++
  }
  
  //index.mjs
  import { num, increment } from './lib2.mjs'
  console.log(num, increment);
  
  //在node环境中执行node index.mjs
  ```

- 如果不希望修改将文件后缀名改为.mjs，那么可以在该项目的`package.json文件`中，指定`type字段为module`.

  ```json
  {
    "typt":"module"
  }
  ```

  **一旦设置以后，该目录里面的JS脚本，就会被解释用ES6模块。**

#### 6.3.2 Node.js环境加载CommonJS模块

如果在Node.js环境中想使用CommonJS模块，有三种方式：

1. 将模块脚本文件的后缀名修改为`.cjs`。
2. 在package.json文件中添加`type字段为common.js`，则` .js `脚本会被解析成CommonJS模块。
3. 文件名为 `.js`，什么都不用处理，因为node.js环境默认采用的是CommonJS模块的方式加载。

```js
{
  "type":"commonjs"
}
```



#### 6.3.3 总结

`.mjs文件`总是以ES6模块加载，`.cjs文件`总是以CommonJS模块加载，`.js文件`的加载取决于package.json里面的type字段的设置。



### 6.4 Node.js环境通过package.json文件配置模块加载方式

之前也有说到，如果不想通过修改脚本文件后缀名来实现加载不同的模块，这时可以通过`package.json配置文件`来设置。

```json
//添加type字段
{
  "type":"commonjs/module"
}
```

除这个功能外，`package.json`配置文件还有很多字段需要我们去了解。



#### 6.4.1 package.json的main字段

`package.json`文件有两个字段可以指定模块的入口文件：`main`和`exports`。

比较简单的模块，可以只使用`main`字段，指定模块加载的入口文件。

```json
//package.json配置文件目录：./node_modules/es-module-package/package.json
{ 
  "type": "module",
  "main": "./src/index.js",
}
```

上面代码指定了项目的入口脚本为`./src/index.js`，它的格式为ES6模块。如果没有`type`字段，那么`index.js`文件就会视为CommonJs模块。

然后在`my-app.mjs`文件中通过`import`命令就能加载这个模块。

```js
import { something } from 'es-module-package'
console.log(something)  //{ name: '辣椒炒肉很好吃', number: 1000, show: [Function: show] }
console.log(something.name);    //辣椒炒肉很好吃
console.log(something.number);  //1000
console.log(something.show());  //椒炒肉很好吃，我要吃1000!
```

**解释说明：**

>- 这里的`my-app.mjs`文件要以`.mjs`为后缀名，因为该文件的同一目录没有`package.json`配置文件，以至于Node.js环境不能把`.js`后缀名的脚本文件视为ES6模块。
>
>- 运行该脚本以后，Node.js 就会到`./node_modules`目录下面，寻找`es-module-package`模块，然后根据该模块`package.json`的`main`字段去执行入口文件。
>
>  ![image-20210802221604746](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015527.png)

如果此时用 CommonJS 模块的`require()`命令去加载`es-module-package`模块会报错，因为 CommonJS 模块不能处理`export`命令。



#### 6.4.2  package.json的exports字段

`exports`字段的优先级高于`main`字段。它有多种用法。

##### 一、子目录别名：

`package.json`文件的`exports`字段可以指定**脚本或子目录的别名**。

示例一：指定脚本文件别名

```json
{ 
  "type": "module",
  "exports": {
    "./index":"./src/submodule.js",
  }
}
```

上面代码指定`./src/submodule.js`脚本文件的别名是`index`，然后可以从这个别名可以加载这个模块脚本文件。

```js
//submodule.js文件信息：
export default {
  name: '油炸大虾',
  number: 100
}
```

```js
//my-app.mjs文件
import submodule from 'es-module-package/index';
console.log(submodule);
```

结果：输入的值符合`submodule.js`模块内部的值。

![image-20210802222622797](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015532.png)

**代码解释说明：**

>- `es-module-package模块`有`package.json`配置文件，里面配置了需要导入的模块脚本文件别名，**模块拼上别名就能找到加载的模块脚本文件。**
>
>  图示：最终加载路径：./node_modules/es-module-package/src/submodule.js
>
>![image-20210802223204352](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015536.png)



示例二：指定子目录别名

```js
{ 
  "type": "module",
    "exports": {
      "./features/":"./src/features/"
    }
}
```

上面的代码指定了`features`子目录的别名，然后从这个**别名+脚本文件名**就能加载这个模板脚本文件。

```js
//x.js文件资源信息：
export default {
  title: '我爱吃棒棒糖',
  name: '我是超人'
}
```

```js
//my-app.mjs
import a from 'es-module-package/features/x.js'
console.log(a);
```

结果：输入的值符合`x.js`模块内部的值。

![image-20210802224903671](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015541.png)

**代码说明解释：**

![image-20210802224657718](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015543.png)





##### 二、main的别名：

`exports`字段的别名如果是`.`，就代表模块的主入口，优先级高于`main`字段，并且可以直接简写成`exports`字段的值。

示例：

```json
{ 
  "type": "module",
  "exports": {
    ".":"./src/submodule.js",
  }
}

//上面等同于下面这种：
{ 
  "type": "module",
  "exports": "./src/submodule.js"
}
```

```js
//my-app1.mjs模块导入文件
import submodule from 'es-module-package';
console.log(submodule);
```



由于`exports`字段只有支持 ES6 的 Node.js 才认识，**所以可以用来兼容旧版本的 Node.js**。

```javascript
{
  "main": "./main-legacy.cjs",
  "exports": {
    ".": "./main-modern.cjs"
  }
}
```

上面代码中，老版本的 Node.js （不支持 ES6 模块）的入口文件是`main-legacy.cjs`，新版本的 Node.js 的入口文件是`main-modern.cjs`。



##### 三、条件加载：<a name="条件加载"></a>

利用`.`这个别名，可以为ES6模块和CommonJS模块指定不同的入口。

示例：

```json
// ./Module_resource/package.json
{
  "type": "module",
  "exports":{
    ".":{
      "require":"./src/main.cjs",
      "default":"./src/main.js"
    }
  }
}
```

- 在`require.cjs`文件中加载模块脚本文件：

```js
const p = require('Module_resource')
console.log(p);
```

**结果：Module_resource模块中的`main.cjs`脚本资源文件导入进来了**

![image-20210803021213908](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015547.png)

- 在`default。mjs`文件中加载模块脚本文件:

```js
import resource from "Module_resource"
console.log(resource)
```

结果：**Module_resource模块中的`main.js`脚本资源文件导入进来了**

![image-20210803021033278](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015549.png)

**说明：**

>- 上面代码中，别名`.`的`require`条件指定`require()`命令的入口文件（即 CommonJS 的入口），`default`条件指定其他情况的入口（即 ES6 的入口）。
>
>- 上面的写法可以简写如下：
>
>  ```json
>  // ./Module_resource/package.json
>  {
>    "type": "module",
>    "exports":{
>      "require":"./src/main.cjs",
>      "default":"./src/main.js"
>    }
>  }
>  ```
>
>  **==不过这种简写是不能存在其它的别名，否则会报错。==**



### 6.5 CommonJS模块加载ES6模块

CommonJS 的`require()`命令不能加载 ES6 模块，会报错，只能使用`import()`这个方法加载。

```js
(async () => {
  const { default: i } = await import('./my-app.mjs')
  console.log(i.title);  //结果：我是ES6导出模块
})();
```

```js
//ma-app.mjs脚本文件
export default {
  title: '我是ES6导出模块'
}
```

上面代码可以在 CommonJS 模块中运行。

`require()`不支持ES6模块的原因是，它是==同步加载==，而ES6模块导入是==异步加载==。

**为了可以在CommonJS模块中加载ES6模块，可以通过`async/await`方式来处理`import()异步函数`，即在ES6模块内部使用顶层`await命令`，这样就能很好的处理ES6模块异步加载问题。**



### 6.6 ES6模块加载CommonJS模块

ES6模块的`import`命令可以加载CommonJS模块，==但是只能整体加载，不能单一加载==。

```js
// 正确
import p from "./commonjs-package.js"

// 报错
import { p } from "./commonjs-package.js"


//  ./commonjs-package.js
module.exports = {
  name: '我是CommonJS模块'
}
```

这是因为 ES6 模块需要支持静态代码分析，而 CommonJS 模块的输出接口是`module.exports`，是一个对象，无法被静态分析，所以只能整体加载。

加载单一的输出项，可以这样做：

```js
import packageMain from 'commonjs-package';
const {method}= packageMain;
```



### 6.7 两模块导入导出总结

#### #6.7.1 ES6模块

1. 在ES6模块中如果想导入commonJS模块，可以使用`import`命令导入，此时导入的文件要么修改为后缀名为`.mjs`，要么把该文件的放到一个子目录并且在该子目录添加一个`package.json`配置文件，在该配置文件中添加`type:module`属性。

   >ES6模块导入commonJS模块有一种整合方法：
   >
   >1. 先一个文件中把所有CommonJS模块导入进来，然后再`module.exports`导出。
   >2. 然后再另一个文件中通过`import`命令导入这些整合的CommonJS模块，再通过ES6输出具名接口。
   >
   >示例：
   >
   >![image-20210803040607042](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015555.png)
   >
   >![image-20210803041104905](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015558.png)
   >
   >![image-20210803041323133](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116021412.png)

2. 在ES6模块导入ES6模块，直接使用`import/export`命令。

#### #6.7.2 CommonJS模块

1. 在CommonJS模块中如果想导入ES6模块，这时需要`import()方法`，并且还需要`async/await`来处理`import()异步加载问题`。
2. 在CommonJS模块中导入CommonJS模块，直接使用`require/module.exports`.

或者可以在`package.json`文件中添加`exports`字段，并且利用`.`别名，这样就可以为不同模块之间指定入口。

具体参考：[package.json的exports字段---条件加载](#条件加载)



#### # 6.7.3 Node环境模块导入导出大致流程图

![image-20210805014138755](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211116015604.png)



### 6.8 Node.js的内置模块

**Node.js 的内置模块可以整体加载，也可以加载指定的输出项。**

```javascript
// 整体加载
import EventEmitter from 'events';
const e = new EventEmitter();

// 加载指定的输出项
import { readFile } from 'fs';
readFile('./foo.txt', (err, source) => {
  if (err) {
    console.error(err);
  } else {
    console.log(source);
  }
});
```



### 6.9 内部变量

为了使ES6模块能在浏览器环境和服务器环境通用，Node.js规定了ES6模块内部不能使用CommonJS模块特有的一些内部变量。

首先就是`this`关键字了，在ES6模块中指向的是`undefined`，而在CommonJS模块中`this`指向的是当前模块。

还有一些顶层变量，ES6模块是不存在的:

>- `arguments`
>- `require`
>- `module`
>- `exports`
>- `__filename`
>- `__dirname`

