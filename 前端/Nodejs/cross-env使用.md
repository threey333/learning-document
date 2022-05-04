# cross-env 使用

## 一、cross-env是什么

运行跨平台设置和使用环境变量的脚本

## 二、出现原因

当您使用 `NODE_ENV=production`, 来设置环境变量时，大多数 Windows 命令提示将会阻塞(报错)。（异常是Windows上的Bash，它使用本机Bash。）换言之，Windows 不支持 NODE_ENV=production 的设置方式。

## 三、解决

cross-env 使得您可以使用单个命令，而不必担心为平台正确设置或使用环境变量。这个迷你的包(cross-env)能够提供一个设置环境变量的 scripts，让你能够以 Unix 方式设置环境变量，然后在 Windows 上也能兼容运行。

## 四、安装

`npm install --save-dev cross-env`

## 五、使用

```json
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

NODE_ENV环境变量将由 cross-env 设置 打印 process.env.NODE_ENV === 'production'
