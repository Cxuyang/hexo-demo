---
title: Hello World
tags: 
	- hexo
  	- hexo theme
categories: hexo
---
## 前言
记录一下我搭建blog的心路历程吧。
我之前选择的是Jekyll+github来搭建blog，但是环境一直没搭好。之间刚好又看到了hexo，环境配置等都比较简单，就转到hexo+github来了。
在此感谢hexo主题Melody的开发者。打算等过段时间工作稳定了再慢慢研究开发自己的hexo theme。

## Quick Start
从hexo官网例子开始
### Create a new post
创建你的hexo工程

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server
在本地运行你的hexo服务

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)
###与github page 建立关联

到这里你会看到_config.yml文件(此为hexo工程配置文件)。
将里面的repo仓库地址，branch分支填好
``` bash
deploy:
  type: git
  repo: git@github.com:your github page
  branch: master
```

### Generate static files
生成静态网页(里面内容也是你关联的github仓库的内容)

``` bash
$ hexo generate
```
修改hexo工程后 可用hexo clean清除缓存
``` bash
$ hexo clean
```
More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites
部署网站到你的github page上
``` bash
$ hexo deploy
```
More info: [Deployment](https://hexo.io/docs/deployment.html)

## theme
主题部分官方以及社区成员给了很多例子。
1.git clone下来放到themes文件夹下面。
2.将统计目录下的_config.yml配置文件里面的theme后面改为你所要用的theme的名称(要与theme文件夹下的主题文件夹名称相同)。
3.一般theme都会有配置说明，对照改成你想要的就行了。
## Archives
文章部分都写在hexo工程source/_posts目录下，采用markdown语法。
一篇文章一个md文件。
## End
后面再补充吧。
