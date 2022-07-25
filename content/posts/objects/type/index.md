+++
title = "类型转换"
description = "类型转换"
date = 2022-07-25T10:10:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "对象"
]
series = [
  "类型"
]
tags = [
  '类型转换'
]

images = []
+++

<!--more-->

# [类型转换](http://javascript.ruanyifeng.com/grammar/conversion.html)

## Number(x),x 能转成功的有 1 数字,2 纯数字字符串(会自动去掉前后空格，parseInt 也是),3 空字符串,4true,5false,6null,7[最多只有一个元素且是 1,2 情况]；x 转化过程 1.先调用 valueOf 2.如果返回对象则调用 toString 3.还是对象就报错

```
 Number(123|'123') //123
 Number('123abc') //NaN
 Number.parseInt('123abc') //123
 Number(true|false) //1|0
 Number(null|undefined) //0|NaN
 Number([]) //0
 Number([1]) //1
 Number({}) //NaN
```

## String(x) x 转化过程,1.调用 toString,2.返回是对象再调用 valueOf,3.还是对象报错

```
String(122) //"122"
String('123') //"123"
String(true|false) //"true"|"false"
String(undefined|null)//"undefined"|"null"
String({}) //"[object Object]"
String([1,2,3]) //"1,2,3"
```

## Boolean(x) x 为 undefined、null、-0、+0、NaN、''、为 false，其余全部为 true

```
Boolean(new Boolean(false)) //true
```

## 自动转换

### 自动转换场景

- 不同类型的数据相互运算(123+'abc')
- 对非布尔值类型的数据求布尔值(if('abc'){})
- 对非数值类型的值使用一元运算符(+ -)

### 自动转化规则

- 预期是什么类型的值就调用该类型转换函数
- 如果该位置预期即可以是字符串也可以是数值，转化为数值
- 建议显示类型转换
- 除了加法运算符（+）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。
- 一元运算符也会把运算子转成数值。+true //1
