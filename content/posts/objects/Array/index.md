+++
title = "数组"
description = "数组"
date = 2022-07-25T10:30:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "对象"
]
series = [
  "Array"
]
tags = [
  'Array'
]

images = []
+++

<!--more-->

### Array

- Array.of(a,b,c...)代替 new Array(...),因为`new Array(3)//[undefined × 3];new Array(3,4)//[3,4]`
- 除 let of 外,遍历:forEach(), filter(), every() 和 some() map(此结果包括空位)都会跳过空位,es6 规定空位被转化为 undefined，由于空位的处理规则非常不统一，所以建议避免出现空位(ff,chrome 将 delete 的位，间隔的位，视为空位，ie 将这些直接转化为 undefined，undefined 为空位)。

#### 函数修改原始值的方法(9 个)

| 修改原始值的方法           | 函数返回值           |
| -------------------------- | -------------------- |
| Array.prototype.copyWithin | 修改后数组           |
| Array.prototype.fill       | 修改后数组           |
| Array.prototype.pop        | 删除的元素           |
| Array.prototype.push       | 数组长度             |
| Array.prototype.shift      | 删除的元素           |
| Array.prototype.unshift    | 数组长度             |
| Array.prototype.sort       | 排序后新数组         |
| Array.prototype.reverse    | 修改后数组           |
| Array.prototype.splice     | 删除的元素组成的数组 |
