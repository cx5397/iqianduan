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

# 虚假加密

##### base64(原生方法或 CryptoJS)

```
var enc = window.btoa(str) //编码成base64的
var dec = window.atob(enc) //把base64解码

//中文需要先编码 还可以解码回来
var str = btoa(encodeURIComponent("中文汉字"));
decodeURIComponent(atob(enc)) =>  中文汉字
```

# 消息摘要（a->b,不可逆、a 只要修改了，b 就会有很大不同）

##### MD5(md5.js、cryptoJS)

##### SHA(sha1.js、cryptoJS)

# 对称加密(加密与解密秘钥相同)

##### DES(CryptoJS)

```
var ciphertext = CryptoJS.DES.encrypt(message, key, cfg);
```

##### 3DES(CryptoJS)

```
var ciphertext = CryptoJS.TripleDES.encrypt(message, key, cfg);
var plaintext  = CryptoJS.TripleDES.decrypt(ciphertext, key, cfg);
```

##### AES(CryptoJS)

```
var CryptoJS = require("crypto-js");

// Encrypt
var ciphertext = CryptoJS.AES.encrypt('my message', 'secret key 123').toString();

// Decrypt
var bytes  = CryptoJS.AES.decrypt(ciphertext, 'secret key 123');
var originalText = bytes.toString(CryptoJS.enc.Utf8);

console.log(originalText); // 'my message'
```

# 非对称加密(加密与解密秘钥不同)

##### RSA(jsencrypt.js)

```
encrypt.setPublicKey(publicKey);
username = encrypt.encrypt(username);
password = encrypt.encrypt(password);
```

##### ECC 更适合实现到资源严重受限制的设备中

# 国密[sm-crypto](https://github.com/JuneAndGreen/sm-crypto)，密钥长度和分组长度均为 128 位

##### SM1 对称加密，SM 1 由获得国密办资质认证的特定机构将算法封装在芯片中，并销售给指定的厂商，需要调用加密芯片的接口进行使用

##### SM2 非对称加密，具有自主知识产权的 ECC 算法

```
//获取密钥对
const sm2 = require('sm-crypto').sm2

let keypair = sm2.generateKeyPairHex()

publicKey = keypair.publicKey // 公钥
privateKey = keypair.privateKey // 私钥

// 默认生成公钥 130 位太长，可以压缩公钥到 66 位
const compressedPublicKey = sm2.compressPublicKeyHex(publicKey) // compressedPublicKey 和 publicKey 等价
sm2.comparePublicKeyHex(publicKey, compressedPublicKey) // 判断公钥是否等价

// 自定义随机数，参数会直接透传给 jsbn 库的 BigInteger 构造器
// 注意：开发者使用自定义随机数，需要自行确保传入的随机数符合密码学安全
let keypair2 = sm2.generateKeyPairHex('123123123123123')
let keypair3 = sm2.generateKeyPairHex(256, SecureRandom)

let verifyResult = sm2.verifyPublicKey(publicKey) // 验证公钥
verifyResult = sm2.verifyPublicKey(compressedPublicKey) // 验证公钥
//=========================================
const sm2 = require('sm-crypto').sm2
const cipherMode = 1 // 1 - C1C3C2，0 - C1C2C3，默认为1

let encryptData = sm2.doEncrypt(msgString, publicKey, cipherMode) // 加密结果
let decryptData = sm2.doDecrypt(encryptData, privateKey, cipherMode) // 解密结果

encryptData = sm2.doEncrypt(msgArray, publicKey, cipherMode) // 加密结果，输入数组
decryptData = sm2.doDecrypt(encryptData, privateKey, cipherMode, {output: 'array'}) // 解密结果，输出数组
```

##### SM3 消息摘要

```
const sm3 = require('sm-crypto').sm3

let hashData = sm3('abc') // 杂凑

// hmac
hashData = sm3('abc', {
    key: 'daac25c1512fe50f79b0e4526b93f5c0e1460cef40b6dd44af13caec62e8c60e0d885f3c6d6fb51e530889e6fd4ac743a6d332e68a0f2a3923f42585dceb93e9', // 要求为 16 进制串或字节数组
})
```

##### SM4 分组密码算法

```
const sm4 = require('sm-crypto').sm4
const msg = 'hello world! 我是 juneandgreen.' // 可以为 utf8 串或字节数组
const key = '0123456789abcdeffedcba9876543210' // 可以为 16 进制串或字节数组，要求为 128 比特

let encryptData = sm4.encrypt(msg, key) // 加密，默认输出 16 进制字符串，默认使用 pkcs#7 填充（传 pkcs#5 也会走 pkcs#7 填充）
let encryptData = sm4.encrypt(msg, key, {padding: 'none'}) // 加密，不使用 padding
let encryptData = sm4.encrypt(msg, key, {padding: 'none', output: 'array'}) // 加密，不使用 padding，输出为字节数组
let encryptData = sm4.encrypt(msg, key, {mode: 'cbc', iv: 'fedcba98765432100123456789abcdef'}) // 加密，cbc 模式
```
