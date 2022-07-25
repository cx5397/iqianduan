+++
title = "加密、解密"
description = "加密、解密"
date = 2022-07-25T10:10:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "计算机基础"
]
series = [
  "安全"
]
tags = [
  '加解密'
]

images = []
+++

<!--more-->

### base64 加解密

```
var enc = window.btoa(str) //编码成base64的
var dec = window.atob(enc) //把base64解码

//中文需要先编码 还可以解码回来
var str = btoa(encodeURIComponent("中文汉字"));
decodeURIComponent(atob(enc)) =>  中文汉字
```

### md5 散列值，用于鉴别传输值是否被篡改(md5.js)

```
var b =$("#logPassword");
$.md5(b.val())

//两次md5 加盐
console.log(md5(md5("Condor Hero") + "a"));
```

### sha1 散列值，用于鉴别传输值是否被篡改，比 md5 强且慢(sha1.js)

```
var sha = hex_sha1('mima123465');
console.log(sha);
```

### RSA 用公钥私钥加密解密(jsencrypt.js)

```
encrypt.setPublicKey(publicKey);
username = encrypt.encrypt(username);
password = encrypt.encrypt(password);
```

### AES

- 最常用的对称加密算法，密钥建立时间短、灵敏性好、内存需求低（不管怎样，反正就是好）
