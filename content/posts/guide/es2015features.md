+++
title = "es2015新特性"
description = "es2015推出后，编码的改变"
date = 2022-07-25T09:10:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "清单"
]
series = [
  "编码规范"
]
tags = [
  'es2015'
]

images = []
+++

<!--more-->

# [es2015(es6) Core features](http://exploringjs.com/es6/ch_core-features.html#sec_from-iifes-to-blocks)

- From var to const/let (var 声明的全局变量，会加入 window 对象，千万不要用 var！)
- From IIFEs(函数包裹作用域,立即执行函数) to blocks({})
- From concatenating strings(拼接字符串 +) to template literals(模版字符串\`${}\`)
- From function expressions to arrow functions
- Multiple return values via(通过) objects 解构赋值
- From for to forEach() to for-of 数组遍历优先级 for=>forEach=>for of
- Handling parameter default values 函数申明默认参数
- Handling named parameters
<pre>
function selectEntries(options) {
    options = options || {}; // (A)
    var start = options.start || 0;
    var end = options.end || -1;
    var step = options.step || 1;
    ···
}
to 
function selectEntries({ start=0, end=-1, step=1 } = {}) {
    ···
}
</pre>
- From arguments to rest parameters
- From apply() to the spread operator (...)
- From concat() to the spread operator (...)
- From function expressions in object literals to method definitions
<pre>
var obj = {
    foo: function () {
        ···
    },
    bar: function () {
        this.foo();
    }, // trailing comma is legal in ES5
}
to
const obj = {
    foo() {
        ···
    },
    bar() {
        this.foo();
    },
}
</pre>
- From constructors to classes
- From custom error constructors to subclasses of Error
- From objects to Maps
- New string methods
- New Array methods
- From CommonJS modules to ES6 modules

# 语言进化

- module and class default is use strict 类和模块隐式为严格模式;
