# NuxtJS学习

## 一、Nuxt.js相关概念和优点

### 1.1 概念

`nuxt.js` 简单的说是Vue.js的通用框架，最常用的就是用来作为SSR（服务端渲染）。

Vue.js是开发SPA（单页面应用）的，而 `nuxt.js` 是基于Vue开发的多页应用的，并且在服务端完成渲染，可以直接用命令把我们制作的Vue项目生成静态html。

**简单说 `nuxt.js项目` ，它其实就是一个vue的项目融合一个 `node.js server` 项目。** 

这里 `node.js server服务` 有两个作用：

- 一、代替浏览器工作，笼统理解就是在 `created` 阶段请求数据和页面渲染。
- 二、当作静态文件服务器，把渲染好的页面返回给用户。



###  1.2 优点

`Nuxt.js` 优点有：

- 基于Vue。
- 自动代码分层。
- 强大的路由功能，支持异步数据。
- 静态文件服务。
- 支持`ES6+` 的语法。
- 打包和压缩 `JavaScript` 和 `css`。
- `HTML` 头部标签管理。
- 本地开发支持热更新加载。
- 集成 `ESLint`。
- 支持各种样式预编译器 `SASS`、`LESS`等等。
- 支持 `HTTP/2` 推送。



### 1.3 ssr服务端渲染大致流程图

流程图：

![image-20211121232854369](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211121232903.png)



## 二、