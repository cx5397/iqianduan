+++
title = "nginx相关配置"
description = "nginx安全头，服务器信息隐藏，安全访问控制等"
date = 2022-07-21T14:10:51+08:00
featured = false
comment = true
toc = true
reward = true
categories = [
  "前端"
]
tags = [
  'nginx'
]
series = [
  "部署"
]
images = []
+++

nginx 安全头，服务器信息隐藏，安全访问控制等

<!--more-->

# 隐私、安全

### 配置的位置 Context（上下文）: http, server, location，（建议配置 server，全局）

### 隐藏 nginx 版本号

- 添加`server_tokens off; #隐藏掉nginx的版本号`

### 隐藏 response 的 X-Powered-By 字段

- 添加 `fastcgi_hide_header X-Powered-By; #隐藏X-Powered-By`

### 重定向不自动带上端口号

- 添加 `port_in_redirect off; #重定向是否带上端口，关闭后，则响应头Location的URL即是重定向url，不会自动加上端口号`

### 限定可以访问的用户(在明确目标访问用户 ip 情况下配置，不强制要求)

```
location / {
    allow 192.168.1.0/24; #代表允许 192.168.1.* 的任何地址访问
    allow 10.1.1.0/16; #代表允许 10.1.. 的任何地址访问
    deny  all;
}
```

### 开启日志功能

- 添加 `access_log /log/path/dir; #开启日志功能`
- 添加 `access_log off; #关闭日志功能`
- 开启日志后磁盘消耗会增大，需要注意

### timeout 配置(可以不配置,都有默认值，下面即默认情况)

- 添加`client_body_timeout 60s; #等待client发送一个请求头的超时时间为60秒`
- 添加`client_body_timeout 60s;#请求体（request body）的读超时时间`
- 添加`keepalive_timeout 75s; #活动的客户端连接将在服务器端保持打开状态时间`
- 添加`send_timeout 60s; #将响应传输到客户端的超时`

### ---配置总结---

```
server_tokens off;#隐藏掉nginx的版本号
fastcgi_hide_header X-Powered-By; #隐藏X-Powered-By
port_in_redirect off; #重定向是否带上端口，关闭后，则响应头Location的URL即是重定向url，不会自动加上端口号
access_log /log/path/dir; #开启日志功能
```

# 安全头

## X-Frame-Options

X-Frame-Options HTTP  响应头是用来给浏览器指示允许一个页面可否在 frame、iframe、embed、object 中展现的标记。站点可以通过**确保网站没有被嵌入到别人的站点里面**，从而避免点击劫持攻击

#### X-Frame-Options  有两个可能的值：

- `X-Frame-Options: DENY`
- `X-Frame-Options: SAMEORIGIN`

#### 说明

- DENY 表示该**页面不允许在 frame 中展示**，即便是在相同域名的页面中嵌套也不允许。
- SAMEORIGIN 表示该页面**可以在相同域名页面的 frame 中展示**。规范让浏览器厂商决定此选项是否应用于顶层、父级或整个链，有人认为该选项不是很有用，除非所有的祖先页面都属于同一来源

#### 配置建议

- Nginx 配置为当前页面可以通过 iframe 嵌入到同源（ip 端口 协议相同）的其它页面中：
- `add_header 'X-Frame-Options' 'SAMEORIGIN';`

#### 配置生效位置：

- location 下 html 静态页面

## X-Content-Type-Options

X-Content-Type-Options HTTP 消息头相当于一个提示标志，被服务器用来提示客户端一定要遵循在  Content-Type  首部中对   MIME 类型   的设定，而不能对其进行修改。这就**禁用了客户端的  MIME 类型嗅探行为**，换句话说，也就是意味着网站管理员确定自己的设置没有问题，要求浏览器只能够用 response 中 content-type 中的类型解析返回的文件，而不要按照浏览器自己的嗅探去解析

#### X-Frame-Options  有 1 个可能的值：nosniff

#### 配置建议

- `add_header 'X-Content-Type-Options' 'nosniff';`
- `add_header 'Content-Type' 'text/html; charset:utf-8';#location /配置了nosniff后需要加上这一行，否则vue路由直接页面显示返回的html字符`

##### 下面两种情况的请求将被阻止：

- 请求类型是"style" 但是 MIME 类型不是 "text/css"，
- 请求类型是"script" 但是 MIME 类型不是   JavaScript MIME 类型

#### 配置生效位置：

location 下.css .js 文件

## Content-Security-Policy

HTTP 响应头[Content-Security-Policy](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy)允许站点管理者控制用户代理能够为**指定的页面加载哪些资源**。除了少数例外情况，设置的政策主要涉及指定服务器的源和脚本结束点。这将帮助防止跨站脚本攻击（Cross-Site Script）

#### 语法：

- `Content-Security-Policy: <policy-directive>; <policy-directive>`
- 示例

```
//如果网站引入了其他域的资源 可以在'self' 后面加上完整的域（包含协议、端口
）
Content-Security-Policy: default-src 'self' http://example.com;
                         connect-src 'none';
Content-Security-Policy: connect-src http://example.com/;
                         script-src http://example.com/;

//如果提示The following directives either allow wildcard sources (or ancestors), are not defined, or are overly broadly defined:
// frame-ancestors, form-action The directive(s): frame-ancestors, form-action are among the directives that do not fallback to default-src,
//missing/excluding them is the same as allowing anything.
//就在后面加指令 frame-ancestors 'none' ;  form-action 'none';
```

#### 配置建议

```
add_header 'Content-Security-Policy' "default-src 'self' 'unsafe-inline' 'unsafe-eval' blob:  data:;";
```

#### 配置生效位置：

location 下 html 文件

## X-XSS-Protection

HTTP X-XSS-Protection  响应头是 Internet Explorer，Chrome 和 Safari 的一个特性，当检测到跨站脚本攻击 (XSS (en-US)) 时，浏览器将停止加载页面。若网站设置了良好的  Content-Security-Policy  来禁用内联 JavaScript ('unsafe-inline')，现代浏览器不太需要这些保护， 但其仍然可以**为尚不支持  CSP(Content-Security-Policy)  的旧版浏览器的用户提供保护**

#### 用法：

- X-XSS-Protection: 0
- X-XSS-Protection: 1
- X-XSS-Protection: 1; mode=block
- X-XSS-Protection: 1; report=

#### 说明

- 0 禁止 XSS 过滤。
- 1 启用 XSS 过滤（通常浏览器是默认的）。 如果检测到跨站脚本攻击，浏览器将清除页面（删除不安全的部分）。
- 1;mode=block 启用 XSS 过滤。 如果检测到攻击，浏览器将不会清除页面，而是阻止页面加载。
- 1; report=  (Chromium only)启用 XSS 过滤。 如果检测到跨站脚本攻击，浏览器将清除页面并使用 CSP report-uri (en-US)指令的功能发送违规报告。

#### 配置建议

    add_header 'X-XSS-Protection' '1;mode=block';

#### 配置生效位置：

location 下 html 文件

## Strict-Transport-Security

（通常简称为 HSTS）是一个安全功能，它告诉浏览器只能通过 HTTPS 访问当前资源，而不是 HTTP。

#### 语法：

- Strict-Transport-Security: max-age=
- Strict-Transport-Security: max-age=; includeSubDomains
- Strict-Transport-Security: max-age=; preload

#### 说明

- max-age= 设置在浏览器收到这个请求后的秒的时间内凡是访问这个域名下的请求都使用 HTTPS 请求。
- includeSubDomains  可选如果这个可选的参数被指定，那么说明此规则也适用于该网站的所有子域名。
- preload  可选 查看   预加载 HSTS  获得详情。不是标准的一部分。

#### 配置建议

    add_header 'Strict-Transport-Security' 'max-age=31536000; includeSubDomains';

#### 配置生效位置：

location 下 html js css 等静态资源文件

### ---安全头配置总结---

- nginx 配置(前后台都在 nginx 中配置)

```shell
# ^~ /back/ 后台接口和 ~* \.[a-z0-9]+$前端资源都配置一致
add_header 'X-Frame-Options' 'SAMEORIGIN';
add_header 'X-Frame-Options' 'DENY';
add_header 'X-Content-Type-Options' 'nosniff';
add_header 'X-XSS-Protection' '1;mode=block';
add_header 'Strict-Transport-Security' 'max-age=31536000; includeSubdomains';
add_header 'Content-Security-Policy' "default-src 'self' 'unsafe-inline' 'unsafe-eval' blob:  data:;";
```
