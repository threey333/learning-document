# JWT知识

## 一、认识JWT

`JWT` 即 `JSON Web Tokens`；它是目前最流行的**跨域认证**解决方案，在集群环境下使得用户的认证变得简单，解放了实现服务器间session共享的复杂逻辑。



## 二、认证方案总结与对比

### 2.1 为什么要进行用户认证

由于**==http协议本身是一种无状态的协议==**，这意味着会出现如下的问题：

- 一、如果用户向我们的应用提供了用户名和密码来进行用户认证，那么下一次发送请求的时候，用户还要再一次进行用户认证（即每次发送请求都要进行认证）。
- 二、不知道请求是哪一个用户发送过来的。

为了解决这些问题，我们再开发应用的时候都需要进行用户认证。

认证的方案目前有两种：1.`传统的session认证`、2.`JWT认证`。



### 2.2 传统session认证

当用户进行登录的时候，我们在服务器存储一份用户登录的信息，这份登录信息会在响应时传递给浏览器并告诉浏览器保存为 `cookie`，以便下次请求时发送给我们的应用，这样我们的应用就能识别请求来自哪个用户了，这就是 `传统的session认证`。

>注意，传统的session认证要基于cookie进行发送，而且还会存在跨域问题。

**传统session认证出现的问题：**

- 一、==对服务器压力大==：因为每一个用户经过我们的应用服务器认证之后，服务器都会做一次记录，而这些记录都是保存在服务器的内存中。随着用户的数量增多，服务器的开销明显增大。

- 二、==程序扩展性变差==：用户认证后，处理认证的服务器就做了认证记录。由于认证记录是记录在该台服务器的内存中，这就意味着用户下一次发送请求必须从发送到这条这台服务器，这样才能拿到授权的信息资源。这种方式在分布式应用中会相应限制负载均衡的能力，也就意味着限制了扩展的能力。

- 三、==安全性偏低==：因为基于 `cookie` 进行用户识别认证，一旦 `cookie` 被别人拦截捕获到，用户就会很容易收到跨站请求的伪造攻击。

![image-20220225224224384](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220225224232.png)



### 2.3 JWT认证(基于token的鉴权机制)

`JWT认证` 主要用于需要**跨域**进行认证的场景，例如服务A在认证了用户身份后，颁发一个很短过期时间的 `JWT` 给客户端，客户端在向服务B的请求中带上该 `JWT`，则服务B可以通过验证该 `JWT` 来判断用户是否有权执行服务B上的相关操作。

**JWT流程大致是这样：**

- 用户使用用户名和密码进行登录，登录就会向服务端发送请求。
- 服务端接收到该用户的信息请求后，开始验证用户的信息。
- 服务端验证完用户信息并将其进行编码加密成一个 `token` 字符串，然后返回给用户。
- 用户的客户端接收该响应的 `token` 字符串后存储在本地中。
- 后面用户每次向服务端发送请求都会携带这个 `token` 信息。
- 服务端接收到该请求后，会从该请求中的 `token` 值进行解密认证。
- 认证无误后返回数据给用户。



## 三、JWT的原理

`JWT` 的原理是，服务端认证以后，会生成一个 `JSON` 对象，然后响应返回给用户。

```json
{
  "姓名": "张三",
  "角色": "管理员",
  "到期时间": "2018年7月1日0点0分"
}
```

以后，用户这边与服务端通信发送请求的时候，都要携带这个 `JSON` 对象，因为服务端是依靠这个 `json` 对象进行用户身份认证。

==**为了防止用户篡改数据，服务端在生成JSON对象的时候，都会加上签名。**==

对于服务端来说，不需要保存任何 `session` 数据。也就是说，服务端变成无状态了，从而比较容易实现扩展。



## 四、JWT的数据结构

我们使用 `jwt.sign()` 生成一个 `JWT数据`。

```js
var jwt = require('jsonwebtoken')
var token = jwt.sign('aaaaa', 'shhhhh')

console.log(token) // => 'eyJhbGciOiJIUzI1NiJ9.YWFhYWE.GklBAyS8EHsvzau1hPjrt31C0AlDbU0kIxo6ZRDho4U'
```

`JWT` 的数据结构是一个很长的字符串，中间用点 `.` 分隔成三个部分。

`JWT数据` 由三个部分组成，这三个部分依次为：

- `Header`（头部）。
- `Payload`（有效关键信息）。
- `Signature`（签名）。

它们组成在一起就是 `JWT数据`。

```markdown
Header.Payload.Signature
```

用图表示 `JWT`：

![image-20220226234244195](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220226234244.png)



### 4.1 Header

Header部分 是一个 `JSON` 对象，描述 `JWT` 的元数据。

它承载了两个信息：

- 一个是`声明令牌的类型`。
- 另一个是`声明加密的算法`。

Header部分 如下所示：

```json
{
  "typ": "JWT",
  "alg": "HS256",
}
```

上面代码的解释：

- `alg` 属性表示的是加密的算法（algorithm），默认是 HMAC SHA256（写成 HS256）。
- `typ` 属性表示的是令牌的类型，JWT令牌统一写为 `JWT`。

最后 Header部分 使用Base64URL算法进行加密（该加密是可以对称解密的)，**加密后转成字符串（如下），也就是JWT数据结构的第一部分。**

```markdown
eyJhbGciOiJIUzI1NiJ9
```



### 4.2 Payload

Payload部分 也是一个 `JSON` 对象，用来**存放实际需要传递的数据。**

JWT 官方规定了7个字段，供选用（建议但不强制使用）：

```markdown
- iss (issuer)：签发人。
- exp (expiration time)：过期时间。
- sub (subject)：主题。
- aud (audience)：受众。
- nbf (Not Before)：生效时间。
- iat (Issued At)：签发时间。
- jti (JWT ID)：编号。
```

除了官方字段，你还可以在这个部分定义私有字段，例如：

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

==**注意，JWT 默认是不加密的，任何人都可以读到，所以不要把重要的秘密信息放在这个部分。**==

这个部分最后也会使用Base64URL算法进行加密，**加密后转成字符串（如下）,也就是JWT数据结构的第二部分。**

```json
YWFhYWE
```



### 4.3 Signature

Signature部分 是对前两部分的签名，防止数据篡改。

也就是说，这个部分需要 `base64加密后的header` 和 `base64加密后的payload`，然后通过 `.` 连接组成字符串，最后通过指定的密钥和 `header` 里面指定的加密算法进行组合，这样就产生了 `JWT` 的第三部分（如下所示）。

```js
const encodedString = base64UrlEncode(header) + '.' + base64UrlEncode(payload);
const signature = HMACSHA256(encodedString, 'secret'); // => GklBAyS8EHsvzau1hPjrt31C0AlDbU0kIxo6ZRDho4U
```

**将 Header、Payload和 Signature 这三部分用 `.` 进行拼接成一个完整的字符串，最终构成 `JWT`：**

```markdown
eyJhbGciOiJIUzI1NiJ9.YWFhYWE.GklBAyS8EHsvzau1hPjrt31C0AlDbU0kIxo6ZRDho4U
```

>**注意**：
>
>- **secret密钥是保存在服务端的，jwt的加密生成也是在服务端的**。
>
>- secret密钥就是用来进行 `jwt` 的加密和 `jwt` 的解密，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。
>- 一旦客户端得知这个secret, 那就意味着客户端是可以自我加密 `jwt` 了。



## 五、JWT的几个特点

一、`JWT` 默认是不加密，但也是可以加密的。生成的 `原始Token` 以后，可以用密钥再加密一次。

```js
const token1 = jwt.sign('aaaaa', 'shhhhh') 
// => eyJhbGciOiJIUzI1NiJ9.YWFhYWE.GklBAyS8EHsvzau1hPjrt31C0AlDbU0kIxo6ZRDho4U

// 对token1再加密一次
const token2 = jwt.sign('token1', 'shhhhh')
// => eyJhbGciOiJIUzI1NiJ9.dG9rZW4.1LU3pqJldzkmxsMjlelJkQimX-Q-YCNTlA2SM94Z7cw
```

二、`JWT` 不加密的情况下，不能将重要的秘密数据写入到 `JWT` 中。

三、`JWT` 不仅可以用于认证，还可以用于交换信息。有效使用 `JWT`，可以有效降低服务器查询数据库的次数。

四、`JWT` 的最大缺点是，由于服务器不保存 `session` 状态，因此无法在使用过程中废止某个 `token`，或者更改 `token` 的权限。也就是说，一旦 `JWT` 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

五、`JWT` 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。**为了减少盗用，`JWT` 的有效期应该设置得比较短。**对于一些比较重要的权限，使用时应该再次对用户进行认证。

六、为了减少盗用，`JWT` 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。



## 六、JWT的使用方式

一、客户端收到服务端返回的 `JWT`（可以认为是 `token` ），可以存储在 `Cookie` 里面，也可以存储在 `localStorage` 中。

此后，客户端每次与服务端进行通信，都要带上这个 `JWT`，因为服务端要进行认证。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息 `Authorization` 字段里面。

```js
Authorization: Bearer <token>

// 使用示例
config.headers.Authorization = token
```

二、另一种做法是，跨域的时候，`JWT` 就放在 POST 请求的数据体里面。



## 七、JWT相关API的使用

API相关使用文档：[https://www.npmjs.com/package/jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)。

### 7.1 签名加密 - jwt.sign()

使用 `jwt.sign()` 可以让我们的有效关键信息进行签名加密。

>API方法: jwt.sign(payload, secretOrPrivateKey, [options, callback])

说明:

- 如果该方法存在回调函数，则该方法是异步方法。
- 如果不存在回调函数，则该方法是同步方法，并且会返回一个字符串类型的JWT。

```js
// 同步方法
const token1 = jwt.sign({ account: 12346789, password: 134567898 }, 'shhhhh')
console.log(token1) 
// =>'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50IjoxMjM0Njc4OSwicGFzc3dvcmQiOjEzNDU2Nzg5OCwiaWF0IjoxNjQ1OTU1ODkxfQ.5URwzm8OnBxGbkMI3riz2U2GySDWbyHw6Vz39g0_34A

// 异步方法
const token2 = jwt.sign({ account: 12346789, password: 134567898 }, 'shhhh', (err, tok) => {
  if (err) throw new Error(err)
  console.log(2, tok) // => 2 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50IjoxMjM0Njc4OSwicGFzc3dvcmQiOjEzNDU2Nzg5OCwiaWF0IjoxNjQ1OTU1ODkxfQ.CSCWLu_9QEZt_ZTjeZPHHi2vdALdGyhPivt8moF5sFs
})
console.log(1, token2) // => 1 undefined
```

参数具体信息说明：

- `payload` <object> | <string> | <Buffer>：有效关键数据。当它是对象形式的时候，可以设置官方规定的七个字段（如 `iss`、`exp`、`sub`、`aud`、`nbf`、`iat`、`jti`）。同时对象形式的 `payload` 会被 `JSON.stringify` 强制转换为字符串。

  >1. 生成的 `JWT` 在默认情况下是含有 `iat` 签发时间，除非在 option选项参数中添加 `noTimestamp` 字段。
  >2. 如果在 `payload` 中添加了 `iat`，那么它将被用来代替真正的时间戳来计算其它东西，比如options.expiresIn中给定时间间隔的exp。

- `secretOrPrivateKey` <object> | <string> | <Buffer>：密钥。1.称加密时是密钥，2.非对称加密时分情况：加密签名的时候使用公钥，解密验证的时候使用私钥。

- `options` <object>：可选择的字段属性（如下）。

  >1. `algorithm` : 加密算法，默认是 `HS256`（对称加密）。
  >2. `expiresIn` : 过期时间。例如:60，“2天”，“10h”，“7d”。数值被解释为秒计数。如果使用字符串，请确保提供时间单位(天、小时等)，否则默认使用毫秒单位(“120”等于“120ms”)。
  >3. `notBefore` : 生效时间。例如:60，“2天”，“10h”，“7d”。数值被解释为秒计数。如果使用字符串，请确保提供时间单位(天、小时等)，否则默认使用毫秒单位(“120”等于“120ms”)。
  >4. `audience` : 受众。
  >5. `issuer` : 签发人。
  >6. `subject` : 主题
  >7. `jwtid` : 编号。
  >8. `noTimestamp` : 是否有时间标志（该值是个boolean值）。
  >9. `header` : 头部
  >10. `keyid` : 键值id。一般是mongoDB数据库中的 `主键_id` 。
  >11. `mutatePayload`：如果为真，sign函数将直接修改有效负载对象。如果在声明应用到有效负载之后，但在它被编码成令牌之前，您需要对它的原始引用，那么这是很有用的。（了解）

  **注意地方：**

  一、上面 2-5 字段属性是没有默认值的。

  二、因为当 `payload` 参数为对象形式的时候也可以设置官方规定的字段，所以一处设置了，另一处 `options` 就不能再设置。

示例一：

```js
const token = jwt.sign({ account: 12346789, password: 134567898 }, 'secret')
console.log(token) // => 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50IjoxMjM0Njc4OSwicGFzc3dvcmQiOjEzNDU2Nzg5OCwiaWF0IjoxNjQ1OTY0OTI2fQ.1GjvxBB8sIFZFQKchxy6OSYyTdyRSwDVjjxdrt9nfXc'

// Header部分: {"alg":"HS256","typ":JWT}
// Payload部分: {"account": 12346789,"password": 134567898,"iat": 1645964926}
// Signature部分:HMACSHA256(base64UrlEncode(header) + "." +base64UrlEncode(payload), secret)
```

示例二：设置有过期时间的 `token` 令牌。

```js
//设置有过期时间的token令牌
// 写法一:
const token = jwt.sign({
  exp: Math.floor(Date.now() / 1000) + (60 * 60),
  data: 'foo bar',
  _id: 1,
  name: 'Eson'
}, 'secret')

// 写法二：
const token1 = jwt.sign({ 
  account: 12346789, 
  password: 134567898 
}, 'secret', { 
  expiresIn: 60 * 60 
})
console.log(token)
```

如果该 `token` 令牌过期了，那么是不能验证解密的。

```js
const res = jwt.verify(token, 'secret')

console.log(res) // => 报错：jwt expired，即token过期
```



示例三：设置签名加密的开始时间（这个例子把签名加密的时间提前了）。

```js
const token = jwt.sign({
  id: 3,
  name: 'yyy',
  iat: Math.floor(Date.now() / 1000) - 30
}, 'shhhhh')
```



#### 7.1.1 对称加密

签名加密默认使用的就是对称加密（HS256），说明加密和解密使用的密钥都是同一个。

由于是对称加密方式，以至于这个密钥很重要，不能随便泄露给别人知道，不然别人就能通过这个密钥获取权限。

```js
// HS256对称加密
const token = jwt.sign({ account: 12346789, password: 134567898 }, 'secret')
```



#### 7.1.2 非对称加密

当我们对有效关键数据进行加密时，如果使用的是 `RS256` 加密方式，那么它就非对称加密。

非对称加密是个几乎不能破解的加密算法，所以建议使用非对称加密。

使用非对称加密的步骤：

- 一、在服务端生成私钥和公钥。

  ```markdown
  - 非对称密钥生成
  > openssl 
  - 生成私钥
  openssl > genrsa -out rsa_private_key.pem 2048
  
  - 根据私钥生成公钥
  openssl > rsa -in rsa_private_key.pem -pubout -out rsa_public_key.pem
  ```

- 二、对关键数据进行加密的时候，采用的是私钥进行加密，同时要在 `options` 选项上的 `algorithm` 设置为 `RS256` 非对称加密算法。
- 三、验证解密的时候，使用公钥进行解密。

示例：

```js
// 非对称加密
const readData = () => {
  return new Promise((resolve, reject) => {
    fs.readFile(path.join(__dirname, './secretKey', 'rsa_private_key.pem'), (err, data) => {
      if (err) reject(new Error(err))
      resolve(data)
    })
  })
}
readData().then(res => {
  const secretKey = res
  const token = jwt.sign({ account: 12346789, password: 134567898, id: 3 }, secretKey, {
    algorithm: 'RS256'
  })
  console.log(token) // =>eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhY2NvdW50IjoxMjM0Njc4OSwicGFzc3dvcmQiOjEzNDU2Nzg5OCwiaWQiOjMsImlhdCI6MTY0NTk2ODYzM30.smDIT8zf8Hxu8wWnThQAJU_QrKag04CGGq5yRkwYkJs
  return token
}).then(token => {
  // 非对称解密
  const publicKey = fs.readFileSync(path.join(__dirname, './secretKey', 'rsa_public_key.pem'))
  const result = jwt.verify(token, publicKey)
  console.log(result) // => 验证解密出来的结果: { account: 12346789, password: 134567898, id: 3, iat: 1645969229 }
})
```



### 7.2 验证解密 - jwt.verify()

签名加密后生成了 `token` 令牌，将该令牌返回给用户，下次用户发送请求过来的时候，我们就是通过验证该 `token` 令牌，然后给用户授权。

使用 `jwt.verify()` 方法可以解密从服务端加密过的 `token` 令牌，然后对用户进行认证。

>API方法 : jwt.verify(token, secretOrPublicKey, [options, callback])

说明：

- 如果该方法存在回调函数，则该方法是异步方法。
- 如果不存在回调函数，则该方法是同步方法，并且会返回原来的 `payload` 有效关键数据。

>注意：如果 `token` 令牌还处在有效时间内或签发人、受众有效，则可以进行验证解密，否则会报错。

参数具体信息说明：

- `token` <string>：签名加密后的 `token` 字符串。

- `secretOrPublicKey` <string> | <Buffer> | <object>：解密的密钥。如果是非对称加密则是公钥。

- `options` <object>：可选择的字段属性。

  >1. `algorithms` : 可进行验证解密的算法名称字符串列表。例如:["HS256"，"RS256"]。
  >
  >2. `audience` : 检查签名加密时的 `audience`。这里可以是字符串、正在表达式 或 （字符串列表或正则表达式列表）。
  >
  >3. `complete` : 该字段的值是 boolean 类型，如果为 true 会返回一个带有已解码的{header，payload，signature}对象（如下）。
  >
  >4. `issuer` : (可选):iss字段的有效值的字符串或字符串列表。
  >
  >5. `ignoreExpiration` : 该值是个 boolean 类型，如果为 true 则不验证令牌的到期。
  >
  >6. `ignoreNotBefore` : 该值是个boolean类型，如果为 true 则不验证令牌生效时间。
  >
  >7. `subject` : 如果要检查主题 ( `sub`)，请在此处提供一个值。
  >
  >8. `maxAge`：令牌仍然有效的最大允许年龄。它以秒或描述时间跨度 zeit/ms 的字符串表示。
  >
  >   ```markdown
  >   例如：`1000`, `"2 days"`, `"10h"`, `"7d"`. 数值被解释为秒数。如果您使用字符串，请确保提供时间单位（天、小时等），否则默认使用毫秒单位（`"120"`等于`"120ms"`）。
  >   ```
  >
  >9. `nonce`：如果要检查`nonce`声明，请在此处提供字符串值。它用于 ID 令牌的 Open ID

示例一：对 `token` 令牌过期或未过期的验证解密情况。

```js
const token = jwt.sign({ _id: 3, name: 'yyy', exp: Math.floor(Date.now() / 1000) + (3 * 1) }, 'secret')

//一、token未过期的时候可以进行验证解密
const result1 = jwt.verify(token, 'secret')
console.log(result1) // => { _id: 3, name: 'yyy', exp: 1645974455, iat: 1645974452 }

//二、3秒后token过期，verify对过期的token不能正常解密，以至于抛出错误
setTimeout(() => {
  const result2 = jwt.verify(token, 'secret') // TokenExpiredError: jwt expired
}, 3000)
```



示例二：异步验证 `token` 令牌情况。

```js
const token = jwt.sign({ _id: 3, name: 'yyy', exp: Math.floor(Date.now() / 1000) + (3 * 1) }, 'secret')

// 异步验证解密token令牌。
jwt.verify(token, 'secret', (err, data) => {
  console.log(data) // => { _id: 3, name: 'yyy', exp: 1645974706, iat: 1645974703 }
})
```



示例三：捕获解密失败的错误。

```js
const token = jwt.sign({ _id: 3, name: 'yyy', exp: Math.floor(Date.now() / 1000) + (3 * 1) }, 'secret')

// 捕获验证解密失败的错误
try {
  const result = jwt.verify(token, 'shhh')
  console.log(result)
} catch (err) {
  console.log(new Error(err).message) // => JsonWebTokenError:无效的签名
}
```



示例四：非对称解密。

```js
// 获取公钥
const publicKey = fs.readFileSync(path.join(__dirname, './secretKey', 'rsa_public_key.pem'))
// 利用公钥解密
const result = jwt.verify(token, publicKey)
```



示例五：`options` 下的 `algorithms` 字段值是一个字符串列表情况。

```js
jwt.verify(token, 'secret', { algorithms: ['HS256', 'RS256'] }, (err, data) => {
  if (err) throw new Error(err)
  console.log(data) // => { _id: 3, name: 'yyy', exp: 1645975421, iat: 1645975418 }
})
```



### 7.3 返回已解码的有效关键数据 - jwt.decode()

使用 `jwt.decode()` 方法可以返回已解码的有效关键数据。

>API方法：jwt.decode(token [, options])。

>注意：这个API方法不会验证签名是否有效，它只是单纯就返回已解码的有效关键数据 `payload`。

参数具体信息说明：

- `token` <string>：签名加密后的 `token` 字符串。

- `options` <object>：可选择的字段属性。

  >1. `json` : 即使 header部分不含有"type":"JWT",也要解释 `payload`。
  >
  >2. `complete` : 该值是个boolean类型，如果为true，则会返回 `JWT`三个部分信息。
  >
  >   ```js
  >   console.log(jwt.decode(token, { complete: true })) 
  >   
  >   //结果: 
  >   {
  >     header: { alg: 'HS256', typ: 'JWT' },
  >     payload: { _id: 3, name: 'yyy', exp: 1645976060, iat: 1645976057 },
  >     signature: 'jcf4LYN72pTmmteYl5dAub0RQb_chqIzrKuOvVpzSEQ'
  >   }
  >   ```



## 八、支持的算法

支持当前以下算法。

| alg 参数值 | 数字签名或 MAC 算法                                          |
| ---------- | ------------------------------------------------------------ |
| HS256      | HMAC 使用 SHA-256 哈希算法                                   |
| HS384      | HMAC 使用 SHA-384 哈希算法                                   |
| HS512      | HMAC 使用 SHA-512 哈希算法                                   |
| RS256      | RSASSA-PKCS1-v1_5 使用 SHA-256 哈希算法                      |
| RS384      | RSASSA-PKCS1-v1_5 使用 SHA-384 哈希算法                      |
| RS512      | RSASSA-PKCS1-v1_5 使用 SHA-512 哈希算法                      |
| PS256      | 使用 SHA-256 哈希算法的 RSASSA-PSS（仅节点 ^6.12.0 或 >=8.0.0） |
| PS384      | 使用 SHA-384 哈希算法的 RSASSA-PSS（仅节点 ^6.12.0 或 >=8.0.0） |
| PS512      | 使用 SHA-512 哈希算法的 RSASSA-PSS（仅节点 ^6.12.0 或 >=8.0.0） |
| ES256      | ECDSA 使用 P-256 曲线和 SHA-256 哈希算法                     |
| ES384      | ECDSA 使用 P-384 曲线和 SHA-384 哈希算法                     |
| ES512      | ECDSA 使用 P-521 曲线和 SHA-512 哈希算法                     |
| 没有       | 不包含数字签名或 MAC 值                                      |

