# Webpack

[webpack官网](https://webpack.js.org/)

[webpack中文网](https://webpack.docschina.org/)

## 一、Webpack介绍

### 1.1 webpack是什么

**webpack** 是 JavaScript 应用程序的`静态模块打包工具`。

当webpack处理应用程序时，它会在内部构建一个依赖图，这个依赖图对应映射到项目所需要的每个模块，并生成一个或多个`bundle(依赖包)`。

![image-20210609163755550](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003118.png)

从 v4.0.0 开始，**webpack 可以不用再引入一个配置文件**来打包项目，然而，它仍然有着 [高度可配置性](https://webpack.docschina.org/configuration)，可以很好满足你的需求。

### 1.2 为什么要使用webpack

- 浏览器不识别 SASS、Less ==> 需要对Less/SASS 预编译CSS => 供浏览器使用
- 项目中的模块化以及互相之间引用依赖包造成文件分散 ==> 需要把各个分散的模块集中打包成大文件，减少HTTP的链接的请求次数 
- 打包成了大文件,体积就变大了 ==> 代码压缩 
- 部分ES6语法有兼容问题 => ES5 => 浏览器使用
- .........

> 以上这些操作以前都是需要我们手动处理,这是非常繁琐的, 这个时候webpack就可以上场了,以上的这些操作,在webpack里,只要配置好,一下就可以都搞定了

具体说明可以去官网查看`为什么选择webpack`：https://webpack.docschina.org/concepts/why-webpack/



### 1.3 Webpack的基本能力

Webpack的基本能力：`处理依赖`、`模块化`、`打包`

- **处理依赖**：方便引用第三方模块，让模块更容易复用、避免全局注入导致的冲突、、避免重复加载或者加载不必要的模块
- **合并代码**：把各个分散的模块集中打包成大文件，减少HTTP的链接的请求次数，优化代码的体积(压缩代码)
- **各种插件**：babel 把 ES6+ 转化为 ES5- 等



## 二、Webpack安装

### 2.1 本地安装

要安装 `最新版本` 或 `特定版本`，运行以下命令之一：

>最新版本：`npm install --save-dev webpack`    简写：npm install -D webpack
>
>特定版本：`npm install --save-dev webpack@<version>`     简写：npm install -D webpack@4.33.0
>
>- -D的说明：--save-dev的简写。表示项目开发期间的依赖。也就是说项目上线后，代码中就不会用到这些依赖包。（开发阶段使用）

如果还想使用webpack v4+版本，还需要安装`CLI`

> 指令:`npm install --save-dev webpack-cli`
>
> 安装指定版本命名：`npm install -D webpack-cli@<version>`

对于大多数项目，我们建议本地安装。这可以在`引入突破式变更(breaking change)版本`时，更容易分别升级项目。

通常会通过运行一个或多个 `npm scripts`在本地 `node_modules` 目录中查找安装的 webpack，来运行 webpack：

```json
"scripts": {
    "build": "webpack --config webpack.config.js"
}
```

>想要运行本地安装的 webpack，你可以通过`node_modules/.bin/webpack -v` 来访问它的版本。



### 2.2 全局安装

通过全局安装，这样全局环境下都可以使用：

>命令：`npm install --global webpack`

==注意：==

**不推荐全局安装 webpack。**这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。



## 三、Webpack四个核心概念

四个核心概念：`入口(entry)`、`出口(output)`、`加载器(loader)`、`插件(plugins)`

### 3.1 入口(entry)

**入口起点**指示webpack应该使用哪个模块来作为构建内部`依赖图`的开始。

进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

>默认值是 `./src/index.js`，但你可以通过在 [webpack configuration](https://webpack.docschina.org/configuration) 中配置 `entry` 属性，来指定一个（或多个）不同的入口起点。

例如：webpack.config.js

```js
module.extports={
  entry:'./path/to/my/entry/file.js'
}
```



### 3.2 出口(output)

**output** 属性告诉 webpack 在哪里输出它所创建的 `bundle`，以及如何命名这些文件。

主要输出文件的默认值是 `./dist/main.js`，其他生成文件默认放置在 `./dist` 文件夹中。

可以通过在配置中指定一个 `output` 字段，来配置这些处理过程：

示例：webpack.config.js

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
};
```

在上面的示例中，我们通过 `output.filename` 和 `output.path` 属性，来告诉 webpack bundle 的名称，以及我们想要 bundle 生成(emit)到哪里。可能你想要了解在代码最上面导入的 path 模块是什么，它是一个`node.js核心模块`，用于操作文件路径。

**用简单的话来说就是要打包到哪里！**



### 3.3 加载器(loader)

webpack **只能理解 JavaScript 和 JSON 文件**，这是 webpack 开箱可用的自带能力。

**loader** 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 [模块](https://webpack.docschina.org/concepts/modules)，以供应用程序使用，以及被添加到依赖图中。

**简单概括就是：==加载器让 webpack 能够去处理那些非 JavaScript 文件（webpack自身只理解 JavaScript)==**

在更高层面，在 webpack 的配置中，**loader** 有两个属性：

1. `test` 属性，识别出哪些文件会被转换。
2. `use` 属性，定义出在进行转换时，应该使用哪个 loader。

**webpack.config.js**

```javascript
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
};
```

以上配置中，对一个单独的 module 对象定义了 `rules` 属性，里面包含两个必须属性：`test` 和 `use`。

这告诉 webpack 编译器(compiler) 如下信息：

> “嘿，webpack 编译器，当你碰到「在 `require()`/`import` 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先 **use(使用)** `raw-loader` 转换一下。”



### 3.4 插件(plugins)

`加载器loader` 用于转换某些类型的模块，而插件则可以用于**执行范围更广的任务**。包括：`打包优化`，`资源管理`，`注入环境变量`。

简单地说就是：**处理加载器完成不了的功能**。

想要使用一个插件，你只需要 `require()` 它，然后把它添加到 `plugins` 数组中。

多数插件可以通过选项(option)自定义。你也可以在一个配置文件中因为不同目的而多次使用同一个插件，这时需要通过使用 `new` 操作符来创建一个插件实例。

示例：webpack.config.js

```js
const HtmlWebpackPlugin = require('html-webpack-plugin'); // 通过 npm 安装
const webpack = require('webpack'); // 用于访问内置插件

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
};
```

在上面的示例中，`html-webpack-plugin` 为应用程序生成一个 HTML 文件，并**自动注入所有生成的 bundle依赖包**。



## 四、Webpack的基本使用

### 4.1 文件命名初始化阶段

==**项目名不能有中文汉字,不能取名叫 webpack**==

步骤：

1.创建项目名称并使用命令:`npm init -y`初始化项目，之后项目目录中出现`package.json`

2.安装`webpack`和`webpack-cli`：`npm install -D webpack webpack-cli`

说明：

>`npm init -y`：在项目初始化时，默认选择yes。
>
>`webpack`：webpack工具的核心包。
>
>`webpack-cli`：提供了一些在终端中使用的命令

3.创建`main.js`入口文件。

4.在 `package.json`的`scripts`中,添加脚本

```js
 "scripts": {
    "build": "webpack main.js"
  },
// webpack 是webpack-cli 中提供的命令, 用来实现打包的
// ./main.js 入口文件,要打包哪个文件
```

> ==**注意：webpack和webpack-cli会因为版本的问题而引发报错，很坑！！！**==
>
> 解决办法：
>
> - **删除原来安装的webpack和webpack-cli，命令：`npm uninstall webpack webpack-cli`**
> - **安装项目相关的webpack和webpack-cli版本，命令：`npm install -D webpack@<version> webpack-cli@<version>`**
> - **今后做项目的时候也要注意这个问题！！！**

5.运行 : `npm run build`

成功的画面：

![image-20210609222234839](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003128.png)

但是下面还有一些橙色的字体，说了什么？

>WARNING in configuration
>The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment.
>
>翻译过后：
>
>配置中的警告
>
>**尚未设置“mode”选项，对于此值，webpack将会默认为“production”**。将“mode”选项设置为“development”或“production”，为每个环境启用默认值。

6.设置开发状态 : `mode`

```js
"build" : "webpack ./main.js --mode development"
```

>项目开发的两种环境：
>
>1. 开发环境(development)：开发过程就是开发环境
>
>2. 生产环境(production)：线上环境，也就是项目做好发布上线了。
>
>   生产环境下，打包生产的js文件都是压缩过后的，开发环境下的代码一般是不压缩的。

代码不压缩:

![image-20210609225736822](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003134.png)

代码压缩：

![image-20210609225849937](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003138.png)



### 4.2 隔行变色案例

1.创建`src/index.html`。

2.在`index.html`中添加HTML代码，比如此次的案例隔行变色：`ul>li{threey$}*10`

3.这里不采用原生js代码实施隔行变色，采用引入`jquery`。在引入之前先安装`jquery`：`npm install jquery`。

4.在没打包的`main.js`中写入隔行变色代码

```js
$('li:odd').css('backgroundColor', 'red')
$('li:even').css('backgroundColor', 'green')
```

5.在`index.html`文件中通过`<script>`标签引入jquery包，打开浏览器显示效果，成功！

![image-20210610142424077](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003141.png)

**注意：这样引入jquery包不符合模块导入的初衷，这种引入依赖包的方式不常用。**

>采用ES6模块化导入方式：
>
>- 在main.js中导入jquery包，但是会发现报错了！![image-20210610143018758](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003147.png)
>
>  翻译过来就是：不能在模块外使用 import 语句，也就是浏览器不支持ES6的方式导入模块。
>
>- 解决的方法就是使用webpack进行打包转换为浏览器能识别的方式，然后再引入打包好后的包，**这些打包好的依赖包一般都在`/dist目录下。`**
>
>![image-20210610143450575](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003144.png)
>
>结果成功：
>
>![image-20210610143530610](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003154.png)

#### 总结

**今后采用ES6模块导入方式，就需要webpack进行打包转换浏览器能识别的方式，然后再进入打包后的依赖包**



## 五、配置

webpack打包方式有两种：一种是`命令行`，另一种是`配置项`。

>- 命令行：即在`package.json文件`中的`scripts`脚本添加所需要的命令。例如：![image-20210610151138870](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003202.png)
>
>  入口文件、出口文件都在脚本命令中定义好了，在终端输入相关的命令就能运行。
>
>- 配置项：即在webpack的配置文件中添加所需要的信息，这个文件是`webpack.config.js`，可以自己在==**根目录下创建**==。注意：不要在这个文件中使用ES6的模块化导入语法。
>
>  ![image-20210610151816528](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003220.png)



### 5.1 webpack配置文件

通常，你的项目还需要继续扩展此能力，为此你可以在**项目根目录下**创建一个 `webpack.config.js` 文件，webpack 会自动使用它。

在`webpack.config.js`中可以添加你所需要做的信息。

>webpack.config.js

简单示例：

```js
const path = require("path");

module.exports = {
  //入口
  entry: path.join(__dirname, "./src/main.js"),
  output: {
    //出口目录
    path: path.join(__dirname, "./dist"),
    filename: "bundle.js",
  },
  mode: "development",
};
```

在`package.json`文件中重新添加`scripts`：![image-20210610160238090](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003235.png)



### 5.2 html-webpack-plugin插件

**html-webpack-plugin 是必备的插件。**

作用：

- 能够根据指定的模板文件 (index.html),自动生成一个新的 `index.html`,并且**自动注入到dist文件夹下**
  2. **能够自动引入出口文件**

1.安装 : `npm i html-webpack-plugin -D`         

指定安装版本：`npm i html-webpack-plugin@<version> -D`

==注意：这里安装html-webpack-plugin插件也要按照项目对应的版本，不然会报错。==

2.配置：

>第一步：引入模块
>
>```js
>const htmlWebpackPlugin= require('html-webpack-plugin')
>```
>
>第二步：配置
>
>//插件
>
>```js
>plugins:[
>  new htmlWebpackPlugin({
>    template:path.join(__dirname,'/src/index.html')
>  })
>]
>```

3.运行：`npm run build`

结果：

![image-20210610161744056](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003248.png)

**总结：**

>有`html-webpack-plugin`插件，我们只需要在==html文件没打包之前添加结构代码，无需关注依赖包的引入==，在webpack打包完后，它会**自动帮我们引入打包后的出口文件**。
>
>打包前：
>
>![image-20210610162224839](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003255.png)
>
>打包后：
>
>![image-20210610162346094](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003300.png)



### 5.3 开发服务器DevServer

`webpack-dev-server`可用于快速开发应用程序。**这个插件是我们使用webpack必备的功能插件。**

>作用：为使用webpack提供了一个服务器环境
>
>- 自动为我们项目创建一个服务器
>- 自动打开浏览器
>- 自动监视文件变化，自动刷新浏览器

使用步骤：

- 安装：`npm install -D webpack-dev-server`

- 两种使用方式：1.命令行方式、2.配置文件方式(推荐)

  方式一：命令行方式。

  >命令行脚本配置：`"dev":"webpack-dev-server"`
  >
  >运行：`npm run dev`
  >
  >成功它会在终端命令窗口出现本地服务器的地址：![image-20210610170946707](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003306.png)
  >
  >在脚本中，可以添加参数实现一些功能：
  >
  >1. `--opne`：自动打开浏览器显示，无需手动。
  >
  >  ```js
  >  "dev":"webpack-dev-server --open "
  >  ```
  >
  >2. `--host`：配置ip地址。
  >
  >  ```js
  >  "dev":"webpack-dev-server --host 127.0.0.1"
  >  ```
  >
  >3. `--port`：配置端口号。
  >
  >  ```js
  >  "dev":"webpack-dev-server --port 3001"
  >  ```
  >
  >4. `--hot`：热更新。
  >
  >   ```js
  >   "dev":"webpack-dev-server --hot"
  >   ```
  >
  >   没有热更新，只要样式一改变，整个页面都会全部刷新。
  >
  >   加了热更新，哪里样式改变，就更改哪里，不需要整体更新页面。
  >
  >
  
  方式二：配置文件中的设置
  
  >```js
  >// 配置 webpack-dev-server
  >devServer: {
  >  open: true,
  >    port: 3002,
  >},
  >```



### 5.4 打包上线问题

开发阶段：`npm run dev`：运行本地开发服务器webpack-dev-server

发布阶段：`npm run build`：webpack打包压缩，处理模块之间的依赖，然后我们再发布给后台。

**总结：在开发阶段我们只需要使用`npm run dev`来实时查看效果。**



## 六、处理非js的文件

webpack 只能处理 `js文件`,非 js(css.less.图片.字体等)处理处理不了, 需要借助 `loader 加载器`才能正常打包，否则会报错。

`loader`加载器可以协助webpack打包处理待定的文件模块，比如：

>- less-loader：可以打包处理 `.less`相关的文件
>- sass-loader：可以打包处理`.scss`相关的文件
>- url-loader：可以打包处理css中与url路径相关的文件。



### 6.1 处理.css文件

1.创建一个`css文件`，然后在`index.js`中使用ES6模块导入语法，但是你会发现报错了。

```html
错误语：You may need an appropriate loader to handle this file type.
翻译过来：您可能需要一个合适的加载器来处理这种文件类型。
```

说明webpack处理不了`非js文件`。要想webpack处理css文件，需要安装相关的加载器才行。

2.安装：`npm i -D style-loader css-loader`

3.在webpack配置文件`webpack.config.js`中，添加个新的配置项`module`

4.在 `module` 中添加 `loader` 来处理 `css`

>```js
>//所有第三方文件模块的匹配规则
>module: {
>  rules: [
>    // /\.css$/：利用正则表达式匹配相关要求的文件，比如这里它是匹配到以css结尾的文件名。
>    { test: /\.css$/, use: ['style-loader', 'css-loader'] }
>  ]
>}
>```
>
>注意点 ==use执行loader 顺序 从右往左==
>
>- css-loader: 读取css文件内容,将其转化为一个模块
>- style-loader :拿到模块, 创建一个style标签,插入页面中

以上过程都处理好后，再`npm run dev`就能成功在浏览器中显示结果：

![image-20210610182333633](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003316.png)



### 6.2 处理.less文件

1.在src文件目录下里的css文件创建一个`less文件`，然后在`index.js`入口文件中用ES6模块导入 `import './css/index.less'`

2.安装处理less文件的加载器loader：`npm install -D less-loader less`       （css-loader和less-loader前面已经安装过了）

3.在 `webpack.config.js` 中, 配置 `module->rules`

4.在 module 中, 添加 loader加载器 来处理 `less`

```js
{ test: /\.less$/, use: ["style-loader", "css-loader", "less-loader"] },
```

​	less文件中的代码

```less
body {
  margin: 0;
  padding: 0;
  ul {
    padding: 0;
    margin: 0;
  }
}
```

结果：webpack处理`.less文件`成功。

![image-20210610201700953](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003326.png)



### 6.3 处理图片文件

1.安装处理图片文件的`loader加载器`：`npm install -D url-loader file-loader`

2.在`webpack.config.js`中配置信息

```js
方式一：
{
  test: /\.(jpg|png|gif|bmp|ttf|eot|svg|woff|woff2)$/,
    use: ["url-loader?limit=大小"],
},
  方式二：
  { 
    test:/\.(jpg|png|gif|bmp|ttf|eot|svg|woff|woff2)$/,
      use:[
        {
          loader:"url-loader",
          options:{limit:大小自己设置}
        }
      ]
  }
```

这里对`limit`说明下：

>- 如果图片小于`limit`指定的大小 ===> 可以转化为 base64，这样好处是减少请求次数。
>- 如果图片大于等于`limit`指定的大小 ===> 转化为 base64 加载就耗性能了。

这里再对`url-loader`和`file-loader`这两个加载器进行说明：

- url-loader：默认会将图片转换为base64编码格式，目的是减少请求次数提高性能。

- file-loader：当文件过大时，不会转换为base64编码格式，这时内部会调用`file-loader`加载图片。示例：

  >```js
  >原来：background-image: url(../images/006Sutb1gy1gpyps2p417j303m03m744.jpg);
  >file-loader加载器加载后：background-image：url(26fbc06c527dd6e7b2366c14cbbaa41f.jpg)
  >```

#### #base64编码格式的图片说明 

- 精灵图 : 将一些小图片合并为一张图片,减少请求次数,提高性能
- 字体图标 :直接将一些小的图片,合并到字体文件中,并且不会失真
- base64 : 是一种编码格式,能够将图片、文字等常见的文件,转化为 base64 格式。这种字符串格式, 浏览器能够识别并且读取显示到页面中;
- base64 是一个字符串,也可以直接被内嵌到页面中, 或者 css 中
- ==注意 : 大图片不适合用 base64 处理, 只有小的图标才适合 base64 处理==



### 6.4 处理字体图标文件

第一步：把字体图标文件复制到`src`文件目录下。示例

![image-20210610220252863](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003343.png)

第二步：在`入口文件index.js`引入字体文件的核心css。示例：

![image-20210610220431973](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003347.png)

第三步：处理字体图标需要`url-loader`加载器，如果没安装则需要安装：`npm install -D url-loader`

第四步：在`webpack.config.js`中配置信息：(前提是没配置信息)

```js
{ test: /\.(bmp|eot|ttf|woff|woff2|svg)$/, use: ['url-loader'] }
```

第五步：在想添加字体图标的HTML标签加上`类名`或者在`css`、`less`、`scss`中给HTML标签元素加上`font-family: 'icomoon';`

示例：

![image-20210610221110656](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003354.png)

![image-20210610221314231](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003358.png)



### 6.5 处理 ES6 语法

现在的项目都是使用 ES6 开发的, 但是这些新的 ES6 语法, 并不是所有的浏览器都支持, 所以就需要有一个工具,帮我们转成 es5 语法, 这个就是: babel。

babel的官网：https://babeljs.io/

官方对Babel的解释：Babel is a JavaScript compiler. ==> babel 是一个 JavaScript 编译器。

>- webpack 只能处理 import / export 这个 es6 模块化语法而其他的 js 新语法,应该使用 babel 来处理。
>- 比如 : `var o = { ...obj }` 在谷歌上可以,edge 就不可以

babel的使用：

第一步：安装1：`npm i -D babel-core babel-loader@7`

- `babel-core`是babel的核心包
- `babel-loader`加载js文件，并将js代码内容交给 `babel-core`  解析为 `es5`低版本的js

第二步：安装2：`npm i -D babel-preset-env babel-preset-stage-2`

>- babel-preset：表示能够解析什么版本的js语法
>
>- babel-preset-env：表示能够解析es2015，es2016，es2017，es2018这些标准语法
>
>- babel-preset-stage-2：用来解析经过会遇到，但是还没有被采纳为标准的语法
>
>  ```js
>  比如，'abc'.padStart(10, '6') : 准备 10 位字符,有 abc,前面不够用 6 补充,是 es2017 的语法,
>  ```

第三步：配置：在`webpack.config.js`中添加一个loader

```js
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```

第四步：在项目根目录中创建 `babel 的配置文件`，叫：`.babelrc`

```js
{
  "presets": [
    "env",
    "stage-2"
  ],
}
```

==**结果：js高级语法在babel工具转换后，js高级语法代码就能转化为其他低版本浏览器能识别的代码，比如IE浏览器。**==

图示：

![image-20210611082952211](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003425.png)

![image-20210611082851566](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117003429.png)