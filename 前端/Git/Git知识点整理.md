# Git知识点整理

## 一、认识Git

### 1.1 Git一般只添加数据

- 执行Git操作，几乎都是往Git仓库**添加数据**。
- 我们使用Git想删除Git仓库中的数据是很难的，换句话就是**Git几乎不会执行任何有关导致文件不可恢复的操作**。

### 1.2 Git的三种状态

Git的三种状态：`已提交 (committed)`、`已修改  (modified)`、`已暂存（staged）`

- `已提交` 表示数据已经安全地保存在 `本地仓库` 中。

- `已修改` 表示修改了文件，但还没保存到 `暂存区` 中。
- `已暂存` 表示对一个已修改文件的当前版本做了标记，使之在提交到本地仓库过程中有个记录。。

### 1.3 Git项目的三个阶段

Git 项目拥有三个阶段：`工作区`、`暂存区 ` 以及 `本地仓库`。

### 1.4 总结

- 如果本地仓库中保存着特定版本的文件，就属于 **已提交** 状态。 

- 如果文件已修改并放入暂存区，就属于 **已暂存** 状态。 

- 如果作了修改但还没有放到暂存区域，就是 **已修改** 状态。



## 二、Git的工作区域和流程

### 2.1 Git的工作区域

- 工作区：平时开发代码的地方，是当前看到的最新内容，同时开发的过程中也是对工作区的操作。
- 暂存区：当执行 `git add` 的命令后，工作区的文件就会被移入暂存区，暂存区标记了当前工作区中那些内容是被 Git 管理的，当完成某个需求或者功能后需要提交代码，第一步就是通过 `git add` 先提交到暂存区。
- 本地仓库：位于自己的电脑上，通过 `git commit` 将暂存区的内容提交到本地仓库中，同时会进入本地仓库。
- 远程仓库：托管代码的远程服务器，例如(gitee、github)。本地仓库修改完代码后通过 `git push` 命令同步代码到远程仓库。



### 2.2 Git的基本工作流程

`Git` 的基本工作流程如下：

- 一、在 `工作区` 修改文件。
- 二、将修改后的文件放到 `暂存区` 中。
- 三、将 `暂存区` 中的文件提交到 `本地仓库` 中。
- 四、将本地仓库的修改推送到 `远程仓库` 中。



### 2.3 Git的工作区域和流程示意图

![image-20210915152957983](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210915164300.png)



## 三、使用Git之前需要配置信息

### 3.1 全局配置Git的用户名和邮箱

```bash
git config --global user.name "xxx"
git config --global user.email "xxx@xx.com"
```

成功后，系统盘中会有这么个 `.gitconfig文件` 记录着该配置信息：

![image-20210915165724436](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210915165731.png)

### 3.2 config 的三个作用域

```bash
$git config --local    //local只对某个仓库有效。
$git config --global   //global对当前用户所有仓库有效。
$git config --system   //system对系统所有登录的用户有效。（这个不常用）
```

==**注意：**==

>当直接输入 `git config` 时，相当于 `git config --global`。



### 3.3 检查配置信息

如果想要检查自己的配置，可以使用 `git config --list` 命令来列出所有的Git当时能找到的配置。 

```bash
$ git config --list
user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...
```

也可以通过以下命令查看所有的配置以及它们所在的位置：

```bash
 git config --list --show-origin
```



### 3.4 查看当前目录下的所有文件

使用 `ls -al` 命令可以显示当前目录下的所有文件及文件夹包括`隐藏的.` 和 `..` 等的详细信息

示例：

![image-20210915182234697](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210915182243.png)

可以发现，`ls -al` 命令可以列出包括隐藏的所有文件信息。



## 四、创建Git仓库

### 4.1 创建Git仓库方式

有两种创建Git仓库的方式：

- 方式一：没有进行版本控制的本地目录初始化为 Git 仓库。
- 方式二、从其它服务器 clone 一个已存在的 Git 仓库。

> 这两种方式都会在你的本地机器上得到一个工作就绪的 Git 仓库。

### 4.2 方式一

进入到某个文件夹然后打开终端，在终端中输入 `git init `命令初始化一个仓库。 

```bash
$ git init
```

如果在一个已有文件的文件夹当中初始化一个 Git 仓库，此时要做的就是要对这些文件进行追踪并将其放入暂存区中，然后提交到本地仓库中。

```bash
$ git add *.xxx
$ git add LICENSE
$ git commit -m 'initial project version'
```

解释：`git add *.xxx`的意思将后缀名为 `xxx` 放入到暂存区中。如下所示：

![image-20210915200925133](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210915200946.png)



### 4.3 方式二

当你在开源社区中克隆别人的项目时，它自动会有一个 Git 仓库。

```bash
$ git clone https://github.com/libgit2/libgit2
```



## 五、Git的基础操作

### 5.1 提交与修改

#### 5.1.1 git add

`git add` 添加文件到暂存区。

```bash
#添加某个文件到暂存区
git add xxxx

#添加当前目录和所有子目录中所有已修改或未跟踪的文件到暂存区
git add .

#添加某种后缀名文件到暂存区
git add *.xxxx
```

#### 5.1.2 git status

可以用 `git status` 命令查看哪些文件处于什么状态。

```bash
# 查看文件处于什么状态
git status
```

如果是克隆仓库后立即使用该命令，会看到如下输出：

```bash
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean

##说明当前工作目录很干净，不需要追踪文件。
```

使用 `git status -s` 命令或 `git status --short` 命令，此时将会得到一种格式更为紧凑的输出。

```js
$ git status -s
 M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```

#### 5.1.3 git commit

`git commit ` 将暂存区中的文件提交到本地仓库中。

```bash
# 提交暂存的更改，会新开编辑器进行编辑
git commit 

# 提交暂存的更改，并记录下备注
git commit -m "you message"

# 等同于 git add . && git commit -m
git commit -am ’xxxx‘

# 对最近一次的提交的信息进行修改,此操作会修改commit的hash值
git commit --amend
```

给 `git commit` 加上 `-a 选项`，Git 就会自动把**所有已经跟踪过的文件**暂存起来一并提交，从而跳过 `git add` 步骤：

```bash
$ git status
On branch master
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
modified:   "source/\350\265\204\346\226\231.txt"

no changes added to commit (use "git add" and/or "git commit -a")

##使用git commit -am '提交更新过的txt文件'
$ git commit -am '提交更新过的txt文件'
[master 7806155] 提交更新过的txt文件
1 file changed, 1 insertion(+)

```

#### 5.1.4 git diff

一、`git diff` 可以更具体地知道暂存区中的哪些文件进行修改后没有进行暂存的。

**==注意：==**

>**此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。**

对已经提交的readme.md进行修改，然后用 `git diff` 命令进行查看。 

```bash
$ git diff
diff --git a/readme.md/readme.md b/readme.md/readme.md
index e69de29..36fee5a 100644
--- a/readme.md/readme.md
+++ b/readme.md/readme.md
@@ -0,0 +1,3 @@
+修改了这个文件了！！！
+
+还好还好。
\ No newline at end of file

#+修改了这个文件了！！！
#+
#+还好还好。
##上面这三行是对该文件添加了新内容，使用git diff可以具体地知道该文件更新的部分。
```

二、`git diff --cached` 命令比较的是**暂存区和HEAD**所含文件的差异。

例如：修改temp分支上的hello.txt文件，`-号`为修改前的地方，`+号`为修改后的地方。

```bash
$ git diff --cached
diff --git a/hello.txt b/hello.txt
index 9f7a10e..3b18e51 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-阿大撒啊
\ No newline at end of file
+hello world
```

**注意：**

>- 更新完文件内容后，需要 `git add filename`命令将文件放入暂存区中，不然无法使用 `git diff --cached` 查看文件差异。
>- `git diff --cached` 命令比较的是暂存区和HEAD所含文件的差异。



#### 5.1.5 git rm 移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。

`git rm` 命令可以实现这个功能，即删除工作区中的文件。

```bash
#使用git rm 命令删除暂存区中的文件
$ git rm 新建文本文档.txt
rm '新建文本文档.txt'

#查看删除暂存区文件后的状态，发现是绿色，说明下次提交文件到本地仓库时可以忽略这个文件的版本管理。
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        deleted:    "\346\226\260\345\273\272\346\226\207\346\234\254\346\226\207\346\241\243.txt"

```

删除缓存区中的文件后，下次提交到本地仓库时，该文件就不会进入本地仓库。

**==注意：==**

>如果要删除之前修改过的文件或已经放到暂存区中的文件，则**必须使用强制删除选项 `-f` ，因为这是一个安全特性。**



#### 5.1.6 git reset

一、 `git reset` 命令使得放入到暂存区中的文件回退到工作区，也就是取消暂存。

```bash
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
  renamed: README.md -> README
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working
directory)
  modified: CONTRIBUTING.md
```

二、`git reset --hard` 命令可以用消除最近的几次提交commit。

```bash
# 清除temp分支最近的commit提交
$ git reset --hard 68d240c
HEAD is now at 68d240c add world.txt
```

结果示意图：

![image-20210918153101041](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210918153254.png)

![image-20210918153237146](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210918153259.png)

**注意：**

>`git reset --hard` 命令很危险，你必须要保证你清除的最近几次提交是彻底没用的，不然它会造成文件的丢失。



### 5.2 Git对文件进行重命名

`Git` 对文件进行重命名可以这么做：`git mv 原文件名称 新文件名称`

```bash
#对文件重命名
$ git mv readme.md readme
$ git mv s.txt hello.txt

#查看状态
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        renamed:    "\346\226\260\345\273\272\346\226\207\346\234\254\346\226\207\346\241\243.txt" -> hello.txt
        renamed:    readme.md/readme.md -> readme/readme.md
```

其实，运行 `git mv` 就相当于运行了下面三条命令：

```bash
$ mv README.md README
$ git rm README.md
$ git add README
```

**注意：**

>对文件进行重命名，该文件必须是被跟踪的文件(即放在暂存区的文件)，不然会重命名失败。



### 5.3 查看提交历史记录

在提交了若干更新，又或者克隆了某个项目之后，如果想查看下提交历史记录，这时可以使用 `git log `命令进行查看。

#### 5.3.1 git log

`git log` 命令可以有效的查看提交历史记录。

```bash
#git log命令使用演示
$ git log
commit 171cc38223a6ab85e936337feb310ad8cb3812c1 (HEAD -> master)
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:48:55 2021 +0800

    修改了部分文件的名称

commit fd025e563525a4344a09f01897d12d079a88c52d
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:27:41 2021 +0800

    提交新更新

commit 78061554adc0181122bb46b3067d7d0198a5bd8d
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:12:42 2021 +0800

    提交更新过的txt文件

commit b31cce389cca958b7e6ce665a7cd2eabe84ca828
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:09:56 2021 +0800

    提交剩余的文件

commit cd761e72f6efbc913537e1081b200e3cbb9b5451
Author: threey33 <229498183@qq.com>
```



#### 5.3.2 git log --oneline

`git log --oneline` 命令可以简洁地显示提交历史记录。

```bash
#git log --oneline
$ git log --oneline
171cc38 (HEAD -> master) 修改了部分文件的名称
fd025e5 提交新更新
7806155 提交更新过的txt文件
b31cce3 提交剩余的文件
cd761e7 提交又修改的README.md
604ddd3 提交新的README
803da1b 修改了readme文件名
8f85fe6 add css-file
999088f add readme.md
4569ad2 add index.html
```



#### 5.3.3 git log -n[count]

使用 `git log -n[count]` 命令可以查看最近几次提交的历史记录。n后面的括号就是次数。

例如查看最近3次的提交历史记录，可以使用 `git log -n3`命令。

```bash
$ git log -n3
commit 171cc38223a6ab85e936337feb310ad8cb3812c1 (HEAD -> master)
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:48:55 2021 +0800

    修改了部分文件的名称

commit fd025e563525a4344a09f01897d12d079a88c52d
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:27:41 2021 +0800

    提交新更新

commit 78061554adc0181122bb46b3067d7d0198a5bd8d
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:12:42 2021 +0800

    提交更新过的txt文件
```



#### 5.3.4 git log --oneline -n[count]

`git log --oneline -n[count]` 命令组合使用，可以简约地查看最近几次提交的历史记录。

```bash
#git log --oneline -n[count]命令组合使用
$ git log --oneline -n4
171cc38 (HEAD -> master) 修改了部分文件的名称
fd025e5 提交新更新
7806155 提交更新过的txt文件
b31cce3 提交剩余的文件
```



#### 5.3.5 git log --all

`git log` 命令只能查看当前分支的提交历史 ，而 `git log --all` 命令可以查看所有分支的提交历史记录。

```bash
#git log --all查看所有分支的命令
$ git log --all
commit a73f378df9b19755c7893abea75535cd539b8291 (HEAD -> temp)
Author: threey33 <229498183@qq.com>
Date:   Fri Sep 17 16:30:49 2021 +0800

    修改了hello.txt

commit 68d240c6c6ad6b785e1d47146531d0ba22dc0761
Author: threey33 <229498183@qq.com>
Date:   Fri Sep 17 16:25:25 2021 +0800

    add world.txt

commit 171cc38223a6ab85e936337feb310ad8cb3812c1 (master)
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:48:55 2021 +0800

    修改了部分文件的名称

commit fd025e563525a4344a09f01897d12d079a88c52d
Author: threey33 <229498183@qq.com>
Date:   Wed Sep 15 22:27:41 2021 +0800
```



#### 5.3.6 git log --graph

使用 `git log --graph` 命令以图形的方式显示提交的历史记录。

示例：输入 `git log --oneline --all -n3 --graph`。

![image-20210917164019010](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210917164033.png)



#### 5.3.7 提交历史记录常用命令组合使用

**示例一：**使用 `git log --oneline --all --graph` 命令，以图形化、简约的方式显示所有分支的提交历史记录。

```bash
$ git log --oneline --all --graph
* a73f378 (HEAD -> temp) 修改了hello.txt
* 68d240c add world.txt
* 171cc38 (master) 修改了部分文件的名称
* fd025e5 提交新更新
* 7806155 提交更新过的txt文件
* b31cce3 提交剩余的文件
* cd761e7 提交又修改的README.md
* 604ddd3 提交新的README
* 803da1b 修改了readme文件名
* 8f85fe6 add css-file
* 999088f add readme.md
* 4569ad2 add index.html
```

**示例二：**使用 `git log --oneline --all --graph -n4` 命令，以图形化、简约的方式显示所有分支最近4次的提交历史记录。

```bash
$ git log --oneline --all --graph -n4
* a73f378 (HEAD -> temp) 修改了hello.txt
* 68d240c add world.txt
* 171cc38 (master) 修改了部分文件的名称
* fd025e5 提交新更新
```



### 5.4 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。这时可以使用 `git commit --amend` 来撤销这次提交操作，也就是我们可以有一次重新提交到本地仓库的机会。

```bash
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

最终提交的历史记录只有一个，也就是第二次 `git commit --amend`提交的记录。



## 六、Git分支

### 6.1 创建分支

#### 6.1.1 使用git branch创建分支

Git 想创建新的分支，需要使用 `git branch` 命令：

```bash
$ git branch 分支名
```

输入这个命令后，它会在**当前所在的提交对象上创建一个指针**。

![image-20210917180447717](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210917180450.png)

那 Git 又是怎么知道当前所在哪个分支的呢？其实是有一个名位 `HEAD` 的特殊指针。

>`HEAD`特殊指针会指向当前所在的分支上。



#### 6.1.2 查看各分支当前所指的对象

**`--decorate`参数可以查看分支当前所指的对象。**

一、可以使用 `git log --oneline --all --decorate` 命令查看各分支当前所指的对象：

```bash
$ git log --oneline --decorate --all
742e49b (HEAD -> testing, master) add book.txt
a73f378 (temp) 修改了hello.txt
68d240c add world.txt
171cc38 修改了部分文件的名称
fd025e5 提交新更新
7806155 提交更新过的txt文件
b31cce3 提交剩余的文件
cd761e7 提交又修改的README.md
604ddd3 提交新的README
803da1b 修改了readme文件名
8f85fe6 add css-file
999088f add readme.md
4569ad2 add index.html
```

从上面可以发现，`master` 和 `testing` 分支当前都指向 `742e49b `开头的提交对象。

二、如果想以图形化的方式查看各分支当前所指的对象，可以使用 `git log --oneline --all -decorate --graph` 命令：

```bash
$ git log --oneline --decorate --all --graph
* 742e49b (HEAD -> testing, master) add book.txt
| * a73f378 (temp) 修改了hello.txt
| * 68d240c add world.txt
|/
* 171cc38 修改了部分文件的名称
* fd025e5 提交新更新
* 7806155 提交更新过的txt文件
* b31cce3 提交剩余的文件
* cd761e7 提交又修改的README.md
* 604ddd3 提交新的README
* 803da1b 修改了readme文件名
* 8f85fe6 add css-file
* 999088f add readme.md
* 4569ad2 add index.html
```

 

#### 6.1.3 切换分支

切换到一个已存在的分支，可以使用 `git checkout` 命令。

比如现在我们切换到 `temp`分支上：

```bash
#第一步：可以先查看当前所处在哪个分支上
$ git branch -v
  master  742e49b add book.txt
  temp    a73f378 修改了hello.txt
* testing 742e49b add book.txt

#第二步：使用git branch temp命令切换到temp分支上
$ git checkout temp
Switched to branch 'temp'
```

 这样 `HEAD` 就指向 `temp   ` 分支上。



#### 6.14 创建并立即切换到新分支

有时我们希望创建新分支，同时切换过去。这时可以使用 `git checkout -b 新分支名` 一条命令就搞定。

```bash
#创建并立即切换到新分支当中：
$ git checkout -b home
Switched to a new branch 'home'

#检查
$ git branch -v
* home    a73f378 修改了hello.txt
  master  742e49b add book.txt
  temp    a73f378 修改了hello.txt
  testing 742e49b add book.txt 
```

 其实 `git checkout -b 新分支名` 等同于如下简写：

```bash
$ git branch home
$ git checkout home
```

 

### 6.2 查看分支

一、 `git branch -v` 命令可以查看当前处在哪个分支。

```bash
$ git branch -v
  master 5631404 revise book1.txt
* temp   d290b2c revise hello.txt
```

当前处在temp分支上。

二、`git branch -av  ` 命令可以查看包括远程仓库中的分支（前提是链接了远程仓库）

```bash
$ git branch -av
  beauty              ded4fd2 add css&images
* main                fa085fc add gits.md
  test                fa085fc add gits.md
  remotes/origin/main df83e1a Initial commit
```



### 6.3 分支重命名

分支重命名可以使用 `git branch -m newBranchName` 命令。

示例：

```bash
#为当前分支改名
$ git branch -M main

#为指定分支改名
$ git branch -m master main
```

 

### 6.4 删除不需要的分支

删除不需要的分支可以使用 `git branch -d/-D 分支名` 命令。

>`-d` 和 `-D` 参数的区别：
>
>- `-d`：安全删除本地某分支。
>- `-D`：强行删除本地某分支。 

```bash
#有时使用 git branch -d 分支名 不能删除不需要的分支
$ git branch -d home
error: The branch 'home' is not fully merged.
If you are sure you want to delete it, run 'git branch -D home'.

#这时就需要带有 -D 的参数进行分支删除。
$ git branch -D home
Deleted branch home (was a73f378).
```



### 6.5 合并分支

#### 6.5.1 基本使用

使用 `git merge 分支名` 命令可以合并分支。

命令使用示例：

```bash
#假设你不是在master主分支上进行项目开发，而是在另一个分支上进行。
#当项目完成后需要将该分支上开发的代码合并到主分支上，操作如下：
#步骤一：切换到主分支上
$ git checkout master
Switched to branch 'master'

#步骤二：将test分支合并到master主分支上
$ git merge test
Updating ded4fd2..fa085fc
Fast-forward
 README.txt | 1 +
 gitss.md   | 0
 2 files changed, 1 insertion(+)
 create mode 100644 gitss.md
```

**注意：**

>- 要想把新分支上的内容合并到主分支master上，首先要切换到主分支上。
>- 然后再将该新分支合并到主分支。



#### 6.5.2 合并分支理解





## 七、本地仓库推到远程仓库中

### 7.1 链接远程仓库

一、链接远程仓库命令：

>- `git remote add origin url地址`
>- 其中origin默认是远程仓库的别名，url**可以使用https或者ssh的方式新建。**

下面的命令就是链接远程仓库，方便后面将本地仓库的文件推送到远程仓库中。

>$ git remote add origin git@github.com:threey333/git_learning_source.git

二、当执行完上面的命令后，使用 `git remote -v` 命令可以列出每一个 `远程仓库` 和 `对应的URL地址`。

示例：

```bash
$ git remote -v
origin  git@github.com:threey333/git_learning_source.git (fetch)
origin  git@github.com:threey333/git_learning_source.git (push)
```



### 7.2 远程仓库重命名

远端仓库重命名可以使用 `git remote rename oldname newname` 命令。

```bash
$ git remote rename oldname newname
```

 

### 7.3 将本地仓库推送到远程仓库

一、`git push origin 本地某个分支名` 命令可以将你本地仓库的某个分支推送到远程仓库中。

```bash
$ git push origin main
```

二、`git push` 命令带 `-u` 参数的话，说明是**设置推送的默认值**。那么以后再进行将本地某分支推送到远程仓库中，后面就不用带参数。

示例：

```bash
#git push命令带有-u参数，即是在设置默认值，这里默认值是远程仓库origin,同时是默认推送当前分支到远程仓库中。
$ git push -u origin main
```

三、`git push origin --all` 命令是将本地所有分支推送到远程仓库中。

```bash
$ git push origin --all
Total 0 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:threey333/git_learning_source.git
 * [new branch]      beauty -> beauty
 * [new branch]      test -> test
```

四、`git push origin <本地分支名>:<远程分支名>` 命令是将本地当前分支推送到远程指定分支上。

五、如果直接输入 `git push` 命令，则是将本地当前分支推送到与本地当前分支同名的远程分支上。**==注意：需要先关联远程分支。==**



### 7.4 将本地仓库推送到远程仓库遇到的问题

**问题一：**

```bash
$ git push -u origin main
To github.com:threey333/git_learning_source.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:threey333/git_learning_source.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

从报错的说明中可以，要想将本地`主分支main`的变更推送到远程仓库中，**首先要先拉取远程仓库中的main分支的代码**到本地。原因是远程仓库中有新变更，导致了版本差。

**解决办法：**

- 先使用 `git pull/fetch origin main` 命令拉取远程仓库中的文件。

- 使用带有 `--allow-unrelated-histories` 参数的合并分支命令，将`本地的main分支`跟`远程仓库的main分支`合并。

  ```bash
  #将本地main分支跟远程仓库main分支合并。
  $ git merge  --allow-unrelated-histories origin/main
  Merge made by the 'recursive' strategy.
  README.md | 2 ++
  1 file changed, 2 insertions(+)
  create mode 100644 README.md
  ```

  为什么要使用 `--allow-unrelated-histories` 参数合并分支呢？原因是本地和远程的main分支是毫无相关的历史分支树，如果不使用该参数就不能进行合并。

- 执行完上面两个操作后，输入 `git push origin main` 命令将 `本地main分支` 推送到 `远程仓库的main分支` 中，结果不报错，说明推送成功。

  ```bash
  #将本地main分支推送到远程仓库main分支
  $ git pull origin main
  From github.com:threey333/git_learning_source
   * branch            main       -> FETCH_HEAD
  fatal: refusing to merge unrelated histories
  ```

**结果示意图：**

拉取后输入 `gitk --all`查看：

![image-20210918121311028](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210918143456.png)

将`本地main`和`远程仓库的main`合并后,输入 `gitk --all`查看： 

![image-20210918143444796](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210918143504.png)



### 7.5 查看某个远程仓库信息

使用 `git remote show 仓库名` 命令可以查看某个远程仓库信息。

```bash
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:threey333/git_learning_source.git
  Push  URL: git@github.com:threey333/git_learning_source.git
  HEAD branch: main
  Remote branches:
    beauty tracked
    main   tracked
    test   tracked
  Local branch configured for 'git pull':
    main merges with remote main
  Local refs configured for 'git push':
    beauty pushes to beauty (up to date)
    main   pushes to main   (up to date)
    test   pushes to test   (up to date)

```

从上面的例子信息中可以知道你处于`main`分支，并且如果运行 `git pull`， 就会抓取所有的远程引用，然后将远程 `master` 分支合并到本地 master 分支。



## 八、从远程仓库拉取代码

### 8.1 git fetch命令

`git fetch` 命令可以从远程仓库中获取代码库。

**使用示例：**

```bash
#这里以main主分支为例子说明：

#第一步：比如我有个远程仓库origin，那么我先拉取这个仓库的main分支上的代码到本地来。
#这里我拉取main分支上的代码到本地
$ git fetch origin/main

#第二步：拉下的代码进行工作开发，然后放入暂存区、提交到本地仓库中的main分支上。
$ git commit -am 'add xxx'

#第三步：拉下的代码更新处理工作完后重新推送到远程仓库的main分支上：
$git merge origin/main
```



### 8.2 git pull命令

`git pull` 命令可以从远程仓库中获取代码并合并本地。

`git pull` 其实就是 `git fetch` 和 `git merge FETCH_HEAD` 的简写。 命令格式如下：

```bash
git pull <远程主机名> <远程分支名>:<本地分支名>
```

**使用示例：**

```bash
#这里以远程仓库main主分支为例子说明：

#将远程主机 origin 的 main 分支拉取过来，与本地的 brantest 分支合并。
$ git pull origin master:brantest

#如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
$ git pull origin master
```



#### 8.2.1 git pull命令使用的注意点

一、如果直接使用 `git pull` 命令，那么该命令的效果是：**将与本地当前分支同名的远程分支 拉取到 本地当前分支上(需要先关联远程分支)。**

二、如果直接使用 `git pull origin <远程分支名>:<本地分支名>` 命令，那么该命令的效果是：**将远程指定分支 拉取到 本地指定的分支上。**

三、如果直接使用 `git pull origin <远程分支名>`  命令，那么该命令的效果是：**将远程指定分支 拉取到 本地当前分支上。**



#### 8.2.2 实际工作中pull命令如何使用

每天进行新的功能开发之前，我们可以这么操作（这里以 `test分支` 为例子）：

1. 使用 `git pull origin master/main` 命令从主干分支拉起最新的代码，这样 `test分支` 上就有了 `master` 或 `main` 分支上最新的代码。
2. 或者在远程仓库的 `master/main` 分支上创建一个新的分支，目的是让新建的分支与master分支保持一致，这样新新分支上就会有最新的代码了。



## 九、Git的可视化页面

使用 `gitk --all` 可显示 `Git可视化页面`。

这样页面有助于我们查看各种信息，比如提交历史记录，分支等等。

```bash
$ gitk --all
```

**示意图：**

![image-20210918152501170](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210918152523.png)

