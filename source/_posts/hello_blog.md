---
title: Hello Blog
date: 2018-02-08 09:01:30
tags: 
	- hexo
  	- hexo theme
categories: hexo
---
## 前言
>Hello Blog

记录一下我搭建blog的心路历程吧。
我之前选择的是`Jekyll+github`来搭建blog，但是环境一直没搭好。之间刚好又看到了hexo，环境配置等都比较简单，就转到`hexo+github`来了。
在此感谢hexo主题Lite的开发者。打算等过段时间工作稳定了再慢慢研究开发自己的hexo theme。

## Quick Start
从hexo官网例子开始
### Create a new post
创建你的hexo工程

``` cmd
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server
在本地运行你的hexo服务

``` cmd
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)
### 与github page 建立关联

到这里你会看到_config.yml文件(此为hexo工程配置文件)。
将里面的repo仓库地址，branch分支填好
``` cmd
deploy:
  type: git
  repo: git@github.com:your github page
  branch: master
```

### Generate static files
生成静态网页(里面内容也是你关联的github仓库的内容)

``` cmd
$ hexo generate
```
修改hexo工程后 可用hexo clean清除缓存
``` cmd
$ hexo clean
```
More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites
部署网站到你的github page上
``` cmd
$ hexo deploy
```
More info: [Deployment](https://hexo.io/docs/deployment.html)

## theme
主题部分官方以及社区成员给了很多很好看的主题，详情见[hexo_theme](https://hexo.io/themes)。
1. git clone主题仓库放到themes文件夹下面。
2. 将同一目录下的_config.yml配置文件里面的theme后面改为你所要用的theme的名称(`要与theme文件夹下的主题文件夹名称相同`)。
3. 一般theme都会有配置说明，对照改成你想要的就行了。

## Archives
hexo文章采用[markdown语法](https://www.appinn.com/markdown/index.html),一篇文章一个md文件。
新建markdown文件的话,在hexo工程目录下执行以下命令就行了
``` cmd
$ hexo new [layout] <title>
```
``` cmd
注：
布局	路径
post	source/_posts
page	source
draft	source/_drafts
```
## End
后面再补充吧。
