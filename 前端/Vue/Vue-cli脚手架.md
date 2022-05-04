# Vue-cli脚手架

## 一、脚手架安装

### 1.1 脚手架安装命令

安装命令：`npm install -g @vue/cli`  或者   `yarn global add @vue/cli`

安装是否成功，可以在终端检查：检查命令：`vue --version` 或 `vue-V`

==注意：==

>- 现在使用`npm install -g @vue/cli`安装的脚手架都是最新版的，如果想安装指定的版本可以使用如下命令：`npm install -g @vue/cli@版本号`。
>
>- 在最新的Vue脚手架如果想拉起脚手架2.x版本的模板，这时你可以全局安装一个`桥接工具`，安装完后就可以使用旧版本的`vue init` 功能。
>
>  命令：`npm install -g @vue/cli-init`。
>
>  ![image-20210611142015213](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010801.png)

**Vue脚手架全局安装目录一般都在：**

![image-20210611144119524](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010805.png)

其他全局安装的依赖包也都是放在这个文件中。



### 1.2 脚手架升级命令

如需升级全局的 Vue CLI 包，请运行：`npm update -g @vue/cli` 或者 `yarn global upgrade --latest @vue/cli`。

### 1.3 脚手架卸载命令

命令：`npm uninstall @vue/cli -g`

### 1.4 初始化Vue项目

#### #Vue Cli2初始化项目

命令：`vue init webpack 项目名称`。比如 `vue init webpack vue-demo1`。

==**注意：在最新的脚手架中初始化Cli2项目会出现问题！！！**==

问题描述：vue-cli Failed to download repo cuejs-templates/webpack:read ECONNRESET

解决办法：

>把模板（webpack-simple或者webpack）下载，然后放在初始化项目同级目录，然后用相对路径初始化Cli2项目。
>
>命令：`vue init /webpack vue-demo`



#### #Vue Cli3初始化项目

命令：`vue create 项目名称`。



## 二、VueCli2脚手架工具的使用

### 2.1 Vue Cli2项目配置选择

第一步：当我们`vue init ./webpack 项目名称`初始化Vue-cli2项目时会遇到如下配置选择：

>目前一般都按如下配置选择，有些插件在初始化项目时没有选择，后面可以自己安装。
>
>```js
>? Project name # 回车
>? Project description # 回车
>? Author  # 回车
>? Vue build standalone  # => 运行时+编译 => 详见下面的问题1 
>? Install vue-router? # Yes
>? Use ESLint to lint your code? # Yes => 详见下面的问题2
>? Pick an ESLint preset Standard  # standard
>? Set up unit tests # No
>? Setup e2e tests with Nightwatch? # No
>? Should we run `npm install` for you after the project has been created? # (recommended) npm
>```

第二步：开启本地服务器，查看项目初始化是否成功：

>进入的项目目录，使用命令`npm run dev`。
>
>成功：![image-20210612153129612](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010816.png)
>
>指令说明:
>
>`"dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js"`
>
>- --inline：指的是信息显示在哪里
>- --progress：进度条
>- 



### 2.2 项目文件结构介绍

#### #第一层文件夹目录

![image-20210612155102586](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010822.png)



#### #第二层文件目录

##### build文件目录下的结构：

==注意！！！webpack.prod.conf.js是webpack的生产环境配置==

![image-20210612155951670](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010828.png)

##### config文件目录下的结构：

![image-20210612160302900](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010833.png)

##### src文件目录下的结构（重要）：

![image-20210612160834326](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010836.png)

##### static文件目录下的结构：

![image-20210612161157215](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010842.png)

##### .editorconfig文件中的信息：

![image-20210612161612213](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010847.png)

> 如何使用该功能？
>
> - 1.安装 vscode 的 `editorConfig for vscode插件`
> - 2.eslint 检测修复后



![image-20210918223749315](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010852.png)

### 2.3 插件的安装

在前端项目中要安装这几个插件：

- `ESlint插件`：检测代码的格式，保证 `代码的一致性` 和 `防止错误`。

  >注意：初始化项目的时候，有Eslint的选项要选。还有该插件要配合`Prettier -Code formatter`插件一块使用。
  >
  >ESLint插件在vscode中的配置：
  >
  >```js
  >//Vetur插件
  >"vetur.format.defaultFormatterOptions": {
  >  "prettier": {
  >    "semi": false,
  >      "singleQuote": true,
  >        "wrap_attributes": "force-aligned",
  >  }
  >  },
  >
  >    // 从这里往下大家都给加上
  >    "eslint.validate": [
  >      "vue",
  >      "javascript",
  >      "html"
  >    ],
  >
  >"prettier.eslintIntegration": true, // #让prettier使用eslint的代码格式进行校验
  >"prettier.semi": false, //#去掉代码结尾的分号
  >"prettier.singleQuote": true, //#使用单引号替代双引号
  >"javascript.format.insertSpaceBeforeFunctionParenthesis": true,
  >"editor.formatOnType": true, //#让函数(名)和后面的括号之间加个空格
  >"vetur.format.defaultFormatter.html": "js-beautify-html",
  >"vetur.format.defaultFormatter.js": "vscode-typescript",
  >"editor.formatOnSave": true
  >```

- `Prettier -Code formatter`插件：美化代码的插件。

- `editorConfig for vscode`插件：有了该插件才能使得`.editorconfig`文件里的配置信息起作用。

  ![image-20210613174901252](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010900.png)

  ![image-20210612161612213](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20211117010905.png)

  

### 2.4 其他小问题

1.@ => src 的绝对路径

2.eslint 的忽略问题:

- `// eslint-disable-next-line` 忽略下一行的警告
- `/* eslint-disable */` 忽略文件里的所有警告 (前面)

3.关闭 eslint

- config/index.js => 25 行 => false



## 三、VueCli3脚手架工具的使用

### 3.1 VueCli3与VueCli2的区别

- `vue-cli3`是基于`webpack4`打造的，`vue-cli2`还是`webpack3`。
- `vue-cli3`的设计原则是 **”0配置“**，移除的配置文件是根目录下的`build`和`config`等目录。
- `vue-cli3`提供了==**vue ui命令**==，使得有了可视化配置选择，更加人性化。
- 移除了`static文件夹`，新增加了`public文件夹`（这个文件夹可以当作是替代static文件夹的），并且**`index.html入口页面` 移动到了`public`中**。



## 四、VueCli3以上版本文件区别说明

### 4.1 Public文件夹和assets文件夹

#### 4.1.1 Public文件夹

`Public文件夹` 也就是老版脚手架中的`static文件夹`。

任何放置在 `public文件夹` 的静态资源都会被简单的复制，而不经过 webpack编译打包 。需要通过绝对路径来引用。

**`public` 目录提供的是一个应急手段，当通过绝对路径引用时，需要留意应用会部署到哪里。如果没有部署在域名的根部，这时就需要为你的`URL`配置`publicPath前缀`:**

例如：在 `public/index.html` 或其它通过 `html-webpack-plugin` 用作模板的 HTML 文件中，需要通过 `<%= BASE_URL %>` 设置链接前缀。

>```html
><link rel="icon" href="<%= BASE_URL %>favicon.ico">   //通过<%= BASE_URL %>设置前缀。
>
><script type="text/javascript" src="<%= BASE_URL %>lib/mui/js/mui.min.js"></script>
>```



#### 4.1.2 assets文件夹

`assets` 目录，在编译过程中会被 webpack 处理，当做模块依赖，只支持相对路径的形式。一般放置可能会变动的文件。

比如：

在weibo项目中，我们在main.js中使用import方式导入`mui.min.css样式文件`，主要还是为了让Webpack帮助我们进行编译和打包，这样就可以让`postcss-px-to-viewport`插件对mui.min.css中的样式单位px进行vw转换。



### 4.2 如何使用Public文件夹

- 一般把静态资源（比如图片、图标、第三方的依赖）都可以直接放在 `public文件夹` 下,这样它就不会经过webpack编译打包处理，而是**直接复制到编译后的文件夹中。**


- 如果没有部署在域名的根部，那么这时就需要通过 `<%= BASE_URL %>` 来为你的URL配置publicPath前缀。**比如当项目文件放在view文件夹下，那么<%= BASE_URL %>就会替换成view，如果放在index文件夹下，那么<%= BASE_URL %>会替换成index。**这就是publicPath前缀。