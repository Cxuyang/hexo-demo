---
title: linux部署与nginx代理
date: 2018-07-05 09:01:30
tags:
  - Node.js
  - Linux
  - Nginx
categories: 前端
---
## 前言

总结一下自己在linux(`以下以centos7.2为例`)下部署网站所遇到的问题以及感想。`(linux系统、nginx基本知识就不展开讲了)`

### 静态网站部署

1. 安装好linux系统以及`vim`、`nginx`等等常用软件
2. 进入 `/etc/nginx/conf.d` 目录，一般会有个`default.conf`配置文件。以你的工程名新建一个`.conf文件`,内容同`default.conf`相同。
3. 配置`.conf`文件(详情配置说明见[.conf](https://blog.csdn.net/u010320371/article/details/78995097))
4. location 配置(以`sell`工程为例)
``` .conf
location /sell/ {
  rewrite ^/server_name/sell/index.html  //server_name为你的IP或者域名
  root   /data/www;  //项目路径位置
  index  index.html index.htm;
}
```

### nginx服务代理

>以我之前的球球音乐为例

1. 安装node.js、pm2
2. 修改你前台请求node server地址为服务器的IP或者域名
3. 后面步骤与静态部署相同
4. pm2启动express文件
5. location配置
``` .conf
location /qqmusic/ {
  rewrite ^/server_name/qqmusic/index.html  //server_name为你的IP或者域名
  proxy_set_header X-RealIP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header Host $http_host;
  proxy_set_header X-Nginx-Proxy true;
  proxy_pass http://127.0.0.1:4000/; // 这里的端口要和你的node服务端口相同
  root   /data/www;  //项目路径位置
  index  index.html index.htm;
}
```
### 以后再补充

## End

