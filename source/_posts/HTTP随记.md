---
title: HTTP随记
date: 2018-11-23 09:01:30
tags:
  - HTTP
  - 前端
  - Nginx
categories: 前端
---
## 前言

基础知识回顾HTTP篇

### 1.HTTP协议基础及发展历史

#### 1.1HTTP发展历史

##### HTTP 0.9

1. 只有一个命令（GET）
2. 没有header等描述数据的信息
3. 服务器发送完毕,就关闭TCP连接

##### HTTP 1.0

1. 增加了很多命令(POST,PUT等)
2. 增加status code 和 header
3. 多字符集支持、多部分发送、权限、缓存等

##### HTTP 1.1

1. 支持了持久连接(HTTP请求后可以不关闭tcp连接)
2. Pipeline(在同一个tcp连接里可以支持多个HTTP请求)
2. 增加host和其他一些命令

##### HTTP 2.0(暂未普及)

1. 所有数据以二进制传输
2. 同一个连接里面发送多个请求不在需要按照顺序来
3. 头信息压缩以及推送等提高效率的功能(推送:服务器端主动向前端推送数据)

#### 1.2HTTP协议基础

![requestProcess](https://github.com/Cxuyang/hexo-demo/blob/master/source/img/http/request.png)

1.如果直接访问域名则还是会先进行dns解析，再进行其他操作。如果是IP就会跳过dns解析，随后判断是否有重定向
2.dns缓存

![model](https://github.com/Cxuyang/hexo-demo/blob/master/source/img/http/model.png)
1. 低三层
a. 物理层: 物理层主要作用是定义物理设备如何传输数据(网线,网卡,电脑等硬件)
b. 数据链路层: 数据链路层在通信的实体间建立数据链路链接
c. 网络层: 网络层为数据在节点之间传输创建逻辑链路(自己电脑与服务器间)
2. 传输层 (TCP IP)
3. 应用层（HTTP）

### 2.http的三次握手

![request](https://github.com/Cxuyang/hexo-demo/blob/master/source/img/http/shakehans.png)
请求过程: 用户在客户端发起http请求 然后 建立tcp 连接 ，把请求发送给服务器端

![shakehands](https://github.com/Cxuyang/hexo-demo/blob/master/source/img/http/shakehans1.png)
1.	客户端发送一个我要创建连接的数据包
2.	服务器端开放一个端口并返回给客户端带有标识的数据包
3.	客户端表示服务器端允许连接后再发送一个带有标识的数据包

>3次握手的目的

防止服务器端开启一些无用的连接(规避网络原因等引起无用的一些端口开销和连接)

### 3.URI URL URN

1. URI: 统一资源标识符（Uniform Resource Identifier，或URI)  用来唯一标识互联网上的信息资源
2. URL：统一资源定位符（Uniform Resource Locator）http  ftp 等都是url
```
a. 协议部分：该URL的协议部分为“http：”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符
b. 域名部分：该URL的域名部分为“www.aspxfans.com”。一个URL中，也可以使用IP地址作为域名使用
c. 端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口
d. 虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”
e. 文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名
f.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分
g.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。
```
3. URN：永久统一资源定位符 在资源移动之后还能找到

### 4.Cache-Control

1. 可缓存性
`Public`  http经过任何地方都可以缓存(服务器,代理服务器等)
`Private` 发起请求的浏览器才可以进行缓存
`No-cache` 可以服务器同意后才能使用缓存
2. 到期
`Max-age=<seconds>`  缓存多少秒过期
`s-maxage=<seconds>` 仅在代理服务器上设置的缓存到期时间
`max-stale=<seconds>` 在设置的缓存过期时间后还能使用过期的缓存
3. 重新验证
`Must-revalidate`  在缓存过期之后必须去服务器重新请求
`Proxy-revalidate`  缓存服务器使用的
4. 其他
`No-store`  本地和服务器都不能存储这个缓存
`No-transform` 代理服务器用

### 5.资源验证(用于设置了no-cache的验证)

>验证头
1. Last-Modified
上次修改时间
服务器端 使用`If-Modified-Since`或者`If-Unmodified-Since`
对比上次修改时间以验证资源是否需要更新
2. Etag
数据签名
服务器端 使用`If-Match`或者`If-Non-Match`
对比资源的签名 判断是否使用缓存

### 6.Cookie

通过Set-Cookie字段设置, 下次请求会自动带上, 以键值对,可以设置多个, `Cookie不等于session`
`属性`
1. Max-age(设置一个过期的时间, 比如几秒)和expires(设置一个具体的过期时间点)设置过期时间
2. Secure只在https的时候发送
2. HttpOnly无法通过documen.cookie访问(无法通过js获取)

### 7.http长链接(Connection:keep-alive,请求后不关闭tcp链接)

1. 设置后, 多个http请求共用一个tcp链接(chorme为最多支持6个并发的http请求)
2. http2.0 支持一个tcp链接 可以多个http请求

### 8.数据协商(客服端发送请求,表明想要什么类型的数据)

>请求

1. Accept 想要的数据类型
2. ACCEPT-Encoding  编码方式(压缩方式, gzip,deflate,br)
3. Accept-Language  语言
4. User-Agent  移动端或者PC端
5. Content-Type  传送的数据格式
6. ...

>返回

1. Content  服务器端返回的数据类型
2. Content-Type  返回的数据格式
3. Content-Encoding  返回的编码方式(压缩方式, gzip,deflate,br)
4. Content-Language  返回的语言
5. X-Content-Type-Options: nosniff   告诉浏览器不要猜测返回数据类型
6. ...

![accept-content](https://github.com/Cxuyang/hexo-demo/blob/master/source/img/http/accept-content.png)

### 9.Redirect(重定向)

``` js
if (request.url === '/') {
  // stateCode(状态码): 301-永久重定向,清除浏览器缓存消失; 302-每次都会进入浏览器进行跳转
  let stateCode = '301'
  response.writeHead(stateCode. {
    'Location': '/new'
  })
  response.end('')
}
```
### 10.Content-Security-Policy(内容安全策略)

1. 限制资源获取
2. 报告资源获取越权

>限制方式

1. default-src 限制全局
2. 限制指定资源类型(connect-src, img-srcm, script-src, style-src...)


### 以后再补充

## End

