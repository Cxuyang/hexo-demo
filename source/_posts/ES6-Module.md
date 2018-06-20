---
title: ES6模块化随记
date: 2018-02-10 09:01:30
tags:
	- ES6
  	- Module
categories: ES6
---
## 前言

>ES6模块化杂谈

从react的学习回过头来看ES6的模块化
ES6中自带模块化机制，告别了sea.js和require.js
类似于`对象的解构赋值`.

## Start
module.js和es6.js位于同一目录下。
### module.js

``` js
export const name = 'cxy'
export function text(){
	console.log('hello world');
}
```
### es6.js
``` js
import {name, text} from './module.js'
console.log(name)
text()
```
引入module.js中所有内容
``` js
import * as mod from './module.js'
```
## End

