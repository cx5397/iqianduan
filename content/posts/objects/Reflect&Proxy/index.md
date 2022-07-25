+++
title = "Reflect&Proxy"
description = "Reflect&Proxy"
date = 2022-07-25T10:37:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "对象"
]
series = [
  "Reflect&Proxy"
]
tags = [
  'Reflect&Proxy'
]

images = []
+++

<!--more-->

# 用新对象 Reflect 代替 Object 部分静态方法与 function 的 apply 方法

<pre>
1.原因4点,见http://es6.ruanyifeng.com/#docs/reflect

2.Reflect方法有13个
Reflect.apply(target,thisArg,args)
Reflect.construct(target,args)
Reflect.get(target,name,receiver)
Reflect.set(target,name,value,receiver)
---属性---
Reflect.defineProperty(target,name,desc)//configurable enumerable writable value
Reflect.deleteProperty(target,name)
Reflect.has(target,name)
Reflect.ownKeys(target)//自身可枚举不可枚举属性
Reflect.getOwnPropertyDescriptor(target, name)
Reflect.isExtensible(target)
Reflect.preventExtensions(target)//不能添加新属性
---原型---
Reflect.getPrototypeOf(target)
Reflect.setPrototypeOf(target, prototype)

3.Object剩余静态方法10个
Object.assign()//复制对象
Object.create()
Object.is()//判断两个值是否相等，大致等于===,两点区别(+0，-0不相等;NaN，NaN相等)

Object.freeze()//冰冻对象,只能查询属性值
Object.isFrozen()

Object.seal()//密封对象,只可以修改(仅仅是值),查询已有属性的值。
Object.isSealed()

Object.keys()//自身可枚举属性
Object.values()//自身可枚举属性的值
Object.entries()//自身可枚举属性的[[key,value],...]

备注:Object的实例方法有四个,mdn还有其它5个(3个非标准，2个极少用)
Object.prototype.hasOwnProperty()
Object.prototype.isPrototypeOf()
Object.prototype.propertyIsEnumerable()
Object.prototype.toString()
</pre>

# preventExtensions seal freeze 区别

| function                        | add property | delete property | modify property value | configurable enumerable writable | prototype modify |
| ------------------------------- | ------------ | --------------- | --------------------- | -------------------------------- | ---------------- |
| freeze(啥都不能干了)            | ×            | ×               | ×                     | ×                                | ×                |
| seal(只能修改属性值)            | ×            | ×               | √                     | ×                                | ×                |
| preventExtensions(不能添加新值) | ×            | √               | √                     | √                                | ×                |
