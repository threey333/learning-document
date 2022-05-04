# axios知识点填坑总结

## 一、axios四种常用请求方法

### 1.1 get请求

`get请求` 用于获取服务器端的数据。

#### 1.1.1 axios API通用get请求写法

`axios `通用 `get请求` 写法：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送get请求</button>
```

```js
const btn = document.querySelectorAll('button')
// axios默认配置
axios.defaults.baseURL = 'http://localhost:3000/'
btn[0].addEventListener('click', async function () {
  //axios API通用写法：
  const { data: res } = await axios({
    method: 'get',
    url: 'gets',
    params: {},
  })
  console.log(res)
})
```



#### 1.1.2 axios独立get请求写法

`axios` 独立 `get请求` 写法：**`axios.get(url,config)`**

- 参数一：请求地址。
- 参数二：配置信息。
  - `params`：表示传递到服务器端的数据请求参数，以url参数的形式拼接在 `参数1` 请求地址后面。(相当于$route中的query查询参数。**为了方便管理使用，一般都是采用在config中添加url参数(params)。**
  - `header`：表示请求头的配置信息。

 ```html
 //使用了bootstrap样式库
 <button type="button" class="btn btn-primary">axios发送get请求</button>
 ```

```js
axios.defaults.baseURL = 'http://localhost:3000/'
const btn = document.querySelectorAll('button')

btn[0].addEventListener('click', async function () {
  const { data: res } = await axios.get('gets', {
    // 为了方便管理，url参数一般采用在config中的添加params属性。
    // ---请求所有数据，使用一个空对象表示
    // ---请求某一个数据，在params参数对象中添加相应要请求的key/value值。
    params: {},
  })

  console.log(res)
})
```



### 1.2 post请求

`post请求` 用于将数据提交到服务器端。（包括表单的提交和文件的上传）

#### 1.2.1 axios API通用post请求写法

`axios` 通用 `post请求`写法：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送post请求</button>
```

```js
const btn = document.querySelectorAll('button')
axios.defaults.baseURL = 'http://localhost:3000/'

btn[1].addEventListener('click', async function () {
  const { data: res } = await axios({
    method: 'post',
    url: 'posts',
    // data: {
    //   food: '我爱吃鲤鱼,呜呜呜',
    //   age: 20,
    // },
    
    header: {
      'Content-Type': 'application/x-www-form-urlencoded',
    },
    data: 'age=21&food=油炸皮卡丘',
  })
  console.log(res)
})
```



#### 1.2.2 axios独立post请求写法

`axios` 独立 `post请求` 写法：**`axios.post(url,data,config)`**

- 参数一：请求地址。
- 参数二：请求体数据（==**post请求数据是在请求体data中。**==）
- 参数三：配置信息。

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送post请求</button>
```

```js
axios.defaults.baseURL = 'http://localhost:3000/'
const btn = document.querySelectorAll('button')

btn[1].addEventListener('click', async function () {
  const { data: res } = await axios.post('posts', {
    title: 'post请求4',
    author: 'R',
  })
  console.log(res)
})
```



#### 1.2.3 post请求的注意事项

**注意事项：**

- post请求中配置信息为`conttent-type` 默认是为：`'application/json'`。即发送数据给后端的格式是`JSON格式`，以至于后端接收数据会失败。
- 前端的解决办法：在`header请求头`中对`content-type`属性进行设置，设置为`'application/x-www-form-urlencoded'`。
- 同时`data请求体`要设置为像get请求的url参数拼接的形式，比如data:'XXX=xxx&XXX=xxx'。

示例：

```js
header: {
  'Content-Type': 'application/x-www-form-urlencoded',
},
data: 'age=21&food=油炸皮卡丘',
```

**说明：**

>在Vue或React脚手架中，data可以使用对象格式，因为它们有插件帮我们解析。



### 1.3 delete请求

`delete请求` 用于删除服务器端的数据。

#### 1.3.1 axios API通用delete请求写法

`axios` 通用 `delete请求`写法：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送delete请求</button>

​```js
axios.defaults.baseURL = 'http://localhost:3000/'
const btn = document.querySelectorAll('button')

btn[2].addEventListener('click', async function () {
  const { data: res } = await axios({
    method: 'delete',
    url: 'posts/5',
  })
  console.log(res)
})

```



#### 1.3.2 axios独立delete请求写法

`axios` 独立 `delete请求` 写法：**`axios.delete(url,config)`**

**注意：**

 >因为delete请求不用携带任何数据，所以要删除什么就直接把删除的参数拼接在请求地址上。

代码示例：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送delete请求</button>
```

```js
btn[2].addEventListener('click', async function () {
  const { data: res } = await axios.delete(`posts/${i}`)
  console.log(res)
})
```



### 1.4 put请求

`put请求`用于更新数据，此更新是全部替换。

- 例如更新id为4数据：如果使用put请求更新，则会把该id为4的所有数据全部替换为data请求体数据。

**更新说明示例图：**

![image-20210912230259216](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210912230727.png)

![image-20210912230533659](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210912230730.png)

![image-20210912230647620](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210912230737.png)



#### 1.4.1 axios API通用put请求写法

`axios` 通用 `put请求`写法：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送put请求</button>
```

```js
const btn = document.querySelectorAll('button')
// axios默认配置
axios.defaults.baseURL = 'http://localhost:3000/'

btn[3].addEventListener('click', async function () {
  const { data: res } = await axios({
    method: 'put',
    url: 'posts/5',
    data: {
      number: 200,
      show: '哈哈哈哈',
    },
  })
  console.log(res)
})
```



#### 1.4.2 axios独立put请求写法

`axios` 独立 `put请求` 写法：**`axios.put(url,data,config)`**

- 参数一：请求地址。
- 参数二：请求体数据。
- 参数三：配置信息。

代码示例：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送put请求</button>
```

```js
const btn = document.querySelectorAll('button')
// axios默认配置
axios.defaults.baseURL = 'http://localhost:3000/'

btn[3].addEventListener('click', async function () {
  const { data: res } = await axios.put('gets/4', {
    title: 'put更新的数据1',
    author: 'yyy',
  })
  console.log(res)
})
```



### 1.5 patch请求

 `patch请求` 用于更新数据，此更新是局部更新。

- 例如更新id为4的数据，那么只需在data请求体数据中填入更新的数据即可。

#### 1.5.1 axios API通用patch请求写法

`axios` 通用 `patch请求`写法：

```html
<button type="button" class="btn btn-primary">axios发送patch请求</button>
```

```js
const btn = document.querySelectorAll('button')
// axios默认配置
axios.defaults.baseURL = 'http://localhost:3000/'

btn[4].addEventListener('click', async function () {
  const { data: res } = await axios({
    method: 'patch',
    url: 'posts/3',
    data: {
      title: 'patch局部更新数据',
    },
  })
  console.log(res)
})
```



#### 1.5.2 axios独立patch请求写法

`axios` 独立 `delete请求` 写法：**`axios.patch(url,data,config)`**

- 参数一：请求地址。
- 参数二：请求体数据。
- 参数三：配置信息。

代码示例：

```html
//使用了bootstrap样式库
<button type="button" class="btn btn-primary">axios发送delete请求</button>
```

```js
const btn = document.querySelectorAll('button')
// axios默认配置
axios.defaults.baseURL = 'http://localhost:3000/'
btn[4].addEventListener('click', async function () {
  const { data: res } = await axios.patch('gets/2', {
    title: 'patch更新的数据',
  })
  console.log(res)
})
```



## 二、请求配置

### 2.1 请求配置 config

**请求配置信息：**

```javascript
{}
   // `url` 是用于请求的服务器 URL
  url: '/user',
    
  // `method` 是创建请求时使用的方法
  method: 'get', // default
    
  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',
    
  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],
    
  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],
    
  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},
    
  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },
    
   // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },
    
  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },
    
  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,
    
   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },
    
 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },
    
   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default
    
  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default
    
   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default
    
  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default
    
   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },
    
  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },
    
   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,
    
  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise。
  // 如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },
    
  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default
    
  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default
    
  // `httpAgent` 和 `httpsAgent` 分别在node.js中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),
    
  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },
    
  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```



## 三、axios请求响应回来的信息

### 3.1 响应信息

**响应信息：**

- `data`：由服务器提供的响应数据。
- `config`：请求的配置信息。

- `request`：可忽略。
- `status`：由服务器响应回来的http状态码。
- `statusText`：由服务器响应回来的http状态信息。

![image-20210913230459414](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210913232028.png)



## 四、axios实例

### 4.1 创建axios实例

可以通过 `axios.create()` 创建一个新的axios实例，每个axios实例都有自己的配置。

```js
const instance = axios.create({
  baseURL: 'http://localhost:3000/gets',
  timeout: 3000,
})
```



### 4.2 新axios实例拥有的方法

新axios实例拥有的方法和原始的axios方法一样。故原始的axios怎么使用方法，新的axios实例就怎么使用方法。

```html
axios#request(config)
axios#get(url[, config])
axios#delete(url[, config])
axios#head(url[, config])
axios#options(url[, config])
axios#post(url[, data[, config]])
axios#put(url[, data[, config]])
axios#patch(url[, data[, config]])
axios#getUri([config])
```

**使用示例：**

```js
const btn = document.querySelectorAll('button')
// 创建axios的新实例
const instance1 = axios.create({
  baseURL: 'http://localhost:3000/gets',
  timeout: 3000,
})
const instance2 = axios.create({
  baseURL: 'http://localhost:3000/posts',
  timeout: 3000,
})

btn[0].addEventListener('click', async function () {
  const res = await instance1.request({
    method: 'GET',
    params: {},
  })
  console.log(res)
})

btn[1].addEventListener('click', async function () {
  const { data: res } = await instance2({
    method: 'GET',
    params: {},
  })
  console.log(res)
})
```

从使用示例中可以发现，它跟原始axios没有区别，只是别名要换成新实例的名字。



### 4.3 为什么要设计create()创建新axios实例

当有这么一个需求：项目中有部分接口需要的配置与另一部分接口需要的配置不一样。

如果处理这个需要？

解决办法就是：创建多个新的axios实例，每个axios实例都有自己特有的配置信息，它能分别应用到不同需求的接口请求中。



## 五、axios拦截器

### 5.1 拦截器

拦截器就是在请求或响应被 `then` 或 `catch` 处理前拦截它们。

axios拦截器有两种：

- `axios.interceptors.request` 请求拦截器。
- `axios.interceptors.response` 响应拦截器。

**使用说明：**

```js
// 请求拦截器
axios.interceptors.request.use(
  // 请求拦截器的use方法里面有两个回调函数，一个是成功拦截的回调，一个是失败拦截的回调。
  config => {
    // 请求拦截器在项目中用的最多的就是中途添加token
    config.headers.Authorization = { token: 'token令牌' }
    console.log('成功拦截的回调函数')
    console.log(config)
    return config
  },
  //请求拦截器的config回调函数中的数据信息，就是我们axios发送请求添加的配置信息数据。
  error => {
    return Promise.reject(error)
  }
)

// 响应拦截器
axios.interceptors.response.use(
  response => {
    console.log('响应拦截器的回调函数')
    console.log(response)
    return response
  },
  error => {
    return Promise.reject(error)
  }
)
//响应拦截器的response回调函数中的数据信息，是服务器端成功接收请求并返回响应数据。数据里面有config配置信息、data响应数据、响应头信息、请求体信息，status状态码、statusText状态内容。

const btn = document.querySelector('button')
btn.addEventListener('click', async function () {
  const { data: res } = await axios({
    method: 'GET',
    url: 'http://localhost:3000/gets',
    params: {},
    // timeout指定请求超时前的毫秒数，如果响应时间超过这个毫秒数就会报错。
    timeout: 3000,
  })
  console.log(res)
})
```



### 5.2 移除拦截器

如果需要移除拦截器，可以这样：

```js
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```



### 5.3 自定义的axios实例添加拦截器

可以给自定义的 axios 实例添加拦截器。

```js
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```



## 六、配置默认值

我们可以指定每个请求配置默认值。

### 6.1 全局 axios 默认值

可以使用 `axios.defaults` 来设置。

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```



### 6.2 自定义axios实例默认值

也可以为创建的`自定义的axios实例` 设置默认值 `instance.defaults`。

```js
// 创建实例时配置默认值
const instance = axios.create({
  baseURL: 'https://api.example.com'
});

// 创建实例后修改默认值
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```



### 6.3 配置的优先级

配置将会按优先级进行合并。它的顺序是：在 /lib/defaults.js 中找到的库默认值，然后是实例的 `defaults` 属性，最后是请求的 `config` 参数。后面的优先级要高于前面的。下面有一个例子。

```js
// 使用库提供的默认配置创建实例
// 此时超时配置的默认值是 `0`
const instance = axios.create();

// 重写库的超时默认值
// 现在，所有使用此实例的请求都将等待2.5秒，然后才会超时
instance.defaults.timeout = 2500;

// 重写此请求的超时时间，因为该请求需要很长时间
instance.get('/longRequest', {
  timeout: 5000
});
```

**总结：**

>**`实例方法中通过修改config配置` > `自己指定的配置默认值` > `库的配置默认` 。**



## 七、取消请求

可以使用取消令牌来取消请求。

### 7.1 取消请求方法一

可以使用 `CancelToken.source` 工厂方法创建一个取消令牌 ，如下所示：

```js
const { CancelToken } = axios
const source = CancelToken.source()

const btn = document.querySelector('button')
btn.addEventListener('click', async function () {
  try {
    const res = await axios({
      method: 'GET',
      params: {},
      url: 'http://localhost:3000/gets',
      cancelToken: source.token,
    })
    source.cancel(
      '请求发送成功,从服务器响应回来的数据成功获取,此请求可以取消了'
    )
    console.log(res)
  } catch (error) {
    // isCancel方法可以用来判断该请求是请求失败原因还是取消请求原因。
    if (axios.isCancel(error)) {
      console.log('取消请求', error.message)
    } else {
      // 这里是处理错误
      console.log('请求错误', error.message)
    }
  }
})
```

**说明：**

>- CancelToken方法中有 `source`方法。
>- source方法返回的是一个对象，该对象里面有 `token取消令牌`和 `cancel取消令牌方法`。
>- 使用cancel()表示取消令牌操作，以此可以取消请求。
>
>![image-20210914183052102](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210914183103.png)



### 7.2 取消请求方法二

可以通过将一个执行器函数(executor) 传递给 `CancelToken` 构造函数来创建取消令牌。

```js
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```



### 7.3 取消请求实用地方

可以配合 `axios拦截器` 来添加 `取消令牌` 并且可以 `取消请求`。

```js
let cancel
axios.interceptors.request.use(
  config => {
    console.log(config)
    // 拦截添加消除请求的token令牌
    const { CancelToken } = axios
    // 如果这个请求存在
    if (cancel) {
      cancel('取消了这次请求...')
    }
    // 拦截请求并且在请求的配置信息中添加消除请求的令牌
    config.cancelToken = new CancelToken(function executor(c) {
      // c是一个函数
      cancel = c
    })
    return config
  },
  error => {
    return Promise.reject(error)
  }
)

const btn = document.querySelector('button')
btn.addEventListener('click', async function () {
  try {
    const { data: res } = await axios({
      method: 'GET',
      url: 'http://localhost:3000/gets',
      params: {},
    })
    // 当响应数据回来后，就可以消除这个请求
    cancel()
    console.log(res)
  } catch (reason) {
    if (axios.isCancel(reason)) {
      console.log(reason.message, 111)
    } else {
      console.log('请求出错了', reason.message)
    }
  }
})

```



## 八、二次封装axios

**二次封装axios代码：**

```js
/* axios二次封装 */
import axios from 'axios'
import router from '../router'
import qs from 'qs'

// 设置统一URL地址前缀
const baseURL = 'xxxxxxxx' //TODO 自己看情况设置

// 设置为 'application/x-www-form-urlencoded' 是为了解决后端接收不到前端发送的数据
const headers = { 'Content-Type': 'application/x-www-form-urlencoded' }

// `transformRequest` 允许在向服务器发送q请求前，修改请求数据
const transformRequest = function (data, headers) {
  let ContentType = headers['Content-Type'] || headers.common['Content-Type'] || headers.post['Content-Type'] || 'application/json'
  if (ContentType === 'application/json') {
    return JSON.stringify(data)
  }
  if (ContentType === 'application/x-www-form-urlencoded') {
    // qs插件是处理url中参数的一个js库
    return qs.stringify(data)
  }
  return data
};

// 设置请求超时的时间
const timeout = 8000

// withCredentials 表示跨域请求时是否需要使用凭证。
const withCredentials = true

// validateStatus 定义对于给定的 HTTP 响应状态码是 resolve还是 reject.
const validateStatus = function (status) {
  // 自定义响应成功的HTTP状态码
  return /^(2|3)\d{2}$/.test(status)
}

export default function $axios (config = {}) {
  const instance = axios.create({
    baseURL,
    timeout,
    headers,
    transformRequest,
    withCredentials,
    validateStatus,
  })

  // 设置请求次数，请求的间隙
  instance.defaults.retry = 4 // 重新发送请求次数
  instance.defaults.retryDelay = 1000 // 每次重新发送请求的间隔时间

  /* 请求拦截器 */
  instance.interceptors.request.use(config => {
    let token = localStorage.getItem('token')
    // 如果token存在
    token && (config.headers['Authorization'] = token)
    return config
  })

  /* 响应拦截器 */
  instance.intercepors.response.use(response => {
    // 只返回响应主体中的信息（部分公司根据需求会进一步完善，例如指定服务器返回的CODE值来指定成功还是失败）
    return response.data
  }, error => {
    // 从服务器没有获取到数据（即网络层失败）
    // let response = null
    const { response } = error
    if (response) {
      // 服务器会有响应，只不过状态码是4/5开头的,所以判断进行处理
      const { status } = response
      switch (status) {
        case 401:
          error.message = '未授权，请登录'
          router.push('/auth')
          // 一般情况下都是未登录，此时在这里写路由跳转到登录页。
          break
        case 403:
          // 一般情况下是TOKEN过期
          error.message = 'TOKEN过期'
          localStorage.removeItem('token')
          //TODO 调整到登录页
          break
        case 404:
          error.message = `请求地址不存在: ${error.response.config.url}`
          // 一般情况下都是地址不存在。
          break
        case 500:
          error.message = '服务器内部错误'
          break
      }
      return Promise.reject(response)
    } else {
      /**
         * 连接超时或者断开网络了，此时进行判断。
         * 注意，超时response是为undefined。
        */
      // 情况一: 断开了网络
      if (!window.navigator.onLine) {
        // 断开网络了，可以让其跳转到断网页面
        // router.push('/auth')
        return
      }
      // 情况二: 连接超时
      if (error && error.code === 'ECONNABORTED') {
        // 处理超时的情况
        const { config } = error
        // 如果配置不存在或未设置重试选项，则拒绝
        if (!config || !config.retry) return Promise.reject(error)

        // 设置用于跟踪重试次数的变量
        config.__retryCount = config.__retryCount || 0

        // 检查我们是否已达到重试总数
        if (config.__retryCount >= config.retry) {
          // 拒绝错误
          return Promise.reject(error)
        }

        // 增加重试次数
        config.__retryCount += 1

        //创建新的promise来处理指数退避
        const backoff = new Promise(function (resolve) {
          setTimeout(function () {
            resolve()
          }, config.retryDelay || 1)
        })

        // 返回调用 axios 重试请求的承诺
        return backoff.then(function () {
          return instance(config)
        })

      }
      return Promise.reject(error)
    }
  })
  // 然后改实例出去
  return instance(config)
}

```



