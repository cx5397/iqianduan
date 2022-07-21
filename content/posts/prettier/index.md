+++
title = "prettier格式化函数名后(之间出现空格"
description = "使用prettier格式化后函数名后(之间出现空格"
date = 2022-07-21T12:10:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "前端"
]
tags = [
  'prettier'
]
series = [
  "代码格式化"
]
images = []
+++

<!--more-->

### 现象：同样的 package.json 及版本， 代码格式化却有两种不同代码质量检查与格式化，导致运行编译不通过

```
# 1.匿名函数function 后面有个空格
[1,2].sort(function (x, y) {
   return x - y;
});

# 2.匿名函数function 后面没有空格
[1,2].sort(function(x, y) {
   return x - y;
});
```

### 原因：eslint-plugin-prettier 依赖的 prettier 版本>=1.13.0,没有限定清楚，prettie1 版本和 2 版本有格式化上的区别，以前老项目安装的是 1.19.1，现在新项目是 2.5.1（在没有 lock 文件锁定版本依赖时）

```
"node_modules/eslint-plugin-prettier": {
 "version": "3.4.1",
 "resolved": "https://registry.npmmirror.com/eslint-plugin-prettier/download/eslint-plugin-prettier-3.4.1.tgz",
 "integrity": "sha1-6d2yAO+289Bf/oOxZlpxavSjh+U=",
 "dev": true,
 "dependencies": {
   "prettier-linter-helpers": "^1.0.0"
 },
 "engines": {
   "node": ">=6.0.0"
 },
 "peerDependencies": {
   "eslint": ">=5.0.0",
   "prettier": ">=1.13.0"
 },
 "peerDependenciesMeta": {
   "eslint-config-prettier": {
     "optional": true
   }
 }
}
```

### 解决方法：

- 1.已有工程 package.lock.json 缺失的，需要补齐，自始至终都不要删除，每次需要提交，禁止使用 cnpm 命令，然后执行 npm install <prettier@1.19.1> -D,固定为现象 2 的格式化
- 2.升级 eslint-plugin-prettier 至 4.2.1 使其依赖 prettier 为 2 版本，对应的其它组件也需要升级(不推荐)，后面会配置好后在项目生成工具支持，在新创建工程采用
