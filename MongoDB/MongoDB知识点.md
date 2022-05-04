# MongoDB知识点

## 一、查看相关的命令

当下载完MongoDB数据库后，在 `/bin` 目录下有许多 `.exe` 可执行应用程序。这些应用程序都有自己的许多指令，我们学习它的时候，除了去官方文档进行查看外，还可以在命令行窗口输入 `xxx --help` 命令进行命令的查看。

这里我们主要查看 `mongo命令` 和 `mongod命令`。

### 1.1 查看mongo命令

`mongo` 的相关命令：

```bash
MongoDB shell version v4.4.12  
## MongoDB shell版本为 v4.4.12

usage: mongo [options] [db address] [file names (ending in .js)] 
## 用法:mongo [options] [db地址][文件名(以.js结尾)]

db address can be:
  foo                   foo database on local machine
  192.168.0.5/foo       foo database on 192.168.0.5 machine
  192.168.0.5:9999/foo  foo database on 192.168.0.5 machine on port 9999
  mongodb://192.168.0.5:9999/foo  connection string URI can also be used
## Db地址可以是:
## foo                  								Foo数据库在本地机器上
## 192.168.0.5/foo      								Foo数据库在192.168.0.5机器上
## 192.168.0.5:9999/foo 								在端口9999的192.168.0.5机器上的Foo数据库
## mongodb://192.168.0.5:9999/foo				也可以使用连接字符串URI
Options:
  --ipv6                               启用IPv6支持(默认禁用)
  --host arg                           要连接的服务器
  --port arg                           要连接的端口
  -h [ --help ]                        显示此使用信息(命令帮助)
  --version                            显示版本信息
  --verbose                            增加冗长
  --shell                              在执行文件后运行shell
  --nodb                               不要在启动时连接mongod -不需要'db address'参数
  --norc                               启动时不会运行“。mongorc.js”文件
  --quiet                              be less chatty
  --eval arg                           评估javascript
  --disableJavaScriptJIT               禁用Javascript即时编译器
  --enableJavaScriptJIT                启用Javascript即时编译器
  --disableJavaScriptProtection        允许自动JavaScript函数编组
  --retryWrites                        在短暂的网络错误时自动重试写操作
  --disableImplicitSessions            不自动创建和使用隐式会话
  --jsHeapLimitMB arg                  设置js作用域的堆大小限制
  --idleSessionTimeout arg (=0)        如果Shell会话空闲了这么多秒，那么终止它

Authentication Options:  ##身份验证选项
  -u [ --username ] arg                用户名进行身份验证
  -p [ --password ] arg                密码进行身份验证
  --authenticationDatabase arg         用户源(默认为dbname)
  --authenticationMechanism arg        身份验证机制
  --gssapiServiceName arg (=mongodb)   使用GSSAPI/Kerberos进行身份验证时使用的服务名称
  --gssapiHostName arg                 用于GSSAPI/Kerberos身份验证的远程主机名

FLE AWS Options:    ##通过AWS选项
  --awsAccessKeyId arg                 AWS Access Key for FLE Amazon KMS
  --awsSecretAccessKey arg             AWS Secret Key for FLE Amazon KMS
  --awsSessionToken arg                Optional AWS Session Token ID
  --keyVaultNamespace arg              database.collection to store encrypted
                                       FLE parameters
  --kmsURL arg                         Test parameter to override the URL for
                                       KMS

AWS IAM Options:
  --awsIamSessionToken arg             AWS Session Token for temporary
                                       credentials

TLS Options:       ##TLS选项
  --tls                                所有连接都使用TLS
  --tlsCertificateKeyFile arg          TLS协议的PEM证书/密钥文件
  --tlsCertificateKeyFilePassword arg  TLS协议的PEM文件中密钥的密码
  --tlsCAFile arg                      TLS的证书颁发机构文件
  --tlsCRLFile arg                     TLS的证书撤销列表文件
  --tlsAllowInvalidHostnames           允许连接到服务器
                                       non-matching hostnames
  --tlsAllowInvalidCertificates        允许使用无效证书的服务器连接
  --tlsFIPSMode                        启动时激活FIPS 140-2模式
  --tlsCertificateSelector arg         系统存储区中的TLS证书
  --tlsDisabledProtocols arg           使用逗号分隔的TLS协议列表禁用[TLS1_0,TLS1_1,TLS1_2]

file names: a list of files to run. files have to end in .js and will exit after unless --shell is specified
## 文件名:要运行的文件列表。文件必须以.js结尾，并且在指定了——shell之后将退出
```



### 1.2 查看mongod命令

`mongod` 的相关命令：

```bash
Options:
  --networkMessageCompressors arg (=snappy,zstd,zlib)
                                        Comma-separated list of compressors to
                                        use for network messages

General options:  ##一般选择:
  -h [ --help ]                         显示此使用信息(查看命令大全的命令)
  --version                             显示版本信息
  -f [ --config ] arg                   指定附加选项的配置文件
  --configExpand arg                    配置文件中的进程扩展指令(none, exec, rest)
  --port arg                            默认端口号为“27017”
  --ipv6                                启用IPv6支持(默认禁用)
  --listenBacklog arg (=2147483647)     设置socket监听待定项大小
  --maxConns arg (=1000000)             最大并发连接数
  --pidfilepath arg                     pidfile的完整路径(如果没有设置，则不会创建pidfile)
  --timeZoneInfo arg                    时区信息目录的完整路径，例如。/usr/share/zoneinfo
  -v [ --verbose ] [=arg(=v)]           更冗长(如-vvvvv)
  --quiet                               安静的输出
  --logpath arg                         要发送写到的日志文件，而不是标准输出——必须是一个文件，而不是目录
  --logappend                           附加到logpath而不是overwrite
  --logRotate arg                       设置日志旋转行为(重命名|重新打开)
  --timeStampFormat arg                 日志消息中所需的时间戳格式。iso8601-utc或iso8601-local
  --setParameter arg                    设置可配置参数
  --bind_ip arg                         默认情况下，在本地主机上监听的ip地址列表，以逗号分隔
  --bind_ip_all                         绑定所有ip地址
  --noauth                              没有安全运行
  --transitionToAuth                    用于滚转访问控制升级。尝试通过传出的连接进行身份验证，无论成功与否都要继续进																					行。接受传入的连接，无论是否进行身份验证。
  --slowms arg (=100)                   配置文件和控制台日志的slow值
  --slowOpSampleRate arg (=1)           要包括在概要文件和控制台日志中的慢操作的部分
  --profileFilter arg                   查询谓词以控制记录和分析哪些操作
  --auth                                运行与安全
  --clusterIpSourceWhitelist arg        允许“__system”访问的网络CIDR规范
  --profile arg                         0=off 1=slow, 2=all
  --cpu                                 定期显示cpu和iowait利用率
  --sysinfo                             打印部分诊断系统信息
  --noscripting                         禁用脚本引擎
  --notablescan                         不允许表扫描
  --keyFile arg                         集群鉴权的私钥
  --clusterAuthMode arg                 集群鉴权的认证方式。替代(密钥文件| sendKeyFile | sendX509 | x509)

Replication options:  ##复制选项:
  --oplogSize arg                       用于复制操作日志的大小(以MB为单位)。默认为磁盘空间的5%(也就是说，越大越好)

Replica set options:
  --replSet arg                         arg is <setname>[/<optionalseedhostlist
                                        >]
  --enableMajorityReadConcern [=arg(=1)] (=1)   使多数readConcern

Sharding options:    ##分片选项:
  --configsvr                           声明这是一个集群的配置数据库;默认端口27019;默认目录 /data/configdb
  --shardsvr                            声明这是一个集群的shard db;默认端口27018

Storage options:     ##存储选项:
  --storageEngine arg                   使用什么存储引擎-如果没有数据文件，默认为wiredTiger
  --dbpath arg                          数据文件目录-默认为\data\db\，根据当前工作驱动器，默认为C: data\db\
  --directoryperdb                      每个数据库将存储在一个单独的目录中
  --syncdelay arg (=60)                 磁盘同步之间的秒数
  --journalCommitInterval arg (=100)    分组/批量提交的频率(ms)
  --upgrade                             如果需要，升级数据库
  --repair                              在所有dbs上运行repair
  --journal                             启用日志记录
  --nojournal                           禁用日志记录(日志记录在默认情况下为64位)
  --oplogMinRetentionHours arg (=0)     要保存在oplog中的最小小时数。默认值是0(关闭)。分数是允许的(如1.5小时)

Free Monitoring Options:   ##免费监控选项:
  --enableFreeMonitoring arg            启用免费云监控(|运行时|关闭)
  --freeMonitoringTag arg               免费云监控标签

TLS Options:     ##TLS选项:
  --tlsOnNormalPorts                    在配置的端口上使用TLS
  --tlsMode arg                         设置TLS运行模式(禁用|allowTLS|preferTLS|requireTLS)
  --tlsCertificateKeyFile arg           TLS的证书和密钥文件
  --tlsCertificateKeyFilePassword arg   TLS证书密钥文件中的密钥解锁密码
  --tlsClusterFile arg                  内部TLS认证的密钥文件
  --tlsClusterPassword arg              内部认证密钥文件密码
  --tlsCAFile arg                       TLS的证书颁发机构文件
  --tlsClusterCAFile arg                CA用于在入站连接期间验证远程连接
  --tlsCRLFile arg                      TLS的证书撤销列表文件
  --tlsDisabledProtocols arg            使用逗号分隔的TLS协议列表禁用[TLS1_0,TLS1_1,TLS1_2]
  --tlsAllowConnectionsWithoutCertificates
                                        允许客户端连接而不提供证书
  --tlsAllowInvalidHostnames            允许服务器证书提供不匹配的主机名
  --tlsAllowInvalidCertificates         允许使用无效证书的服务器连接
  --tlsFIPSMode                         启动时激活FIPS 140-2模式
  --tlsCertificateSelector arg          系统存储区中的TLS证书
  --tlsClusterCertificateSelector arg   SSL/TLS系统存储区的证书，用于内部TLS认证
  --tlsLogVersions arg                  使用逗号分隔的TLS协议列表登录连接[TLS1_0,TLS1_1,TLS1_2]

AWS IAM Options:
  --awsIamSessionToken arg              AWS Session Token for temporary
                                        credentials

WiredTiger options:   ##WiredTiger选项:
  --wiredTigerCacheSizeGB arg           分配给高速缓存的最大内存量;默认为物理RAM的1/2
  --wiredTigerJournalCompressor arg (=snappy)
                                        使用压缩器记录日志[none|snappy|zlib|zstd]
  --wiredTigerDirectoryForIndexes       将索引和数据放在不同的目录中
  --wiredTigerCollectionBlockCompressor arg (=snappy)
                                        采集数据的块压缩算法[none|snappy|zlib|zstd]
  --wiredTigerIndexPrefixCompression arg (=1)
                                        在行存储叶页上使用前缀压缩

Windows Service Control Manager options:   ##Windows服务控制管理器选项:
  --install                             安装Windows服务
  --remove                              删除Windows服务
  --reinstall                           重新安装Windows服务(相当于——remove后面跟着——install)
  --serviceName arg                     Windows服务名称
  --serviceDisplayName arg              Windows服务显示名称
  --serviceDescription arg              Windows服务描述
  --serviceUser arg                     服务执行帐号
  --servicePassword arg                 用于验证serviceUser的密码
```





## 二、MongoDB服务开启与连接

### 2.1 设置系统环境变量

将 `MongoDB文件夹`下的 `bin文件夹目录` 设置到环境变量后，我们在命令行窗口就能更方便操作MongoDB。

![image-20220214215248087](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220219003635.png)

设置后就不用进入到相关 `bin目录` 中执行相关命令：比如查看MongoDB安装是否成功。

```bash
mongo
```

如果不设置系统环境变量的话，那么要想执行相关 `MongoDB命令` 就只能先进入到 `MongoDB` 文件夹中的 `bin` 目录中执行。

例如 MongoDB文件夹在d:\software_tool：

```bash
D:\software_tool\MongoDB\bin> mongo
```



### 2.2 开启MongoDB数据库服务

开启 `MongoDB数据库服务` 的步骤：

- 一、先创建一个空数据目录。（比如在D盘根目录中创建了一个 `MongoDB_data\db目录` ）。
- 二、在命令行窗口中使用 `mongod --dbpath 要当作数据库的目录文件` 命令初始化该数据目录。

操作完上面两个步骤后，`MongoDB_data\db目录` 下就会有数据，此时这个目录就是一个数据库服务。

![image-20220214224741398](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220214230235.png)

同时在命令行窗口中会出现如下结果，说明初始化数据目录成功。**这样该目录就是一个MongoDB数据库服务。**

![image-20220214225910612](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220214230253.png)

**==注意：==**

>- 新版本MongoDB数据库的服务器已经不需要我们自己手动开始服务了，因为MongoDB数据库服务器已设置为自动开启。（这个功能是在我们安装的时候选择的，这样它就帮我们创建一个服务配置文件）**相关服务配置文件如下：**
>
>![image-20220216002337972](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220219003626.png)
>
>**==注意：这个配置文件是可以根据我们的情况进行修改。==**
>
>- 当MongoDB数据库服务打开以后，不要关闭服务进程，不然后面是连接不上MongoDB数据库服务的。



### 2.3 连接MongoDB数据库服务

将 `MongoDB文件夹` 下 `bin文件目录` 设置到环境变量后，我们可以在命令行窗口中的任何目录直接连接 `MongoDB数据库` ，执行命令如下：

```bash
# 命令行窗口
mongo 
```



## 三、MongoDB的核心概念

### 3.1 数据库、集合和文档的基本概念

在 `MongoDB` 中核心概念是数据库、集合和文档。

**一、库：**

> **MongoDB中的库就类似于传统关系型数据库中库的概念，通过不同的库隔离不同应用数据。**

`MongoDB` 中可以建立多个数据库，每一个库都有自己的 `集合` 和 `权限`，不同的数据库也放置在不同的文件中。默认的数据库为 `test`，数据库存储在启动指定的 `data目录` 中。

**二、集合：**

> **集合就是MongoDB文档组，类似于RDBMS（关系数据库管理系统：Relational Database Management System）中的表的概念。**

集合存在于数据库中，一个数据库可以创建多个集合，每个集合没有固定的结构，这意味着你在对集合可以插入不同格式类型的数据，但通常情况下我们插入集合的数据都会有一定的关联性。

**三、文档：**

集合中的一条条数据就是文档，它是一组 `键值对(key-value)`。`MongoDB` 的文档不需要设置相同的字段，并且相同的字段不需要相同的数据类型，这与关系型数据库有很大的区别，也是 `MongoDB` 非常突出的特点。

一个简单的文档例子如下：

```json
{
  "name":"threey",
  "age":"18"
}
贾帆,马燕,王节,等. 应用Web技术的图书管理系统[J]. 重庆理工大学学报（自然科学版）,2013,27(8):76-79. DOI:10.3969/j.issn.1674-8425(z).2013.08.016.
```

**四、MongoDB数据库、集合和文档之间的关系：**

![image-20220216215516160](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220219003619.png)



### 3.2 关系型数据库和MongoDB数据库关系对比

| 关系型数据库术语 | MongoDB数据库术语 | 说明                                |
| :--------------- | :---------------- | :---------------------------------- |
| database         | database          | 数据库                              |
| table            | collection        | 数据库表/集合                       |
| row              | document          | 数据记录行/文档                     |
| column           | field             | 数据字段/域                         |
| index            | index             | 索引                                |
| table joins      |                   | 表连接,MongoDB不支持                |
| primary key      | primary key       | 主键,MongoDB自动将_id字段设置为主键 |



## 四、库的相关操作

### 4.1 显示数据库

#### 4.1.1 显示所有数据

显示所有数据库的命令为：`show dbs | show databases`。

**==注意：==**

>**当输入显示所有数据库命令时，MongoDB会对没有数据的库是不显示的。**

当输入完这个命令之后，命令行窗口会显示有三个数据库。它们分别为：`admin`、`config`、`local`。

对这三个数据库的解释：

- `admin`：从权限的角度来看，这是“root”数据库。要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限。一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器。
- `local`：这个数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合。
- `config`：当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息。

**==注意：==**

> - `admin`、`config`、`local`这三个数据库，我们不建议操作它，也不建议使用它。
> -  `local` 这个数据库，如果你有需求，是可以使用的，另外两个则是不建议。

![image-20220216222752279](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20220219003615.png)



#### 4.1.2 显示当前所在数据库

显示当前所在哪个数据库可以使用 `db` 这个命令。

**==注意：==**

>- 当我们第一次输入这个命令的时，显示的是 `test` 这个数据。原因是MongoDB中默认连接进来的是 `test` 这个数据库。
>-  一开始 `test` 这个数据库没有数据，而MongoDB对没有数据的库是不显示的。
>- 所以当你输入 `db` 这个命令的时候显示的 `test` 这个数据库，而输入 `show dbs` 命令的时候则没有显示有 `test` 数据库，就是没有数据的原因。



### 4.2 创建/切换数据库

`MongoDB` 创建数据库的命令如下：

```markdown
use DATABASE_NAME
```

**==注意：如果数据库不存在，则是创建数据库，否则是切换指定的数据库。==**

示例：创建一个名为 `books` 的数据库。

```bash
## 创建一个名为books的数据库。
> use books   
switched to db books
## 显示当前所在的数据库
> db
books
## 显示当前存在的所有数据库
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
>
```

要想显示刚创建的 `数据库books`，需要向它插入一些数据。

```bash
> db.user.insert({"name":"threey","age":"18",gender:"man"})
WriteResult({ "nInserted" : 1 })  ## 插入成功
```



### 4.3 删除数据库

`MongoDB` 删除数据库的命令如下：

```markdown
db.dropDatabase()
```

删除当前数据库，默认为 `test` 数据库，你可以使用 `db` 命令查看当前数据库名。

示例：

```bash
## 显示当前所在哪个数据库。
> db
books

## 删除当前所在的数据库。
> db.dropDatabase()
{ "dropped" : "books", "ok" : 1 }

## 删除完后查看当前存在的所有数据库
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
```



## 五、集合的相关操作

### 5.1 查看集合

`MongoDB` 查看当前数据库集合的命令如下：

```markdown
show collections | show tables
```

示例：

```bash
## 1.创建一个books数据库
> use books 
switched to db books

## 2.查看当前所在哪个数据库
> db 
books

## 3.向books数据库中的user集合插入数据
> db.user.insert({name:'threey',age:18,sex:'boy'})
WriteResult({ "nInserted" : 1 })
> db.foo.insert({like:'watch TV',food:'fish'})
WriteResult({ "nInserted" : 1 })

## 4.查看当前数据库的所有集合。
> show collections
foo
user
```

> ==注意==：如果在命令行窗口输入 `show collections` 命令没有结果显示出来，说明当前所在的数据库没有一个集合。



### 5.2 创建集合

在 `MongoDB` 中如果想创建集合，可以使用 `createCollection()方法` 来创建。

```markdown
db.createCollection(name, options)
```

参数说明：

- `name`：要创建的集合名称。
- `options`：可选参数，指定有关内存大小及索引的选项。

| 字段   | 类型    | 描述                                                         |
| :----- | :------ | :----------------------------------------------------------- |
| capped | boolean | （可选）如果为 true，则创建固定集合。固定集合是指有着固定大小的集合，当达到最大值时，它会自动覆盖最早的文档。 **当该值为 true 时，必须指定 size 参数。** |
| size   | number  | （可选）为固定集合指定一个最大值，即字节数。 **如果 capped 为 true，也需要指定该字段。** |
| max    | number  | （可选）指定固定集合中包含文档的最大数量。                   |

在插入文档时，MongoDB首先检查固定集合的 `size` 字段，然后检查 `max` 字段。

示例一：创建集合。

```bash
## 1.创建集合
> db.createCollection("test")
{ "ok" : 1 }

## 2.查看创建后的集合
> show collections
foo
test
user
>
```

示例二：创建一个集合，该集合空间大小为6142800B，文档最大数量为1000个。

```bash
## 1.创建一个空间大小为6142800B，文档最大数量为1000的集合。
> db.createCollection("mycol",{capped:true,size:6142800,max:10000})
{ "ok" : 1 }

## 2.查看所有集合。
> show collections
foo
mycol
test
user
```



### 5.3 删除集合

在 `MongoDB` 中，如果想删除集合，可以使用 `drop()方法`。

```markdown
db.集合名称.drop()
```

**返回值：**如果成功删除集合，则 `drop()方法` 返回 true，否则返回 false。

>注意：如果删除的是不存在的集合，则直接返回 false。

示例：删除当前数据库中的某个集合。

```bash
## 1.显示当前数据库中的所有集合
> show collections
mycol
test
user

## 2.删除mycol集合
> db.mycol.drop()
true
>
```



## 六、文档的相关操作

文档的数据结果跟 `JSON` 基本一样。

所有存储在集合中的数据都是 `BSON` 格式。而 `BSON` 格式是一种类似 `JSON` 的二进制形式的存储格式（ Binary JSON 的简称）。

### 6.1 插入文档

在 `MongoDB` 中插入文档有如下三个方法：

- `db.集合名称.insert()`：如果该方法插入成功后，会返回一个包含操作状态的对象；如果插入的数据中主键 `_id` 已经存在，则会报错。更多具体信息下文再介绍。
- `db.集合名称.insertOne()`：
- `db.集合名称.insertMary()`：

#### 6.1.1 insert()

`db.集合名称.insert()` 将一个或多个文档插入到集合中。

插入语法如下：

```markdown
db.集合名称.insert(
	<文档或文档数组>,
	{
		writeConcern: <document>, // 写入策略
    ordered: <boolean> // 写入顺序
	}
)
```

参数具体信息：

| 参数           | 类型           | 描述                                               |
| :------------- | :------------- | :------------------------------------------------- |
| `document`     | 文档或文档数组 | 要插入到集合中的文档或文档数组。                   |
| `writeConcern` | 文档           | 写入策略，默认为 1，即要求确认写操作，0 是不要求。 |
| `ordered`      | 布尔值         | 指定是否按顺序写入，默认 true，按顺序写入。        |

>注意：
>
>- 如果向不存在的集合插入数据，则会创建该集合。具体查看示例二。
>- 插入文档数据时，我们可以自己指定主键 `_id` 的值；但注意避免重复问题发生。具体看示例四
>
>- 如果插入的数据主键 `_id` 已经存在，则会抛出异常。异常会提示该主键 `_id` 已存在，不会保存当前插入的数据。具体查看示例五。

示例一：向已存在的集合插入文档数据。

```bash
## 向user集合插入数据并且手动添加主键_id。
> db.user.insert({_id:'620faf5956fbe1bca548524a',name:'yyy'})
WriteResult({ "nInserted" : 1 })

## nInserted字段显示成功插入的文档数。
```

示例二：向不存在的集合插入文档数据。

```bash
## 向不存在的school集合插入文档数据
> db.school.insert({students:100,level:3})
WriteResult({ "nInserted" : 1 })

## 结果就是会创建该集合
```

示例三：插入文档数组数据。

```bash
## 向user集合插入文档数组数据
> db.user.insert([{id:002,name:'Tony',age:18},{id:003,name:'Lina',age:16}])
BulkWriteResult({
  "writeErrors" : [ ],
  "writeConcernErrors" : [ ],
  "nInserted" : 2,
  "nUpserted" : 0,
  "nMatched" : 0,
  "nModified" : 0,
  "nRemoved" : 0,
  "upserted" : [ ]
})

## nInserted字段显示成功插入的文档数。
```

示例四：插入文档数据时，设置指定的主键 `_id`。

```bash
## 插入文档数据时，设置指定的主键_id
> db.school.insert({_id:1,students:200,level:2})
WriteResult({ "nInserted" : 1 })
```

示例五：插入数据时如果与集合中的某个文档的主键 `_id` 相同，则会抛出异常。

```bash
## 插入数据时如果存在重复的主键 `_id` 则会抛出异常。
> db.user.insert({_id:'620faf5956fbe1bca548524a',name:'yyssy'})
WriteResult({
  "nInserted" : 0,
  "writeError" : {
  "code" : 11000,
  "errmsg" : "E11000 duplicate key error collection: books.user index: _id_ dup key: { _id: \"620faf5956fbe1bca548524a\" }"
  }
})
```



#### 6.1.2 insertOne()

`db.集合名称.insertOne()` 将单个文档数据插入到集合中。

插入语法如下：

```markdown
db.collection.insertOne(
   单条文档,
   {
      writeConcern: <document> // 写入策略
   }
)
```

参数具体信息：

| 参数           | 类型     | 描述                                               |
| :------------- | :------- | :------------------------------------------------- |
| `document`     | document | 要插入到集合中的文档。                             |
| `writeConcern` | document | 写入策略，默认为 1，即要求确认写操作，0 是不要求。 |

**返回值：**

- `acknowledged`：为true则是可写，为false则不是可写。
- `insertedId`：成功插入文档的主键 `_id值`。

>注意：
>
>- 向某个集合插入文档数据时，如果该集合不存在，则会创建该集合。（示例一）
>- 插入文档数据时，我们可以自己指定主键 `_id` 的值；但注意避免重复问题发生。
>- 如果插入的数据主键 `_id` 已经存在，则会抛出异常。异常会提示该主键 `_id` 已存在，不会保存当前插入的数据。
>- 该方法只能插入单个文档数据。

示例一：向不存在的集合插入文档数据。

```bash
## 向不存在的集合插入文档数据
> db.food.insertOne({item:'apple',number:100})
{
  "acknowledged" : true,
  "insertedId" : ObjectId("620fc3e956fbe1bca548524f")
}
```

示例二：向存在的集合插入文档数据。

```bash
## 向集合school插入文档数据
> db.school.insertOne({item:"card",qty:15})
{
  "acknowledged" : true,
  "insertedId" : ObjectId("620fc1cf56fbe1bca548524e")
}
```

示例三：添加文档数据时，添加第二个参数。

```bash
> db.foo.insertOne({item:'banana',number:1000},{w:"majority",wtimeout:100})
{
  "acknowledged" : true,
  "insertedId" : ObjectId("620fc5b256fbe1bca5485250")
}
```



#### 6.1.3 insertMary()

`db.集合名称.insertMary()` 将多个文档插入到集合中。

插入如下：

```markdown
db.集合名称.insertMany ( 
   [  < document  1 >  ,  < document  2 > ,  ...  ], 
   { 
      writeConcern :  < document > , // 写入策略
      ordered :  < boolean >  // 插入顺序
   } 
)
```

参数具体信息：

| 参数           | 类型     | 描述                                               |
| :------------- | :------- | :------------------------------------------------- |
| `document`     | 文档数组 | 要插入到集合中的文档数组。                         |
| `writeConcern` | 文档     | 写入策略，默认为 1，即要求确认写操作，0 是不要求。 |
| `ordered`      | 布尔值   | 指定是否按顺序写入，默认 true，按顺序写入。        |

**返回值：**

- `acknowledged`：为true则是可写，为false则不是可写。
- `insertedIds`：一个insertedIds数组，包含每个成功插入的文档的主键 `_id值`。

>注意：
>
>- 该方法只能插入文档数组数据。
>
>- `insertMany()`与 `db.collection.explain()` 不兼容，这时就使用 `insert()方法`。
>- 插入多个文档数据时，如果该集合不存在，则在成功插入入时创建该集合。
>- 如果插入的文档数据主键 `_id` 已经存在，则会抛出异常。异常会提示该主键 `_id` 已存在，不会保存当前插入的数据。

示例一：向存在的集合插入文档数组数据。

```bash
## 由于MongoDB数据库服务器终端支持shell脚本，所以可以使用try...catch捕获
try {
   db.products.insertMany( [
      { item: "card", qty: 15 },
      { item: "envelope", qty: 20 },
      { item: "stamps" , qty: 30 }
   ] );
} catch (e) {
   print (e);
}

## 返回值:
{
   "acknowledged" : true,
   "insertedIds" : [
      ObjectId("562a94d381cb9f1cd6eb0e1a"),
      ObjectId("562a94d381cb9f1cd6eb0e1b"),
      ObjectId("562a94d381cb9f1cd6eb0e1c")
   ]
}
```

示例二：插入文档数组数据时，可以指定主键 `_id值`。

```sql
-- 指定主键_id值。
try {
   db.products.insertMany( [
      { _id: 10, item: "large box", qty: 20 },
      { _id: 11, item: "small box", qty: 55 },
      { _id: 12, item: "medium box", qty: 30 }
   ] );
} catch (e) {
   print (e);
}

-- 返回值
{ "acknowledged" : true, "insertedIds" : [ 10, 11, 12 ] }
```



### 6.2 查询文档

#### 6.2.1 find()

在 `MongoDB` 中查询文档使用 `db.集合名称.find()` 方法就足够了。

查询语法如下：

```markdown
db.集合名称.find(query,projection)
```

如果需要以易读的方式来读取数据，可以使用 `pretty()` 方法。 

语法如下：

```markdown
db.集合名称.find(query,projection).pretty()
```

>注意：`pretty()` 方法以格式化的方式来显示所查询到文档数据。

参数具体信息：

| 参数         | 类型 | 描述                                                         |
| :----------- | :--- | :----------------------------------------------------------- |
| `query`      | 文档 | （可选的）使用查询操作符指定查询条件。若要返回集合中的所有文档，请忽略此参数或传递 `空文档({})`，如db.集合名称.find({})。 |
| `projection` | 文档 | （可选的）使用 `投影操作符` 指定返回的值。查询时返回文档中的所有键值，只需省略该参数即可（默认省略）。 |

**返回值：**返回根据查询条件匹配到文档数据。

示例一：查询books集合中的所有文档数据。

```sql
-- 查询当前集合所有文档数据
> db.books.find({})      
{ "_id" : 1, "name" : "编程书籍1", "number" : 100, "update" : 2001 }  
{ "_id" : 2, "name" : "编程书籍2", "number" : 100, "update" : 2001 }  
{ "_id" : 3, "name" : "编程书籍3", "number" : 100, "update" : 2001 }  
{ "_id" : 4, "name" : "编程书籍4", "number" : 100, "update" : 2001 }  
{ "_id" : 5, "name" : "编程书籍5", "number" : 100, "update" : 2001 }  
{ "_id" : 6, "name" : "编程书籍6", "number" : 100, "update" : 2001 }  
{ "_id" : 7, "name" : "编程书籍7", "number" : 100, "update" : 2001 }  
{ "_id" : 8, "name" : "编程书籍8", "number" : 100, "update" : 2001 }  
{ "_id" : 9, "name" : "编程书籍9", "number" : 100, "update" : 2001 }  
{ "_id" : 10, "name" : "编程书籍10", "number" : 100, "update" : 2001 }
```



#### 6.2.2 MongoDB中的查询条件

##### 6.2.2.1 和关系型数据库的where语句比较

如果你熟悉常规的 `SQL` 数据，通过下表可以更好的理解 `MongoDB` 的条件语句查询：

| 操作       | 格式                     | 示例                                          | RDBMS中的类似语句               |
| :--------- | :----------------------- | :-------------------------------------------- | :------------------------------ |
| 等于       | `{<key>:<value>`}        | ` db.books.find({name:"JavaScript权威指南"})` | `where name=JavaScript权威指南` |
| 小于       | `{<key>:{$lt:<value>}}`  | `db.books.find({number:{$lt:100}})`           | `where number < 100`            |
| 小于或等于 | `{<key>:{$lte:<value>}}` | `db.books.find({number:{$lte:99}}) `          | `where number <= 99`            |
| 大于       | `{<key>:{$gt:<value>}}`  | `db.books.find({number:{$gt:100}})`           | `where number > 100`            |
| 大于或等于 | `{<key>:{$gte:<value>}}` | `db.books.find({number:{$gte:100}})`          | `where number >= 100`           |
| 不等于     | `{<key>:{$ne:<value>}}`  | `db.books.find({number:{$ne:100}})`           | `where number != 100`           |



##### 6.2.2.2 MongoDB的AND条件

`find()` 方法的第一个查询条件参数 `query` 可以传入多个键(key)，每个键(key)以逗号隔开，相当于 `SQL` 的 `AND` 条件（即与条件）。

使用格式：

```bash
db.集合名称.find({key1:value1, key2:value2})
```

示例一：查询主键 `_id` 大于 10 同时 `name` 是 TypeScript 的数据。

```sql
> db.books.find({_id:{$gt:10},name:"TypeScript"})
{ "_id" : 11, "name" : "TypeScript", "number" : 101, "update" : "2021-5-30" }
```

示例二：查询 `number` 是 100 同时 `name` 是 编程书籍8的书籍。

```sql
> db.books.find({number:100,name:"编程书籍8"})
{ "_id" : 8, "name" : "编程书籍8", "number" : 100, "update" : 2001 }
```



##### 6.2.2.3 MongoDB的OR条件

在 `MongoDB` 中，要实现 `OR` 条件，可以使用关键字 `$or`。

使用格式如下：

```bash
>db.集合名称.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
)
```

>注意：使用$or进行 `or条件` 查询时，数组里面的key可以是相同也可以是不相同。

示例一：（key相同）使用 `OR` 条件查询 `name` 是 JavaScript权威指南 或 `name` 是 TypeScript 的文档数据。 

```sql
> db.books.find({$or:[{name:"JavaScript权威指南"},{name:"TypeScript"}]})
{ "_id" : 100, "name" : "JavaScript权威指南", "number" : 6, "update" : "2022-2-18" }
{ "_id" : 11, "name" : "TypeScript", "number" : 101, "update" : "2021-5-30" }     
```

示例二：（key不相同）查询 `name` 是 Vue.js进阶 或 `number` 是 99 的文档数据。

```sql
> db.books.find({$or:[{name:"Vue.js进阶"},{number:99}]})
{ "_id" : "25", "name" : "Vue.js进阶", "number" : 56, "update" : "2022-1-10" }   
{ "_id" : 49, "name" : "编程与类型系统", "number" : 99, "update" : "2021-5-16" } 
{ "_id" : 13, "name" : "数据结构与算法", "number" : 99, "update" : "2021-12-31" }
```



##### 6.2.2.4 MongoDB的AND条件和OR条件联合使用

`AND` 和 `OR` 可以联合一起使用。

```sql
> db.books.find({update:{$gt:"2021-1-1"},$or:[{name:"Vite"},{name:"编程与类型系统"}]})
{ "_id" : 49, "name" : "编程与类型系统", "number" : 99, "update" : "2021-5-16" }      
{ "_id" : 50, "name" : "Vite", "number" : 99, "update" : "2021-2-1" }
```

 

#### 6.2.3 查询结果进行排序

当查询文档数据的时候，我们可以对查询结果的数据进行我们想要的排序方式显示。

```sql
db.集合名称.find({}).sort({key1,value1,...})
-- 其中value值为1升序，等于-1降序。
```

示例一：根据 age 进行查询结果排序。

```bash
> db.student.find({}).sort({age:1})                
{ "_id" : 2, "name" : "小红", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }
{ "_id" : 1, "name" : "小明", "age" : 18, "sex" : "男", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }
```

示例二：根据 age 和 address 进行查询结果排序。

```bash
> db.student.find({}).sort({age:1,address:-1})                        
{ "_id" : 2, "name" : "小红", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 1, "name" : "小明", "age" : 18, "sex" : "男", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
```

从上面的结果知道，它是先根据 age 对查询结果进行排序后，再根据 address 对相关 age 的进行排序。



#### 6.2.4 查询结果进行分页

对查询结果进行分页，可以使用 `skip()` 和 `limit()` 这两个方法实现。

`skip()` 控制这查询结果的起始位置点。我们不使用它的时候，搜索结果都是默认从0位置开始显示。

```bash
db.集合名称.find({...})skip(<number>).limit(<number>)
```

示例：

```sql
> db.student.find({}).sort({age:1,address:-1}).skip(0).limit(2)
{ "_id" : 2, "name" : "小红", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
```



#### 6.2.5 查看当前集合的文档总数

查看当前集合的文档数据数量或者是查看查询结果的总数，可以使用 `count()` 实现。

```bash
db.集合名称.count() | db.集合名称.find({...]}).count()
```

示例一：查看当前集合的文档数据数量。

```sql
> db.student.count()
6
```

示例二：对查询结果进行汇总。

```sql
-- 条件查询
> db.student.find({sex:'男'})
{ "_id" : 1, "name" : "小明", "age" : 18, "sex" : "男", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }        
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }

-- 计算结果总数
> db.student.find({sex:'男'}).count()
4
```



#### 6.2.6 对查询结果指定返回字段

对查询结果指定返回字段可以配合 `find()` 的第二个参数实现；第二个参数不使用时，默认返回所有字段。

>注意：
>
>- 字段的值只有1和0，两者不能同时使用。
>- 1是会返回字段，0不会返回字段。
>- 指定返回字段后，没有指定的就不会返回显示。

示例：

```sql
-- 1.没有指定返回字段，默认是返回所有查询到的结果字段。
> db.student.find({sex:'男'})
{ "_id" : 1, "name" : "小明", "age" : 18, "sex" : "男", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }

-- 2.指定返回字段。
> db.student.find({sex:"男"},{likes:1,age:1})
{ "_id" : 1, "age" : 18, "likes" : [ "看电视", "跑步", "看书" ] }
{ "_id" : 3, "age" : 16, "likes" : [ "睡懒觉", "游泳" ] }
{ "_id" : 5, "age" : 18, "likes" : [ "看书", "跑步", "玩游戏" ] }
{ "_id" : 6, "age" : 18, "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ] }
```



#### 6.2.7 模糊查询

在 `MongoDB` 中进行模糊查询，可以通过正在表达式实现。

**==只要符合JavaScript的正在表达式都可以进行模糊查询。==**

示例：

```sql
> db.student.find({name:/小./}) 
{ "_id" : 1, "age" : 14, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ], "birthday" : "3-25", "name" : "小新" }   
{ "_id" : 2, "name" : "小红", "age" : 20, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 14, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南", "birthday" : "3-25" }
{ "_id" : 6, "name" : "小斌", "age" : 14, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏", "birthday" : "3-25" }
```



### 6.3 更新文档

修改集合中现有文档的数据，可以使用 `update()`、`updateOne()`、`updateMarry()` 等方法。

- `update()`：即可以更新一个文档数据，也可以更新多个文档数据；是 `updateOne()` 和 `updateMarry()` 的综合方法。
- `updateOne()`：更新一个文档数据的方法，部分功能跟 `update()` 差不多。
- `updateMarry()`：更新多个文档数据的方法，部分功能跟 `update()` 差不多。

#### 6.3.1 update()

`update()` 方法可以多种方式更新文档数据，采用哪种方式取决于更新参数。

- 更新现有文档数据。
- 更新文档中特定字段的数据。
- 完全替换现有文档中的数据。

`update()` 语法格式如下：

```markdown
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>,
     collation: <document>,
     arrayFilters: [ <filterdocument1>, ... ],
     hint:  <document|string>        // Available starting in MongoDB 4.2
   }
)
```

>默认情况下，`update()` 方法只更新单个文档。

参数具体信息：

| 参数         | 类型                 | 描述                                                         |
| :----------- | :------------------- | :----------------------------------------------------------- |
| query        | document             | 指定的要查询的文档数据内容                                   |
| update       | document or pipeline | 将查询到文档数据内容进行更新                                 |
| upsert       | boolean              | 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew；true为插入，默认是false，不插入。 |
| multi        | boolean              | 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。 |
| writeConcern | document             | 可选，抛出异常的级别。                                       |
| collation    | document             | 可选，指定排序规则。                                         |
| arrayFilters | array                | 可选，用于确定要为某个数组字段进行修改某个数组元素。         |



示例一：对符合条件的文档数据全部更新成新的文档内容，==相当于先删除原来文档数据再更新==。

```sql
-- 1.原来的文档的数据。
> db.student.find({})
{ "_id" : 1, "name" : "小杨", "birthday" : "1-20" }
{ "_id" : 2, "name" : "小红", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }        
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }

-- 2.进行更新替换后
> db.student.update({name:'小杨'},{age:17,sex:'男',likes:['看书','写字','唱歌','跑步']})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

-- 3.结果
> db.student.find({})
{ "_id" : 1, "age" : 17, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ] }  // => 修改的部分
{ "_id" : 2, "name" : "小红", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 5, "name" : "强强", "age" : 18, "sex" : "男", "likes" : [ "看书", "跑步", "玩游戏" ], "address" : "广西" }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }
```



示例二：查询某个条件的文档数据并对符合条件的某个字段数据进行更新（该更新只会更新符合条件的第一个）。

```sql
-- 1.查询某个条件的文档数据并对符合条件的某个字段数据进行更新
> db.student.update({age:15},{$set:{age:20}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

-- 2.结果：
> db.student.find({})
{ "_id" : 1, "age" : 17, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ] }
{ "_id" : 2, "name" : "小红", "age" : 20, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东" }
{ "_id" : 3, "name" : "小牛", "age" : 16, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南" }        
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 5, "age" : 20 }
{ "_id" : 6, "name" : "小斌", "age" : 18, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏" }
```

通过 `$set` 字段，可以保留原来的数据对某个字段进行更新。



示例三：查询某个条件的文档数据并更新所有符合条件的文档某些字段数据（保留原来的数据进行更新）

```sql
-- 1.将名字是小xx的生日该为6-1:
> db.student.update({name:/小./},{$set:{birthday:"6-1"}},{multi:true})
WriteResult({ "nMatched" : 4, "nUpserted" : 0, "nModified" : 4 })  

-- 2.结果:
> db.student.find()                                                  )
{ "_id" : 1, "age" : 14, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ], "birthday" : "6-1", "name" : "小新" }
{ "_id" : 2, "name" : "小红", "age" : 20, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东", "birthday" : "6-1" }
{ "_id" : 3, "name" : "小牛", "age" : 14, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南", "birthday" : "6-1" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 5, "age" : 20 }
{ "_id" : 6, "name" : "小斌", "age" : 14, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏", "birthday" : "6-1" }
```

要更新所有符合条件的文档某些字段数据，要设置 `multi:true` 。



示例四：查询某个条件的文档数据并更新所有符合条件的文档某些字段数据，如果没有条件符合时则插入要更改的数据。

```sql
-- 某个条件查询不到结果，那么插入要更新的数据。
> db.student.update({age:21},{$set:{address:"天津"}},{multi:true,upsert:true})))
WriteResult({
        "nMatched" : 0,        
        "nUpserted" : 1,
        "nModified" : 0,
        "_id" : ObjectId("62111e1649e83c26c63bc83f")
})

-- 结果: 
> db.student.find()
{ "_id" : 1, "age" : 14, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ], "birthday" : "6-1", "name" : "小新" }
{ "_id" : 2, "name" : "小红", "age" : 20, "sex" : "女", "likes" : [ "看电视", "跑步", "看书" ], "address" : "广东", "birthday" : "6-1" }
{ "_id" : 3, "name" : "小牛", "age" : 14, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南", "birthday" : "6-1" }
{ "_id" : 4, "name" : "曼曼", "age" : 15, "sex" : "女", "likes" : [ "看电视", "跑步", "数学" ], "address" : "北京" }
{ "_id" : 5, "age" : 20 }
{ "_id" : 6, "name" : "小斌", "age" : 14, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏", "birthday" : "6-1" }
{ "
```

设置了 `upsert:true`，当查询不到符合条件的数据时，那么就会将要更新的文档数据插入到集合中。

**总结：**

>- 没有设置 `$set` 字段进行更新文档数据，是会先删除原来的数据再进行更新。
>- 当设置了 `$set` 字段进行更新文档数据，则会保留原来的数据进行更新。
>- 当第三个参数设置了某些key，则会有不同的效果。比如 `multi:true` 会将所有符合条件的文档数据进行更新、`upsert:true` 当没有符合条件的文档数据时，把要更新的数据直接插入到该集合中。



### 6.4 删除文档

删除集合中某个文档数据，可以使用 `remove()` 方法。

语法格式如下：

```bash
db.集合名称.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>,
     collation: <document>
   }
)
```

参数具体信息：

| 参数           | 类型     | 描述                                                         |
| :------------- | :------- | :----------------------------------------------------------- |
| `query`        | document | 指定删除文档的查询条件。如果要删除集合中的所有文档，请传递一个空文档({})。 |
| `justOne`      | boolean  | （可选）如果设为 true 或 1，则只删除一个文档，如果不设置该参数，或使用默认值 false，则删除所有符合条件的文档数据。 |
| `writeConcern` | document | 可选，抛出异常的级别。                                       |
| `collation`    | document | 可选，指定排序规则。                                         |

>建议：在执行 `remove()` 方法前先执行 `find()` 方法来判断执行的条件是否正确

示例一：删除某个文档数据。

```sql
-- 1.删除之前先执行find()查看执行条件是否正确
> dn.student.remove({age:20,_id:5})
uncaught exception: ReferenceError: dn is not defined :
@(shell):1:1

-- 2.删除某个文档数据
> db.student.remove({age:20,_id:5})
WriteResult({ "nRemoved" : 1 })    
```

示例二：由于 `remove()` 方法默认是删除所有符合条件的文档数据，如果你只想删除符合条件的一条文档数据，可以通过设置第二个参数的 `justOne` 为 `true` 就能实现。

```sql
-- 1.删除之前先执行find()查看执行条件是否正确
> db.student.find({age:14})
{ "_id" : 1, "age" : 14, "sex" : "男", "likes" : [ "看书", "写字", "唱歌", "跑步" ], "birthday" : "6-1", "name" : "小新" }
{ "_id" : 3, "name" : "小牛", "age" : 14, "sex" : "男", "likes" : [ "睡懒觉", "游泳" ], "address" : "湖南", "birthday" : "6-1" }
{ "_id" : 6, "name" : "小斌", "age" : 14, "sex" : "男", "likes" : [ "吃零食", "看书", "玩游戏", "听歌" ], "address" : "西藏", "birthday" : "6-1" }

-- 2.删除符合条件的文档数据（只删除一个文档数据）
> db.student.remove({age:14},{justOne:true})
WriteResult({ "nRemoved" : 1 })
```

