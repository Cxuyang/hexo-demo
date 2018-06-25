---
title: JS模块化规范
date: 2018-02-10 09:01:30
tags:
  - JavaScript
	- ES6
  - 模块化
categories: 前端
---
## 前言

>ES6模块化杂谈

从react的学习回过头来看ES6的模块化
ES6中自带模块化机制，告别了sea.js和require.js
类似于`对象的解构赋值`.

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

## JS模块化规范

>JS模块化规范

学习node.js的过程中回头来谈谈自己对于JS模块化规范的总结(总结比较简单,详情可看我提供的资料或者自行查询)。

### CommonJS规范

为了弥补`JavaScript`相关缺陷，以及为了规范其标准，以达到像Python、Ruby、和Java具备开发大型应用的基础能力。

>CommonJS模块化规范

在CommonJS中，有一个全局性方法require()，用于加载模块。假定有一个数学模块math.js，就可以像下面这样加载。
```js
var math = require('math');
```
然后，就可以调用模块提供的方法：
```js
var math = require('math');
math.add(2,3); // 5
```
CommonJS定义的模块分为:{模块引用(require)} {模块定义(exports)} {模块标识(module)}

require()用来引入外部模块；exports对象用于导出当前模块的方法或变量，唯一的导出口；module对象就代表模块本身。

### AMD规范(异步模块定义)

CommonJS模块规范的延伸。因为之前CommonJS模块化引入方法是`同步`的,不完全适合前端的应用场景,因此AMD规范(异步模块定义)便有了更好的运用。

>AMD模块化规范

AMD也采用require()语句加载模块，但是不同于CommonJS，它要求两个参数：
```js
require([module], callback);
```
第一个参数[module]，是一个数组，里面的成员就是要加载的模块；第二个参数callback，则是加载成功之后的回调函数。如果将前面的代码改写成AMD形式，就是下面这样：
```js
require(['math'], function (math) {
　math.add(2, 3);
});
```
math.add()与math模块加载不是同步的，浏览器不会发生假死。所以很显然，AMD比较适合浏览器环境。目前，主要有两个Javascript库实现了AMD规范：`require.js`和`curl.js`。

### CMD规范

CMD规范由国内的玉伯提出,与AMD规范的主要区别在于定义模块和依赖引入的部分.

AMD需要在声明模块的时候指定所有依赖
```js
define(['dep1', 'dep2'], function (dep1, dep2) {
	return function() {};
})
```
与AMD模块规范相比,CMD模块更接近于Node对CommonJS规范的定义:`define(factory)`
```js
define(function (require, exports, module) {
	// code
})
```
require, exports, module通过形参传递给模块,在需要依赖模块时,再调用require()引入模块。

## 兼容多种模块规范
```js
(function (name, definition) {
  // 检测上下文环境是否为AMD或CMD
  var hasDefine = typeof define === 'function',
    // 检查上下文环境是否为Node
    hasExports = typeof module !== 'undefined' && module.exports;
 
  if (hasDefine) {
    // AMD环境或CMD环境
    define(definition);
  } else if (hasExports) {
    // 定义为普通Node模块
    module.exports = definition();
  } else {
    // 将模块的执行结果挂在window变量中，在浏览器中this指向window对象，在node中为global
    this[name] = definition();
  }
})('hello', function () {
  var hello = function () {};
  return hello;
});
```
## 参考资料

> 《深入浅出node.js》

> [JS模块化规范](https://www.cnblogs.com/chenguangliang/p/5856701.html)

## End

