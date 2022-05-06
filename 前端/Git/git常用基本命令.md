# git常用基本命令

1.$ git config --global user.name “Your Name” 配置git用户信息

2.$ git config --global user.email “email@example.com” 配置git用户信息

3.$ git init 初始化仓库

4.$ git add readme.txt 添加到暂存区

5.$ git commit -m “wrote a readme file” 提交到版本库，备注信息参考git提交规范

6.$ git status git状态（有无未添加到暂存区/提交到版本库内容）

7.$ git diff readme.txt 查看文件有哪些改动

8.$ git log/$ git log --pretty=oneline 提交记录

9.$ git reset --hard HEAD^ 回退一个提交

10.$ cat readme.txt 看文件内容

11.$ git reset --hard 1094a 回退到指定提交

12.$ git reflog 查看所有提交记录

13.$ git checkout – readme.txt 撤销工作区某文件的修改

14.$ git reset HEAD readme.txt 回退暂存区修改回工作区

15.$ ssh-keygen -t rsa -C “youremail@example.com” 生成密匙

16.$ git remote add origin [git@github.com]():michaelliao/learngit.git 添加远程仓库

17.$ git push -u origin master 推送到远程仓库（绑定）

18.$ git push origin master 推送到远程库master分支

19.$ git clone [git@github.com]():michaelliao/gitskills.git 克隆

20.$ git checkout -b dev 创建并切换分支

21.$ git branch dev 创建分支

22.$ git checkout dev 切换分支

23.$ git branch 查看所以分支

24.$ git merge dev 将dev分支合并到当前分支

![img](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/collab/QoJGq7b9bMZrnAKe/0d45d698-48f9-40f8-89d2-f9a243cc6e7c.png)

（多分支）

25.$ git branch -d dev 删除dev分支

\26. $ git add readme.txt （冲突后修改本地文件后提交即可处理冲突）

27.$ git log --graph --pretty=oneline --abbrev-commit 查看所以提交一行展示

28.$ git merge --no-ff -m “merge with no-ff” dev 合并分支（不使用快进方式，保留了被合并分支的提交记录）

![img](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/collab/QoJGq7b9bMZrnAKe/eefcadf4-c396-40a5-8572-f420b4055787.png)

（非快进合并分支）

![img](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/collab/QoJGq7b9bMZrnAKe/90b0ca31-5e99-4da0-bf5e-dd4029be5089.png)

（分布式多分支协作）

29.$ git stash 隐藏工作区修改

30.$ git stash list 查看隐藏列表

31.$ git stash apply 恢复隐藏回工作区

32.$ git stash drop 删掉隐藏记录

33.$ git stash apply stash@{0} 恢复指定隐藏

\34. $ git branch -D feature-vulcan 强制删除某分支

35.$ git remote 查看远程库

36.$ git remote -v 远程库详细信息

37.$ git push origin dev 推送到远程库dev分支

38.$ git checkout -b dev origin/dev 本地创建dev分支并关联远程dev分支

39.$ git pull 拉取远程仓库到本地库

40.$ git branch --set-upstream-to=origin/dev dev 第一次拉取可能不成功，应为未建立连接，需要建立下连接

\41. $ git rebase 提交记录处理为一条直线记录

42.$ git tag v1.0 添加版本

43.$ git tag 查看所以版本

44.$ git tag v0.9 f52c633 给指定提交添加版本号

45.$ git show v0.9 查看版本

46.$ git tag -a v0.1 -m “version 0.1 released” 1094adb 给指定提交指定版本号和备注信息

47.$ git tag -d v0.1 删除某版本

48.$ git push origin v1.0 推送本地版本到远程库

49.$ git push origin --tags 推送所以本地版本到远程库

50.$ git tag -d v0.9 先删除本地版本信息

51.$ git push origin :refs/tags/v0.9 再删除远程版本信息

52.$ git config --global color.ui true 配置git信息（分颜色显示信息）

.gitignore文件

\# Windows:

Thumbs.db

ehthumbs.db

Desktop.ini

(配置希望git忽略提交的文件信息)

53.$ git add App.class 添加到git管理

54.$ git add -f App.class 强制添加到git管理

55.$ git check-ignore -v App.class 添加不成功，查看是否配置为了已忽略文件

56.$ git config --global alias.co checkout 配置指令别名





常见连续操作：

1、回退到指定版本

git log 查看分支的日志

git reset --hard 1094a 回退到指定提交

git push origin HEAD --force 强制提交分支

2、拉取一个分支开发

在git上面从某个分支建立一个分支 例如从master建立的dev/sq分支

git remote update origin --prune 刷新分支列表

git checkout dev/sq 切换本地分支到新建的那个分值