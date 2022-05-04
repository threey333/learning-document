# 利用 GitHub +  jsDelivr + PicGo + Typora 搭建属于自己的图床

## 一、GitHub配置

### 1.1 创建仓库

要搭建图床首先要在 GitHub 上创建一个用来存放图片的仓库。

![image-20210827190133638](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827190138.png)

### 1.2 生成token

`token`是用来给 `PicGo`上传图片到GitHub仓库的令牌，此token要自己**记录**下来！ 

**步骤：**

一、进入设置

![image-20210827190914639](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827190920.png)

二、选择开发者设置

![image-20210827191103639](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827191112.png)

三、生成新的token

![image-20210827192046013](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827192047.png)

四、勾选repo即可

![image-20210827191843165](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827191844.png)



## 二、PicGo配置

### 2.1 下载PicGo

https://github.com/Molunerfinn/picgo/

### 2.2 配置GitHub图床

![image-20210827192318375](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827192324.png)

**说明：**

- 设定仓库名：填你在GitHub上创建的用来存放图片的仓库。格式是：**【用户名/仓库名】**

- 设定分支名：main主分支（之前是master，现在改成了main）

- 设定的Token：你在GitHub上生成的 token 信息，复制粘贴上去就行。

- 存储路径：你自己看这填就行。

- 设定自定义域名：采用jsDelivr CDN访问加速，格式是：**【https://cdn.jsdelivr.net/gh/用户名/仓库名】**

  最终上传图片后，会自动拼接链接【https://cdn.jsdelivr.net/gh/用户名/仓库名/路径/图片名】，如“https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827192324.png”

### 2.3 PicGo设置Server

当使用PicGo上传图片到GitHub时，可能出现上传图片失败，当检查不是仓库名和自定义域名的问题后，可能就是Server端口问题了，这里如果不是36677端口则会出现上传图片失败。

![image-20210827193051445](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827193052.png)



## 三、 jsDelivr用法

地址：[https://www.jsdelivr.com/](https://www.jsdelivr.com/features)

这里我们直接利用https://cdn.jsdelivr.net/gh，所以不多说，想了解更多信息可以去官网地址看。



## 四、Typora配置

### 4.1 下载Typora

下载地址：https://www.typora.io/

### 4.2 配置Typora从而可以上传图片

第一步：

![image-20210827193632533](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827193633.png)

第二步：

![image-20210827193826917](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827194041.png)

第三步：检查是否可以上传图片：

这里有两个方式检查：

- 1.在 偏好设置->图像 中点击验证图片上传选项。

  ![image-20210827194034836](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827194036.png)

- 2.在PicGo中的上传区上传图片：

  ![image-20210827194133442](https://cdn.jsdelivr.net/gh/threey333/Picture/img/20210827194531.png)



做完以上操作，那么自己的图床也就完成了。



## 五、错误处理

当出现如下错误，我们可以通过**将Github设置为默认图床** 的方式解决。

![image-20220413002633961](https://cdn.jsdelivr.net/gh/threey333/Picture/img/image-20220413002633961.png)

