+++
title = "使用正则的场景"
description = "字符串与正则相关的使用方法"
date = 2021-12-26T11:37:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "正则"
]
tags = [
  'replace',
  'exec',
  'test'
  'match',
  'search',
  'split'
 
]
series = [
  "javascript"
]
images = []
+++

字符串与正则相关的使用方法

<!--more-->

# 方法总览
- RegExp.prototype.test
- RegExp.prototype.exec[× 需要while遍历,尽量少用,用match/matchAll代替 ]

- String.prototype.match
- String.prototype.matchAll
- String.prototype.replace
- String.prototype.replaceAll
- String.prototype.split
- String.prototype.search


# 方法详情

### RegExp.prototype.test(String):Boolean, 正则表达式与指定的字符串是否匹配
- 匹配就返回true，不匹配就返回false
```
let str = 'hello world!';
let result = /^hello/.test(str);
console.log(result);
// true
```
- 如果正则表达式设置了全局标志 连续的执行`test()`方法，后续的执行将会从 lastIndex 处开始匹配字符串
```
var regex = /foo/g;

// regex.lastIndex is at 0
regex.test('foo'); // true

// regex.lastIndex is now at 3
regex.test('foo'); // false
```

### RegExp.prototype.exec(String):[Array|null], 在一个指定字符串中执行一个搜索匹配

- 如果匹配成功返回一个数组， 并更新正则表达式对象的 `lastIndex`属性。完全匹配成功的文本将作为返回数组的第一项，从第二项起，后续每项都对应正则表达式内捕获括号里匹配成功的文本。
- 如果匹配失败，`exec()` 方法返回 `null`，并将 `lastIndex` 重置为 0 。
- 如果模式修正符带有g，需要while遍历，多次执行才能得到所有匹配
```
var myRe = /ab*/g;
var str = 'abbcdefabh';
var myArray;
while ((myArray = myRe.exec(str)) !== null) {
  var msg = 'Found ' + myArray[0] + '. ';
  msg += 'Next match starts at ' + myRe.lastIndex;
  console.log(msg);
}
//output
//Found abb. Next match starts at 3 
//Found ab. Next match starts at 9 
//返回myArray结构如
[
    0: "abb" ---第n次万全匹配
    1: "a"   ---第n次括号匹配
    groups: undefined
    index: 0
    input: "abbcdefabh"
    length: 1
]
```


### String.prototype.match/matchAll (RegExp):Array, 返回一个字符串匹配正则表达式的结果

- 如果传入一个非正则表达式对象参数，则会隐式地使用 `new RegExp(obj)` 将其转换为一个 `RegExp`
- 模式修正符g的区别
| 方法 | 带g | 不带g |
| -- |-- | -- |
|match| 1.完整匹配的所有结果，无捕获组 |  2.第一个完整匹配及其相关的捕获组 |
|matchAll|3.所有完整匹配的结果及分组捕获组的迭代器|×|

```
//---1.match带g（×要匹配全部，建议使用matchAll）---
var regexp = /t(e)(st(\d?))/g;
var str = 'test1test2';

str.match(regexp);
// Array ['test1', 'test2']

//---2.match不带g---
var regexp = /t(e)(st(\d?))/g;
var str = 'test1test2';

str.match(regexp);
//Array ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', groups: undefined]

//---3.matchAll 必须带g(可以替代exec方法)---
var regexp = /t(e)(st(\d?))/g;
var str = 'test1test2';
let array = [...str.matchAll(regexp)];

//返回二维数组
array[0];
// ['test1', 'e', 'st1', '1', index: 0, input: 'test1test2', length: 4]
array[1];
// ['test2', 'e', 'st2', '2', index: 5, input: 'test1test2', length: 4]
```

### String.prototype.replace/replaceAll (RegExp|substr, newSubStr|function), 替换字符串
- 该方法并不改变调用它的字符串本身，而只是返回一个新的替换后的字符串

- 第一个参数的区别
| 方法 | 字符串 | 正则带g | 正则不带g |
| -- |-- | -- | -- |
|replace| 替换第一个匹配到的 | 替换所有匹配到的(×建议语义化replaceAll替代) |  2.替换第一个匹配到的 |
|replaceAll| 替换所有匹配的 | 3.替换所有匹配到的 | × |

```
//---2 replace 正则不带g---
var re = /h/;
var str = "John Smith";
var newstr = str.replace(re, "?");
// Jo?n Smith
console.log(newstr);

//---3 replaceAll---
var re = /h/g;
var str = "John Smith";
var newstr = str.replaceAll(re, "?");
// Jo?n Smit?
console.log(newstr);
```

- 第2个参数为newSubStr时,用`$1-99`表示第 n 个括号匹配的字符串,如果不存在第 n个分组，那么将会把匹配到到内容替换为字面量。比如不存在第3个分组，就会用“\$3”替换匹配到的内容;“\$&” 插入匹配的子串;“\$\`”插入当前匹配的子串左边的内容;“\$'” 插入当前匹配的子串右边的内容

```
//交换一个字符串中两个单词的位置
var re = /(\w+)\s(\w+)/;
var str = "John Smith";
var newstr = str.replace(re, "$2, $1");
// Smith, John
console.log(newstr);
```

- 第2个参数为函数时,函数参数为function(match,p1,p2,...pn,offset,string),match匹配到的字符串;p1,p2...pn相当于$1-$99括号匹配;offset匹配到的子字符串在原字符串中的偏移量;string被匹配的原字符串

```
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is nondigits, p2 digits, and p3 non-alphanumerics
  //match= abc12345#$*%
  return [p1, p2, p3].join(' - ');
}
var newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
console.log(newString);  // abc - 12345 - #$*%
```

### String.prototype.search (RegExp):index, RegExp在字符串中首次匹配项的索引
- 返回首次匹配项的索引，未匹配则返回 `-1`
- 类似于正则表达式的`test():Boolean`方法,`'ssass'.indexOf('a')`方法; 当要了解更多匹配信息时，可使用 `match()`方法
```
var str = "hey JudE";
var re = /[A-Z]/g;
var re2 = /[.]/g;
console.log(str.search(re)); // returns 4, which is the index of the first capital letter "J"
console.log(str.search(re2)); // returns -1 cannot find '.' dot punctuation
```

### String.prototype.split ([separator[, limit]] ):Array, 用separator把字符串分割成数组 

- `separator` 可以是一个字符串或正则表达式
```
var myString = "Hello 1 word. Sentence number 2.";
var splits = myString.split(/(\d)/);
//[ "Hello ", "1", " word. Sentence number ", "2", "." ]
console.log(splits);
```

- `separator`如果是数组会转化为字符串
```
const myString = 'ca,bc,a,bca,bca,bc';

const splits = myString.split(['a','b']);
// myString.split(['a','b']) is same as myString.split(String(['a','b']))

console.log(splits);  //["c", "c,", "c", "c", "c"]
```

- limit为一个整数，限定返回的分割片段数量
```
var myString = "Hello World. How are you doing?";
var splits = myString.split(" ", 3);
//['Hello', 'World.', 'How']
console.log(splits);
```