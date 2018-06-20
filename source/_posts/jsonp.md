---
title: JSONP抓取数据
date: 2018-04-25 09:01:30
tags:
	- axios
	- jsonp
	- node.js
categories: 前端
---
## 前言

最近做自己个人项目(球球音乐PC端以及移动端)的时候，涉及到接口数据的抓取。
根据接口的特性,用到了jsonp的方式进行抓取，谈谈自己的总结吧。

>JSONP工作原理

进行jsonp通信时,客户端会在其脚本上新建一个`<script>`元素,其src地址指向API接口地址。
例如:`<script src="https://c.y.qq.com/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg?param1=1&param2=2"></script>`
(加参数的话,可在链接后面直接加相应参数,也可在配置对象里面加),并且提供一个回调函数来接受JSON数据(可自行约定)。

注:jsonp库我用的是node官方的jsonp包,地址[jsonp](https://www.npmjs.com/package/node-jsonp)

## JSONP抓取

>普通抓取(服务器不做限制)

官方提供了三个列子

``` js
	JSONP('http://twitter.com/users/oscargodson.json',function(json){
		console.log(json)
	})

	JSONP('http://api.flickr.com/services/feeds/photos_public.gne',{'id':'12389944@N03','format':'json'},'jsoncallback',function(json){
		console.log(json)
	})

	JSONP('http://graph.facebook.com/FacebookDevelopers', 'callback', function(json){
		console.log(json)
	})
```

## 通过axios代理

>通过axios代理

由于jsonp是一种非正式传输协议，不同于XMLHttpRequest一样需要按照CORS规范,并且配置相应header头文件进行传输。
所以很多公司会对其API进行请求验证。以球球音乐为例,它会对请求header中的host以及referer进行验证,如图所示
![jsonp_axios](/img/jsonp/jsonp_axios.png)

so,看了imocc关于这方面的代码。

最后以`node server`作为中间层,通过axios代理,配置其header后再进行API的调用,以此方式来绕过host验证,从而获取到数据。
配置代码如下:

vue工程的话,在`/build/webpack.dev.conf.js`中进行node server配置以及axios代理。
react工程的话,在其`webpack配置文件`中配置。
``` js
//首先引入axios以及bodyParser
const axios = require('axios')
const bodyParser = require('body-parser')
const devWebpackConfig = merge(baseWebpackConfig, {
  module: {
    rules: utils.styleLoaders({sourceMap: config.dev.cssSourceMap, usePostCSS: true})
  },
  // cheap-module-eval-source-map is faster for development
  devtool: config.dev.devtool,

  // these devServer options should be customized in /config/index.js
  devServer: {
    before(app) {
	  //bodyParser.urlencoded接收req.body,并对其进行解析。
      app.use(bodyParser.urlencoded({extended: true}))
      app.get('/api/getDiscList', function (req, res) {
        const url = 'https://c.y.qq.com/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg'
        axios.get(url, {
          headers: {
            referer: 'https://c.y.qq.com/',
            host: 'c.y.qq.com'
          },
          params: req.query
        }).then((response) => {
          res.json(response.data)
        }).catch((e) => {
          console.log(e)
        })
      })
	}
	//...
  }
  //...
}
```
`api/getDiscList.js`
``` js
export function getDiscList() {
  const url = '/api/getDiscList'
  const data = Object.assign({}, commonParams, {
    platform: 'yqq',
    hostUin: 0,
    sin: 0,
    ein: 29,
    sortId: 5,
    needNewCode: 0,
    categoryId: 10000000,
    rnd: Math.random(),
    format: 'json'
  })

  return axios.get(url, {
    params: data
  }).then((res) => {
    return Promise.resolve(res.data)
  })
}
```
实现原理:1.调用`getDiscList`方法。2.请求node server服务配置地址。3.通过回调函数返回数据。

以上有错误的地方,或者不懂的地方,欢迎来我的微博给我留言。

## End

