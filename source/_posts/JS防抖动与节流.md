---
title: JS防抖动与节流
date: 2018-06-07 09:01:30
tags:
	- JavaScript
categories: 前端
---
## 前言

由于自己在之前的个人项目中涉及到JS防抖动的问题,所以我上网查询了相关资料,顺便谈谈自己对防抖动与节流的总结吧

>应用场景

1. window的resize，scroll事件
2. 拖拽过程中的 mousemove事件
3. 文字输入过程中的keyup
4. 连续快速点击等事件

这些事件会在短时间内多次触发,十分影响浏览器性能而且也不利于以后维护。

## 防抖动(debounce)

>原理

在调用事件之前,设置一个计时器,在规定时间之后才会去调用。
而在规定时间内再次执行这个调用事件的动作的话,就会把原来的定时器clear掉,再重新设置一个定时器。
与函数节流不同,防抖动只有最后一次操作能被触发

>具体代码实现


``` js
// 将会包装事件的 debounce 函数
function debounce(fn, delay) {
  // 维护一个 timer
  let timer = null;

  return function() {
    // 通过 ‘this’ 和 ‘arguments’ 获取函数的作用域和变量
    let context = this;
    let args = arguments;

    clearTimeout(timer);
    timer = setTimeout(function() {
      fn.apply(context, args);
    }, delay);
  }
}
```

``` js
// 当用户滚动时被调用的函数
function foo() {
  console.log('You are scrolling!');
}

// 在 debounce 中包装我们的函数，过 2 秒触发一次
let elem = document.getElementById('container');
elem.addEventListener('scroll', debounce(foo, 2000));
```
## 函数节流(throttle)

节流是另一种处理类似问题的解决方法。 
`节流函数允许一个函数在规定的时间内只执行一次。`

它和防抖动最大的区别就是，`节流函数不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数。(每隔一段时间执行一次)`

比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次 Ajax 请求，而不是在用户停下滚动页面操作时才去请求数据。这样的场景，就适合用节流阀技术来实现。

>原理

当触发事件的时候，我们设置一个定时器，再触发事件的时候，如果定时器存在，就不执行；直到delay秒后，定时器执行函数，清空定时器，这样就可以设置下个定时器。

>具体代码实现

``` js
var throttle = fucntion(func,delay){
    var timer = null;

    return funtion(){
        var context = this;
        var args = arguments;
        if(!timer){
            timer = setTimeout(function(){
                func.apply(context,args);
                timer = null;
            },delay);
        }
    }
}
```
以上有错误的地方,或者不懂的地方,欢迎来我的微博给我留言。

参考链接 [JS防抖动与节流](https://blog.csdn.net/crystal6918/article/details/62236730)

## End

