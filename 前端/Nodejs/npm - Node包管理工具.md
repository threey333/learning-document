# npm - Node包管理工具

在掘金有一篇总结npm知识很好的文章，地址：https://juejin.cn/post/6844903870578032647。

由于npm知识很多，这里只记录常用的知识，更多的可以到npm官网和npm中文文档学习。

- [npm官网](https://npmjs.com)
- [npm中文文档](https://www.npmjs.com.cn/)

## 一、依赖包版本号

### 1.1 每个版本号的含义

`npm`采用了`semver`规范作为依赖版本管理方案。

按照`semver`的约定，一个`npm`依赖包的版本格式一般为：**主版本号.次版本号.修订号**（`x.y.z`），每个号的含义是：

- **主版本号**（也叫大版本，`major version`）

​		大版本的改动很可能是一次颠覆性的改动，也就意味着可能存在与低版本不兼容的`API`或者用法，（比如 `vue 2 -> 3`)。

- **次版本号**（也叫小版本/功能版本，`minor version`）

​		小版本的改动应当兼容同一个大版本内的`API`和用法，因此应该让开发者无感。所以我们通常只说大版本号，很少会精确到小版本		号。

- **修订号**（也叫补丁，`patch`）

​		一般用于修复`bug`或者很细微的变更，也需要保持向前兼容。

### 1.2 常见的几个版本格式

为了方便理解，这里以1作为大版本号、2作为次版本号、3作为补丁号。

常见的几个版本格式如下："1.2.3"、"^1.2.3"、"~1.2.3"、（"1.x" 、"1.X"、1.\*"、"1"、"\*"）、("1.2.3-alpha.1"、"1.2.3-beta.1"、"1.2.3-rc.1")。

#### 1.2.1 "1.2.3"

”1.2.3“：表示精确版本号。任何其他版本号都不匹配。**在一些比较重要的线上项目中，建议使用这种方式锁定版本。**



#### 1.2.2 "^1.2.3"

表示兼容补丁和小版本更新的版本号。官方的定义是“能够兼容除了最左侧的非 0 版本号之外的其他变化”(Allows changes that do not modify the left-most non-zero digit in the [major, minor, patch] tuple)。这句话很拗口，举几个例子:

```js
"^1.2.3" 等价于 ">= 1.2.3 < 2.0.0"。即只要最左侧的 "1" 不变，其他都可以改变。所以 "1.2.4", "1.3.0" 都可以兼容。

"^0.2.3" 等价于 ">= 0.2.3 < 0.3.0"。因为最左侧的是 "0"，那么只要第二位 "2" 不变，其他的都兼容，比如 "0.2.4" 和 "0.2.99"。

"^0.0.3" 等价于 ">= 0.0.3 < 0.0.4"。大版本号和小版本号都为 "0" ，所以也就等价于精确的 "0.0.3"。
```

从这几个例子可以看出，`^` 是一个兼具更新和安全的写法，但是对于大版本号为 1 和 0 的版本还是会有不同的处理机制的。



#### 1.2.3 "~1.2.3"

表示只兼容补丁更新的版本号。关于 `~` 的定义分为两部分：如果列出了小版本号（第二位），则只兼容补丁（第三位）的修改；如果没有列出小版本号，则兼容第二和第三位的修改。我们分两种情况理解一下这个定义：

```js
"~1.2.3" 列出了小版本号 "2"，因此只兼容第三位的修改，等价于 ">= 1.2.3 < 1.3.0"。

"~1.2" 也列出了小版本号 "2"，因此和上面一样兼容第三位的修改，等价于 ">= 1.2.0 < 1.3.0"。

"~1" 没有列出小版本号，可以兼容第二第三位的修改，因此等价于 ">= 1.0.0 < 2.0.0"
```

从这几个例子可以看出，`~` 是一个比`^`更加谨慎安全的写法，而且`~`并不对大版本号 0 或者 1 区别对待，所以 "~0.2.3" 与 "~1.2.3" 是相同的算法。当首位是 0 并且列出了第二位的时候，两者是等价的，例如 "~0.2.3" 和 "^0.2.3"。



#### 1.2.4 "1.x" 、"1.X"、1.\*"、"1"、"\*"

表示使用通配符的版本号。x、X、* 和 （空） 的含义相同，都表示可以匹配任何内容。具体来说：

```js
"*" 、"x" 或者 （空） 表示可以匹配任何版本。

"1.x", "1.*" 和 "1" 表示匹配主版本号为 "1" 的所有版本，因此等价于 ">= 1.0.0 < 2.0.0"。

"1.2.x", "1.2.*" 和 "1.2" 表示匹配版本号以 "1.2" 开头的所有版本，因此等价于 ">= 1.2.0 < 1.3.0"。
```



#### 1.2.5 "1.2.3-alpha.1"、"1.2.3-beta.1"、"1.2.3-rc.1"

带预发布关键词的版本号。先说说几个预发布关键词的定义：

```js
alpha(α)：预览版，或者叫内部测试版；一般不向外部发布，会有很多bug；一般只有测试人员使用。

beta(β)：测试版，或者叫公开测试版；这个阶段的版本会一直加入新的功能；在alpha版之后推出。

rc(release candidate)：最终测试版本；可能成为最终产品的候选版本，如果未出现问题则可发布成为正式版本。
```

以包开发者的角度来考虑这个问题：假设当前线上版本是 "1.2.3"，如果我作了一些改动需要发布版本 "1.2.4"，但我不想直接上线（因为使用 "~1.2.3" 或者 "^1.2.3" 的用户都会直接静默更新），这就需要使用预发布功能。因此我可能会发布 "1.2.4-alpha.1" 或者 "1.2.4-beta.1" 等等。

```js
">1.2.4-alpha.1"表示接受 "1.2.4-alpha" 版本下所有大于 1 的预发布版本。因此 "1.2.4-alpha.7" 是符合要求的，但 "1.2.4-beta.1" 和 "1.2.5-alpha.2" 都不符合。此外如果是正式版本（不带预发布关键词），只要版本号符合要求即可，不检查预发布版本号，例如 "1.2.5", "1.3.0" 都是认可的。

"~1.2.4-alpha.1" 表示 ">=1.2.4-alpha.1 < 1.3.0"。这样 "1.2.5", "1.2.4-alpha.2" 都符合条件，而 "1.2.5-alpha.1", "1.3.0" 不符合。

"^1.2.4-alpha.1" 表示 ">=1.2.4-alpha.1 < 2.0.0"。这样 "1.2.5", "1.2.4-alpha.2", "1.3.0" 都符合条件，而 "1.2.5-alpha.1", "2.0.0" 不符合。
```



## 二、常用的命令

### 2.1 初始化

一、使用`npm init`初始化一个新的项目时会提示你去填写一些项目描述信息。如果觉得填写这些信息比较麻烦的话，可以使用`-y`标记表示接受`package.json`中的一些默认值：

```js
npm init -y
```

还可以设置初始化的默认值：

```bash
npm config set init-author-name <name> -g
npm config set init-author-email <email> -g
```

上面两条指令为你的`npm`设置了默认的作者名和邮箱，当执行`npm init -y`的时候，`package.json`中的作者姓名和邮箱字段就会自动写入预设的值。



二、上面是我们对`npm init`最熟悉的认知，其实`npm init`还隐藏了一个不为人知却非常实用的功能：

> `npm init <initializer>` 通常被用于创建一个新的或者已经存在的`npm`包。

`initializer`在这里是一个名为`create-<initializer>`的`npm`包，该包将由`npx`来安装，然后执行其`package.json`中`bin`属性对应的脚本，会创建或更新`package.json`并运行一些与初始化相关的操作。

`npm init <initializer>`时转换成`npx`命令的规则为：

- `npm init foo` -> `npx create-foo`
- `npm init @usr/foo` -> `npx @usr/create-foo`
- `npm init @usr` -> `npx @usr/create`

`Vite`脚手架推荐我们使用`npm init`来初始化：

```bash
npm init vite@latest -> npx create-vite@latest
```

实际上也就是通过`npx`去下载`create-vite`最新的包。



### 2.2 下载安装模块包

#### 2.2.1 下载安装需要使用的模块包

一、我们克隆别人的项目时，是没有把该项目的依赖包也克隆下来的，这时需要我们输入 `npm install` 下载该项目的依赖包。

```bash
npm install
```

==**注意：**==

>- 执行 `npm install` 命令会把 `package.json文件` 下的 `dependencies` 和 `devDependencies` 这两个依赖下的模块包都下载。
>- 除非使用 `npm install --production` 命令，这样就只会下载 `dependencies` 依赖下的包。

二、主动下载我们需要的依赖包时，可以输入 `npm install <tarball file>`，即下载安装某某模块包文件。

```bash
npm install vue  // => 下载安装vue模块包。
npm install nodemon // => 下载安装nodemon模块包。
```

三、下载某某模块包时，还可以指定版本：`npm install <tarball file> @<version>`

```bash
npm intall vue@2.4.0  // => 下载安装vue2.4.0版本的压缩包
```

四、全局安装某某包。

```bash
npm uninstall vue-cli -g // => 全局安装vue脚手架
```



#### 2.2.2 npm install相关命令总结

- `npm i packageName -g / npm install packageName -g`：全局下载安装某某需要的包。
- `npm i packageName / npm install packageName / npm install packageName --save / npm install packageName -S `：本地下载安装某某需要的包，并且该包会添加到 `package.json文件` 下的 `dependencies`中。==注意：这四种写法功能效果都一样。==

- `npm i packageName --save-dev / npm install packageName --save-dev /npm install packageName -D`：本地下载安装某某需要的包，并且该包会添加到 `package.json文件` 下的 `devDependencies`中。
- `npm i packageName@<version>`：下载安装指定某某版本的包。
- `npm i  --production`：只会下载 `dependencies` 依赖下的包。



### 2.3 卸载模块包

当我们需要卸载某某包的使用可以使用 `npm uninstall packageName` 命令进行卸载。

#### 2.3.1 卸载全局下的包

卸载全局下的包使用的命令也是 `npm uninstall packageName`，只不过后面多了个 `-g` 的标志。

```bash
npm uninstall nodemon -g  // =>删除全局下的nodemon包
```



#### 2.3.2 卸载本地下的包

卸载本地下的包使用的命令也是 `npm uninstall packageName`命令。

```js
npm uninstall vue  // => 卸载本地下的vue模块包
```

这里要说明一下：

1. 执行 `npm uninstall packageName` 进行本地卸载模块包的时候，不用区分是 `dependencies` 下的依赖包还是 `devDependencies` 下的依赖包，因为该命令会自动识别该模块包在哪个依赖下。
2. 所以根本不需要在命令后面加 `--save` 或 `--save-dev` 进行标识删除。



### 2.4 更新模块包

使用 `npm update packageName` 命令将会把该模块包更新到该模块大版本下的最新版本。

例如本地下vue模块包的版本号为2.5.0，使用 `npm update vue` 则把会vue模块包更新到大版本2下的最新版本。

```bash
npm update vue  // => 版本号从 2.5.0 更新为了 2.6.14 因为大版本2下的最新版本就是2.6.14 再下去就是大版本3了，而2版本的模块包并不属于3的模块包。
```

又例如本地下vue模块包的版本号为3.0.0，使用 `npm update vue` 则把会vue模块包更新到大版本3下的最新版本。

```bash
npm update vue // => 版本号从 3.0.0 更新为了 3.2.29 因为大版本3下的最新版本就是 3.2.29
```



### 2.5 检查环境-npm doctor

可以通过`npm doctor`命令在我们的环境中运行多个检查。比如，检查我们当前的环境是否能够连接到`npm`服务、检查`node`和`npm`版本、检查`npm`源、检查缓存文件的权限等：

```bash
npm doctor
```

图示：

![image-20220207095134302](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220207095138.png)



### 2.6 模块管理

#### 2.6.1 查看模块包的版本信息

一、使用 `npm view/info packageName version` 可以查看某个模块包的版本信息。

```js
npm view vue version   // => 2.6.14版本
npm view jquery version // => 2.2.4版本
```

二、使用 `npm view/info packageName versions` 可以查看某个模块包的所有版本信息。

```base
npm view jquery versions

//jquery所有版本结果如下：
[
  '1.5.1',        '1.6.2',      '1.6.3',        '1.7.2',       
  '1.7.3',        '1.8.2',      '1.8.3',        '1.9.1',       
  '1.11.0-beta3', '1.11.0-rc1', '1.11.0',       '1.11.1-beta1',
  '1.11.1-rc1',   '1.11.1-rc2', '1.11.1',       '1.11.2',      
  '1.11.3',       '1.12.0',     '1.12.1',       '1.12.2',      
  '1.12.3',       '1.12.4',     '2.1.0-beta2',  '2.1.0-beta3', 
  '2.1.0-rc1',    '2.1.0',      '2.1.1-beta1',  '2.1.1-rc1',   
  '2.1.1-rc2',    '2.1.1',      '2.1.2',        '2.1.3',       
  '2.1.4',        '2.2.0',      '2.2.1',        '2.2.2',       
  '2.2.3',        '2.2.4',      '3.0.0-alpha1', '3.0.0-beta1', 
  '3.0.0-rc1',    '3.0.0',      '3.1.0',        '3.1.1',       
  '3.2.0',        '3.2.1',      '3.3.0',        '3.3.1',       
  '3.4.0',        '3.4.1',      '3.5.0',        '3.5.1',       
  '3.6.0'
]
```



#### 2.6.2 查看模块包的所有信息

如果想查看某个模块包的所有信息，可以使用 `npm view/info <packageName>` 命令。

所有信息包括：

- 依赖
- 关键字
- 更新日期
- 贡献者
- 仓库地址
- 许可证

图示：

![image-20220207100011983](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220207100013.png)



#### 2.6.3 查看当前项目中可升级的模块

使用 `npm outdated` 命令可以查看当前项目中有哪些模块可以升级的：

```bash
npm outdated
```

输入 `npm outdated` 命令后，如果当前项目中有可升级的模块，那么它会显示这么个结果：

![image-20220207101247617](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220207101250.png)

这里解释一下每个字段的含义：

- `Package`：模块包名。
- `Current`：模块包名当前的版本号。
- `Wanted`：模块包能升级的最大范围。如果没有可用的升级范围，则显示当前安装的版本。
- `Latest`：模块包最新的版本号。
- `Location`：模块包当前所处的位置。



#### 2.6.4 查看全局可升级的模块包

查看全局可升级的模块同样是使用 `npm outdated` 命令，只不过多了个 `-g` 全局标志。

```bash
npm outdated -g
```

显示结果基本跟2.7的结果一样。



#### 2.6.5 查看当前项目依赖的所有模块包

查看当前项目依赖的所有模块使用的命令是 `npm list/ls`。

```bash
npm list/ls
```



#### 2.6.6 查看当前项目依赖的某个模块包

查看当前项目依赖的某个模块包使用的命令是 `npm list/ls <packageName>`。

```bash
npm list/ls <packageName>
```



### 2.7 查看模块文档

一、打开模块的主页：

```bash
npm home <packageName> 
```

二、打开模块的代码仓库：

```bash
npm repo <packageName> 
```

三、打开模块的文档地址：

```bash
npm docs <packageName>
```

四、打开模块的 issues 地址：

```bash
npm bugs <packageName>
```



### 2.8 依赖锁定

`npm`默认安装模块时，会通过脱字符`^`来限定所安装模块的主版本号。可以配置`npm`通过波浪符`~`来限定安装模块版本号：

```bash
npm config set save-prefix="~"
```

当然还可以配置`npm`仅安装精确版本号的模块：

```bash
npm config set save-exact true
```



### 2.9 清除缓存

我们使用 `npm` 下载一些模块包的时候，会出现卡住下载不动的问题，此时如果你在终端 `Ctrl + c` 中断这次下载，那么有关该模块包的一些请求数据是会被存储下来的，其缓存地方是在：`cacache`。

此时如果你不清一下数据缓存，可能第二或者第n次都不会把该模块包下载成功，所有清除缓存有时很有必要。

```bash
npm cache clean
```

在终端输入该指令有可以有效清除缓存。



### 2.10 查看当前项目环境变量

使用 `npm run env` 可以查看当前项目环境变量。

```bash
npm run env
```

这个命令有时还是很有用的。



### 2.11 查看全局node_modules目录

使用 `npm root -g` 可以查看全局 `node_modules` 目录在哪个地方。



## 三、npm config 命令

`npm` 提供了几个 `npm config` 指令来进行用户级和全局级配置。

>参考 [npm config](https://link.juejin.cn/?target=https%3A%2F%2Fdocs.npmjs.com%2Fmisc%2Fconfig) 来获取更多的默认配置。

### 3.1 set

将配置键设置为该值。如果`key`不存在，则会新增到配置中。如果省略`value`，则`key`会被设置成`true`。

```bash
一、npm config set <key> <value> [-g|--global]

二、npm config set prefix <path>  # prefix 参数指定全局安装的根目录
# 配置 prefix 参数后，当再对包进行全局安装时，包会被安装到如下位置：
# Mac 系统：{prefix}/lib/node_modules
# Windows 系统：{prefix}/node_modules
# 把可执行文件链接到如下位置：
# Mac 系统：{prefix}/bin
# Windows 系统：{prefix}
```

例如设置npm下载源，其源默认为是：https://registry.npmjs.org/。

```bash
npm config set registry https://registry.npm.taobao.org  
# => 将npm下载源从默认的:https://registry.npmjs.org/ 设置为淘宝源: https://registry.npm.taobao.org。
```



### 3.2 get

```bash
npm config get <key>

npm config get prefix  # 获取 npm 的全局安装路径
```

按照配置优先级，获取指定配置项的值。

示例：查看当前下载源。

```bash
npm config get registry

## 结果
https://registry.npmmirror.com/
```



### 3.3 delete

```bash
npm config delete <key>
```

可以删除所有配置文件中指定的配置项。



### 3.4 list

```bash
npm config list [-l] [--json]
```

加上`-l`或者`--json`查看所有的配置项，包括默认的配置项。不加的话，不能查看默认的配置项。



### 3.5 edit

```bash
npm config edit [-g|--global]
```

在编辑器中打开配置文件。使用`-g|--global`标志编辑全局级配置和默认配置，不使用的话编辑用户级配置和默认配置。



## 四、nrm管理源

NRM (npm registry manager)是npm的镜像源管理工具，有时候国外资源太慢，使用这个就可以快速地在 npm 源间切换。

### 4.1 nrm安装

```bash
npm i nrm -g
```



### 4.2 查看nrm所有指令

```bash
nrm
```

`nrm` 所有指令如下：

```bash
#结果如下:
Usage: cli [options] [command]

Options:
  -V, --version                           输出版本号
  -h, --help                              输出使用信息

Commands:
  ls                                      列出所有的注册表
  current [options]                       显示当前注册表名称或URL
  use <registry>                          将注册表更改为注册表
  add <registry> <url> [home]             添加一个自定义注册表
  login [options] <registryName> [value]  使用base64编码的字符串或用户名和密码为自定义注册表设置授权信息
  set-hosted-repo <registry> <value>      为自定义注册表设置托管的npm存储库来发布包
  set-scope <scopeName> <value>           将范围与注册表关联
  del-scope <scopeName>                   删除一个范围
  set [options] <registryName>            设置自定义注册表属性
  rename <registryName> <newName>         设置自定义注册表名称
  del <registry>                          删除一个自定义注册表
  home <registry> [browser]               用可选的浏览器打开注册表的主页
  publish [options] [<tarball>|<folder>]  如果当前注册表是自定义注册表，则将包发布到当前注册表。
																					如果你没有使用自定义注册表，这个命令将直接运行NPM publish
  test [registry]                         显示特定或所有注册表的响应时间
  help                                    打印此帮助
  如果你想在卸载时清除NRM配置，你可以执行“npm uninstall NRM -g -C或npm uninstall NRM -g——clean”
```



### 4.3 切换下载源

使用 `nrm use <registry>` 指令可以切换下载源。

```bash
nrm use taobao  #切换淘宝源
nrm use npm     #切换成默认源
```



### 4.4 查看当前下载源

使用 `nrm current <options>` 指令可以查看当前下载源

```bash
nrm current
```



### 4.5 查看所有下载源

使用 `nrm ls` 指令可以查看所有下载源

```bash
nrm ls

#结果如下
npm ---------- https://registry.npmjs.org/
yarn --------- https://registry.yarnpkg.com/
tencent ------ https://mirrors.cloud.tencent.com/npm/
cnpm --------- https://r.cnpmjs.org/
taobao ------- https://registry.npmmirror.com/       
npmMirror ---- https://skimdb.npmjs.com/registry/ 
```



### 4.6 添加下载源

使用 `nrm add <registry> <url>` 指令可以添加新的下载源，**这个一般在公司工作的时候是添加私有源，方便拉起公司项目。**

```bash
nrm add usetaobao https://registry.npm.taobao.org/
```

结果：

![image-20220208013348538](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220208013350.png)



### 4.7 测试下载源的速度

一、使用 `nrm test [registry]` 指令可以测试某个下载源的下载速度。

```bash
nrm test usetaobao    #usetaobao - 250ms
```

二、使用 `nrm test` 指令可以测试所有下载源的下载速度。

```bash
nrm test
```



## 五、npx命令

`npx命令` 可以认为是 `npm` 的功能扩展。意思就是它能替 `npm` 解决 `npm` 不能处理的问题。

`npm` 从5.2版本开始，就增加了`npx命令` 给开发者使用。如果不能使用了，就手动安装该命令包。

```bash
npm i npx -g
```

### 5.1 方便调用项目安装的模块

#### 5.1.1 不使用npx命令

`npx` 可以解决的主要问题，就是**方便调用本地项目内部中已安装的模块包**。

举个例子，查看本地项目中已安装的 `mocha` 模块包版本。在 `npx命令` 没出之前，要想查看该模块包版本，有两种方式：

- 方式一：明确查找到该项目的 `node_module` 下面的 `.bin目录` 下的 `mocha`。（即 `./node-modules/.bin/mocha --version`）。
- 方式二：在 `scripts字段` 定义脚本，然后执行该脚本查看该模块包版本。

```bash
#方式一:
./node-modules/.bin/mocha --version

#方式二:
"自定义字段": "mocha --version"
```

方式一图示：

![image-20220208234812622](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220208234916.png)

方式二图示：

![image-20220208234847300](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220208234919.png)

![image-20220208234911683](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220208234925.png)

**==注意：如果本地项目中没有该模块包，那么使用上述的两种方式查看模块包版本是会发生报错的，而且还很麻烦。==**

#### 5.1.2 使用npx命令

而 `npx命令` 的出来，调用本地项目的模块就变得很简单。

```bash
## 使用npx查看本地项目的模块包
npx mocha --version
```

即使本地项目中没有该模块包，使用 `npx命令` 都能让你成功调用所需的模块包（除非该模块包在包管理工具中没有）。

**npx命令执行原理：**

>1. 首先 `npx命令` 会先到 `./node_modules/.bin` 目录中查找是否有该模块包，如果有则会调用。
>2. 如果在 `./node_modules/.bin` 目录中查找不到该模块包，就会到全局环境下查找该模块包，如果有则会调用。
>3. 如果还找不到，那么 `npx命令` 就会帮我们把该模块包下载到临时文件中给我们使用，使用完它会自动删除。也就是说，在你的电脑里，如果本地项目中和全局环境中都没有改模块包，那每次使用 `npx命令` 调用你想执行的模块，它都会帮我们重新下载。
>4. 就如下面5.2点说的，`npx命令` 会避免全局安装模块。



### 5.2 避免全局安装模块

当在全局和本地都没有安装你要使用的模块包，那么使用 `npx命令` 会临时帮你把模块包下载在临时文件中让你使用，使用完后它会自动帮你删除。

举个例子，全局和本地都没有vue脚手架，但你又想在不安装该脚手架的前提下使用它，有办法实现吗？

回答肯定是有的。办法就是使用 `npx命令` 临时下载该模块包使用，使用完后删除。

```bash
## 自动安装，使用完后删除，再次执行则会再次安装
npx @vue/cli create vue-project

## 等同于
npm i @vue/cli -g
vue create vue-project
```

`npx命令` 还允许指定模块包版本。

```bash
## 指定模板包版本
npx @vue/cli@4.5.10 create vue-project
```



### 5.3 npx命令的相关参数

#### 5.3.1 --no-install 参数

如果想让 `npx命令` 强制使用本地/全局的模块包，而不下载远程模块包，可以使用 `--no-install` 参数。

```bash
npx http-server --no-install
```

如不本地/全局都找不该模块包就会报错。

#### 5.3.2 --ignore-existing参数

如果忽略本地的同名模块，强制安装使用远程模块，可以使用`--ignore-existing`参数。比如，本地已经全局安装了`create-react-app`，但还是想使用远程模块，就用这个参数。

```bash
npx --ignore-existing create-react-app my-react-app
```

#### 5.3.3 -p参数

`-p  ` 参数用于指定 `npx命令` 所要安装的模块。

>指令用法：npx -p <packageName> -p <packageName> .... [command]

```bash
npx -p node@14.1.0 node -v
```

`-p` 参数对于需要安装多个模块的场景很有用。

```bash
npx -p node@14.1.0 -p http-server -p @vue/cli@4.5.10... [command]
```

#### 5.3.4 -c参数

`-c`参数可以将所有命令都用 npx 解释。有了它，下面代码就可以正常执行了。

```bash
npx -p lolcatjs -p cowsay -c 'cowsay hello | lolcatjs'
```

`-c`参数的另一个作用，是将环境变量带入所要执行的命令。举例来说，`npm` 提供当前项目的一些环境变量，可以用下面的命令查看。

```bash
npm run env | grep npm_
```

`-c`参数可以把这些 `npm` 的环境变量带入 npx 命令。

```bash
npx -c 'echo "$npm_package_name"'
```



### 5.4 使用不同版本的node

利用 `npx命令` 可以临时下载模块包使用这个特点，那么就可以指定某个版本的Node运行脚本。

```bash
npx node@14.1.0 -v
# v14.1.0
```

上面命令会使用 0.12.8 版本的 Node 执行脚本。原理是从 npm 下载这个版本的 node，使用后再删掉。

某些场景下，这个方法用来切换Node版本，要比 `nvm` 那样的版本管理器方便一些。

