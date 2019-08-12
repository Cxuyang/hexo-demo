---
title: vue-cli3自定义传参
date: 2019-07-31 09:01:30
tags:
  - JavaScript
  - vue-cli3
  - webpack
categories: 前端
---
## 前言

>需求说明

因为项目需求,需要在docker打包时能够在打包命令后面自定义传参,来动态改变配置文件
但是因为vue-cli3做了类似黑盒处理 不能像npm命令那样传参(process.argv始终为空)
所以在询问了其他人后有了以下方法

## 自定义传参

虽然叫做自定义传参，不过方法是在webpack中使用DefinePlugin设定环境变量,然后在main.js中读取就行

``` vue.config.js
/* vue.config.js */
const webpack = require('webpack')
const arg = process.argv[process.argv.length - 1]
module.exports = {
  configureWebpack: {
    plugins: [
      new webpack.DefinePlugin({
        EnvParams: JSON.stringify(arg)
      })
    ]
  }
}
```

``` 命令行
/* 命令行 */
npx vue-cli-service serve --test=123
or
npm run serve -- --test=123
(注: 使用npm run serve --test=123依然取不到参数)
```

``` main.js
/* main.js */
console.log(EnvParams) // --test=123
```

## End

