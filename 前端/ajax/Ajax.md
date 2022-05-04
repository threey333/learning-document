# Ajax

## 一、HTTP

### 1.1 超文本传输协议

​		http(hypertext transport protocol)协议【**即超文本传输协议**】，协议详细规定了浏览器和万维网服务器之间互相通信的规则。

​		HTTP与平台无关，在任何平台都可以使用HTTP访Internet上的文档。

### 1.2 请求/响应报文结构

请求报文由`请求行`、`头部`构成，有时还有`主体`。

![image-20210429212703039](https://cdn.jsdelivr.net/gh/threey333/Picture/ajax/image-20210429212703039.png)

```http
请求行：请求类型、URL、HTTP版本
请求行			POST（请求类型）  /s?ie=uft-8（URL）  HTTP/1.1（HTTP版本）
头部			 Host:atguigu.com
			  Cookie:name=guigu
			  Content-type:application/x-www-form-urlencoded
			  User-Agent:chrome 83
空行
主体			  username=admin$password=admin   
			(如果是get请求，主体为空，如果是post请求，主体可以不为空)。
```

==头部的规律：都是键值对形式。==



响应报文由`状态行`、`头部`构成，有时还有`主体`。

![image-20210429212629724](https://cdn.jsdelivr.net/gh/threey333/Picture/ajax/image-20210429212629724.png)

```html
状态行：HTTP版本、空格、状态代码、空格、状态语句
状态行					HTTP/1.1（HTTP版本）  200(状态代码)  OK（状态语句）
头部					 Content-Type:text/html;charset=utf-8
					  Content-length:2048
					  Content-encoding:gzip
空行
主体					<html>
    					<head></head>
    					<body><h1>
                            计算机网络
                            </h1>
    					</body>
					  </html>
```

==头部的规律：都是键值对形式。==



## 二、安装服务端

### 2.1 express框架的安装

因为使用ajax需要服务端，所以借此框架搭建服务端。

1. `在vscode中文件最外层`打开终端，==注意：目录路径不能包含有中文名字==
2. 使用`npm init --yes`初始化，安装完后在最底层的目录会出现`package.json`文件（npm init --yes是直接配置的意思）
3. 使用`npm i express`安装express框架。 

![image-20210430163617426](https://cdn.jsdelivr.net/gh/threey333/Picture/ajax/image-20210430163617426.png)

![image-20210430163656321](https://cdn.jsdelivr.net/gh/threey333/Picture/ajax/image-20210430163656321.png)

![image-20210430165613126](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430165613126.png)



### 2.2 配置express.js文件

express.js文件（这个文件是自己建的）相当于服务器后台，所以这里我们要自己手动使用express框架（图下）

![image-20210430171542715](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430171542715.png)

​		写完后开启服务，使用node命令：`node  文件名.js`，如果出现监听端口启动服务出现的“服务已经启动，8000端口启动中”，则说明服务启动成功。

![image-20210430172012922](https://cdn.jsdelivr.net/gh/threey333/Picture/ajax/image-20210430172012922.png)

​		在网页中打开访问网址：127.0.0.1:8000就可以看到结果。（8000是我们自己设置的端口号，如果不设置8000而是设置3000，就打开访问网站：127.0.0.1:3000）。



### 2.3 开启和关闭服务

1. 开启：在你所要开启的目录打开终端，输入命令`node 文件名.js`就可以开启服务。
2. 关闭：快捷键`Ctrl+C` 关闭服务。



### 2.4 访问网站中查看报文结构

​		1.按F12点击Network处，在`Header`处可以查看`请求和响应`的行、头部。

示例：响应报文结构

![image-20210430173617936](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430173617936.png)

响应报文主体

![image-20210430174046544](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430174046544.png)

请求报文结构

![image-20210430173819255](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430173819255.png)



## 三、 使用原生AJAX

### 3.1 XMLHttpRequest的方法

|              方法              | 描述                                                         |
| :----------------------------: | :----------------------------------------------------------- |
|            abort()             | 取消当前请求                                                 |
|    getAllResponseHeaders()     | 获取所有的响应头信息                                         |
|   getResponseHeader(header)    | 获取指定的响应头信息，参数header:指定HTTP头名称，例如:getResponseHeader("Content-Type") |
|   ==open(method,url,async)==   | 实例对象初始化,其中method：请求类型，url：文件在服务器上位置，async:确定是否异步处理，取true(异步)或fasle(同步)两个值，默认是true。 |
|        ==send(string)==        | 将HTTP请求发送到服务器。对`POST`请求可以使用string参数，而`GET`请求则不需要参数。 |
| setRequestHeader(header,value) | 设置指定的HTTP头信息，参数header:指定的HTTP头名称；参数value：指定HTTP头信息 |



### 3.2 XMLHttpRequest属性

|        属性        | 描述                                                         |
| :----------------: | :----------------------------------------------------------- |
|   ==readyState==   | 状态有0 1 2 3 4 <br/>0代表为初始化状态<br/>1代表open()方法调用完毕<br/>2代表send()方法调用完毕<br/>3代表服务端返回的部分结果<br/>4代表服务端返回的全部结果 |
| onreadystatechange | 事件属性，指定当readystate属性值改变时的事件处理回函数       |
|     ==status==     | 状态码：<br/>200 OK:表示客户端请求成功。<br/>400 Bad Request：表示客户端请求有语法错误，不能被服务器理解<br/>403 Forbidden：表示客户端收到了请求，但拒绝提供服务<br/>404 Not Found：表示请求的资源不存在<br/>500 Internal Server Error：服务器发生了不可预期的错误<br/>503 Server Unavailable:服务器当前不能处理客户端的请求，但一段时间后，可能回复正常 |
|    responseBody    | 将响应信息正文以Unsigned Byte数组形式返回                    |
|   responseStream   | 以ADO Stream对象的形式返回响应信息                           |
|  ==responseText==  | 将响应信息作为字符串返回                                     |
|    responseXML     | 将响应信息格式化为XML文档格式返回                            |
|    responseType    | 将响应信息转为我们所需要的格式。                             |
|      timeout       | 一个无符号长整型（`unsigned long`）数字，表示该请求的最大请求时间（毫秒），若超出该时间，请求会自动终止 |



### 3.3 XMLHttpRequest事件

|   事件    |                             描述                             |
| :-------: | :----------------------------------------------------------: |
|   error   |                  当request遭遇错误时触发。                   |
|   load    |              XMLHttpRequest请求成功完成时触发。              |
|  loaded   | 当请求结束时触发，无论请求成功(load)还是失败(abort或error)。 |
| loadstart |                    接收到响应数据时触发。                    |
| progress  |            当请求接收到更多数据时，周期性地触发。            |
|  timeout  |              在预设时间内没有接收到响应时触发。              |



### 3.4 获取服务端的响应

![image-20210502163947957](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502163947957.png)

>事件绑定中的事件类型

处理服务器端返回的结果有两种事件类型：

1. 使用`onload`事件类型
2. 使用`onreadystatechange`事件类型

两者区别：

- onload不用判断状态码，被调用一次
- onreadystateChange需要判断状态码，当状态码发生改变时就需要重新调用

>onreadystatechange事件类型说明

1. on==>跟when差不多，都有当....时候的意思

2. readyState是XMLHttpRequest实例对象中的属性，表示`状态`

   ```html
   状态有0 1 2 3 4 
   0代表为未初始化状态（即还没有调用open()）
   1代表open()方法调用完毕
   2代表send()方法调用完毕
   3代表服务端返回的部分结果
   4代表服务端返回的全部结果
   ```

3. change==>改变的意思，意思就是用这个事件类型可以当状态码发生改变后可以重新调用

>总结

今后能用onload就用onload（推荐），方便而且效率高。



### 3.5 原生Ajax-get请求

​		因为发送AJAX请求默认是要遵循==同源策略==，所以要在服务台文件中设置响应头（设置允许跨域）

![image-20210430180820024](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430180820024.png)

​		express框架的服务台可以在任何的js中（复制粘贴就行），只需要在==接收请求的URL中引入相关的路径==，在网站中打开也需要在后面`添加相应的URL`。

示例：

![image-20210430181326990](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430181326990.png)



前台Ajax请求步骤：

1. 创建XMLHttpRequest实例对象
2. 调用XMLHttpRequest实例对象的open()方法初始化，设置`请求类型`和`url`
3. 调用XMLHttpRequest实例对象的send()方法发送请求
4. 事件绑定，处理服务端返回的结果

![image-20210430205358635](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430205358635.png)



### 3.6 原生AJax-post请求

### 3.7 Ajax设置请求参数

> 1.get请求类型，直接在url后面加参数

![image-20210430224006952](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430224006952.png)

==说明：get请求只能是`application/x-www.form-urlencoded`的参数格式==即

```javascript
a=100&b=200&c=300
```



> 2.post请求类型，在`send()方法`中添加参数

post请求参数必须要设置请求头信息：

![image-20210502174330862](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502174330862.png)

![image-20210430230421411](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430230421411.png)

​		==post请求中的参数可以设置任何发送类型格式，但最好还是json格式，因为后端也能处理。==

示例：

![image-20210430231703712](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210430231703712.png)

>请求参数格式

1.application/x-www.form-urlencoded

```
a=100&b=200&c=300
```

2.application/json

```
{name:"yangyeyong,age:18"}
```

​		**在设置请求头信息的时候，Content-Type属性的值是application/json,告诉服务器当前请求参数的格式是json**

​		又因为send()方法发送请求参数必须是字符串类型，所以要先转为json字符串

```javascript
JSON.stringify()
```



### 3.8 Ajax设置请求头信息

1. ==请求头信息必须在实例对象调用open()方法之后，实例对象调用send()方法之前==
2. 设置请求头信息运用到了XMLHttpRequest方法中的`setRequestHeader`方法。
3. `POST请求`必须要设置请求头信息。（方便请求参数格式的类型）

![image-20210501131538429](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501131538429.png)

在Network处查看我们所设置的请求头信息：

![image-20210501131727887](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501131727887.png)



### 3.9 服务端响应的json数据

当服务端返回的数据格式是**json数据**时，我们前端接收json数据并且**转换对象数据类型**可以有两种方式：

1. 先创建XMLHttpRequest实例对象，然后利用实例对象的`setRequest`属性，将json数据转为为对象数据类型

   ![image-20210501135121218](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501135121218.png)

![image-20210501135140075](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501135140075.png)



​	2.我们手动将返回的json数据格式转换为对象数据类型，利用`JSON.parse()方法`

![image-20210501135452188](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501135452188.png)



### 3.10 安装nodemon工具

​		**nodemon工具能自动重启我们的服务端**。（即当我们在服务端修改代码时能自动帮我们重启服务，并不需要我们手动启动服务）

​		在npm官网搜索nondemon，然后按照npm命令下载。`npm install -g nodemon`

​		安装完后用`nodemon 文件名.js`启动服务。

![image-20210501141526490](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501141526490.png)

相关用法说明：https://www.npmjs.com/package/nodemon

​		这里可能会报错，尝试用管理员身份启动vscode，如果还不行就下拉打开另一个终端powershell，然后输入：set-ExecutionPolicy RemoteSigned——选择Y。然后再启动服务就行了。



### 3.11 请求超时与网络异常处理

​		如果服务端设置了延迟发送响应头，在前台设置了相应的时间接收响应数据，如果超过了这个时间就==拒绝响应或者回调相关的函数提示相关问题==

​		如果是响应错误，也要可以利用一些XMLHttpRequset实例的`error`事件进行处理，处理的网络异常。

![image-20210501155316361](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501155316361.png)

更多的网络异常处理：

![image-20210502165336134](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502165336134.png)



### 3.12 取消请求

利用XMLHttpRequest实例对象中的`abort()`来取消当前请求

![image-20210501160518559](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501160518559.png)



### 3.13 防止重复请求问题

​		在实际场景中，用户可能会频繁点击发送请求。如果服务响应比较慢而用户又疯狂去点击，这就会造成服务器压力会很大，它会接收到非常多的请求，而且是相同请求，然后频繁去处理这些请求。

​		为了解决这一问题，就要利用==节流阀开关==来控制重复发送相同请求。

示例:

![image-20210501162347463](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501162347463.png)

这样就会防止重复请求。

![image-20210501162510066](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501162510066.png)



## 四、使用Axios发送AJAX请求

axios是目前前端最热门的AJAX工具库。也是vue和react推荐的AJAX工具包。

Axios中文说明文档：https://www.kancloud.cn/yunye/axios/234845

==使用Axios发送AJAX请求前，要先安装axios工具包或者使用cdn引用来使用==

安装：

使用npm:

```
$ npm install axios
```

使用 cdn:

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```



### 4.1 get请求示例：

![image-20210501211356428](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501211356428.png)

get请求，第一个参数是`url`，第二个参数是`其它配置`。

![image-20210501212736334](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501212736334.png)

### 4.2 post请求示例：

![image-20210501212522768](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501212522768.png)

post请求，第一个参数是url，第二个参数是请求体，第三个参数是其它配置。

![image-20210501212721367](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501212721367.png)

```
axios.post(url,data,config);
```

### 4.3 通过baseURL设置url

示例：

```javascript
axios.defaults.baseURL = "http://127.0.0.1:8000";
```

这样后面的url部分直接填文件在服务器上的位置。



### 4.4 使用Axios函数发送AJAX请求

使用Axios函数（通用方法）发送AJAX请求相当于报文方式一样，很清楚很方便。

示例：

![image-20210501215024662](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501215024662.png)



## 五、使用fetch()函数发送AJAX请求

对fetch()函数的相关说明：https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch

fetch()函数用的比较少，但出现也要知道。



## 六、跨域

### 6.1 同源策略

同源策略是浏览器的一种安全策略。

同源：协议、域名、端口号必须完全相同。

发送AJAX请求默认是要遵循==同源策略==，也就是说，不满足同源策略是发不了AJAX请求的。

​		违背同源策略就是跨域。在项目中经常出现，因为单台服务器的服务是有上限的，性能是有瓶颈的，所以要加入更多的服务器来提供服务。服务器一增加就会形成跨域。



示例：

![image-20210501230459222](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210501230459222.png)



### 6.2  JSONP解决跨域

>JSONP是什么

​		JSONP（JSON with Padding），是一个==非官方的跨域解决方案==，纯碎凭借程序员的聪明才智开发出来，只支持`get请求`

​		JSONP是 json 的一种"使用模式"，可以让网页从别的域名（网站）那获取资料，即跨域读取数据。

>JSONP怎么工作的？

​		在网页有一些标签天生具有跨域能力，比如:img link iframe script。

​		JSONP就是利用script标签的跨域能力来发送请求的。

>JSONP的使用

1.动态的创建一个`script标签`

```javascript
const script=document.createElement("script");
```

2.设置script的src，设置回到函数

```javascript
script.src="http://localhost:3000/test/testAJAX?callback=abc";
```



示例：验证script标签的跨域功能

首先新建一个xx.js文件，在主html文件中用==script标签==引入该文件资源。

![image-20210502141728591](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502141728591.png)

app.js文件中的代码如下：

```javascript
const data = {
    name: "小杨先生",
};

//处理数据
//封装一个可重复执行功能的代码块
function render(data) {
    const result = document.querySelector(".box");
    result.innerHTML = data.name;
}

//执行函数
render(data);
```

在主html文件打开，在浏览器中显示结果，此时是`同源策略下的结果`

**要验证script标签的跨域能力**，我们这么做：

1. 在vscode中通过==live server==打开，

![image-20210502142049485](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502142049485.png)

​	2.在Network中打开app.js文件，然后看在浏览器上显示该文件在`HTTP`协议下发送请求。

![image-20210502142326911](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502142326911.png)

​	3.复制该地址，替换原理的scr链接，直接用本地打开，然后在==file协议下==显示的结果也一样，说明script标签能跨域发送请求

![image-20210502142600447](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502142600447.png)



示例2：

​		在服务端建立jsonp服务

![image-20210502145013826](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502145013826.png)

​		在前台利用script标签向服务端发送请求，结果返回正常。

![image-20210502145138591](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502145138591.png)



### 6.3 JSONP实践

验证用户名是否存在

![image-20210502151822598](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502151822598.png)

![image-20210502151844216](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502151844216.png)

![image-20210502151902840](C:\Users\YYY\AppData\Roaming\Typora\typora-user-images\image-20210502151902840.png)



### 6.4 设置CORS响应头实现跨域

CORS资料说明：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS

