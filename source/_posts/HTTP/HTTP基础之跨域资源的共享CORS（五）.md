---
title: HTTP基础之跨域资源的共享CORS（五）
categories: 技术笔记
tags:
  - HTTP
  - 跨域
  - CORS
excerpt: HTTP基础之跨域资源的共享CORS
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922195707.png'
articleLink: d8d8d9b800c01919
date: 2019-08-19 16:42:19
---

## 预检请求

1. 下列请求：PUT,DELETE,CONNECT,OPTIONS,TRACE或PATH
2. 不得认为设置：Accept ，accept-language content-language content-type
3. Content-type 值仅限于下面
   1. text/plain
   2. multipart/form-data
   3. application/x-www-form-urlencoded
4. 需要先用options方法发生一个预检请求到服务器以获取服务器是否允许该实际请求
5. 可以避免跨域请求对服务器的用户数据产生未预期的影响

## 请求消息

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgzcZfesQ3dnHUTlV.png)

1. 首部Access-Control-Request -method 表示post方法
2. 首部Access-Control-Request -Header 表示该请求携带 x-pingother首部字段



## 响应消息

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img745zm2bCY9tEUvu.png)

1. 首部Access-Control-Allow -methods 表示允许 post get options请求方法
2. 首部Access-Control-allow -Headers 表示允许请求携带 x-pingother首部字段
3. 首部Access-Control-Max -Age 表示设置相应有效时间为 1728000秒 

 

## 认证请求是什么

```
xmlHTTPRequest.withCredentials = True
//向服务器发送cookie 
//返回
Access-control-allow-credentials：true
//如果未携带将不会把响应内容返回给请求的发送者
```

## CORS是什么

被称为 跨域资源共享 允许服务器声明那些原站有权限访问那些资源

1. 简单请求
   1. get head post
   2. 不得人为设置Accept ，accept-language content-language content-type
   3. Content-type 值仅限于下面
      1. text/plain
      2. multipart/form-data
      3. application/x-www-form-urlencoded
   4. 没有正常响应则请求方是不会收到任何数据 因此那些不允许跨域请求的网站无需为这一新的http访问控制担心
2. 预检请求
3. 认证请求

### 请求消息

首部字段 Origin 表示请求来源于那些网站 ip 域名

### 响应请求

Access-Control-Allow-Origin 使用Origin 就可以完成最简单的访问控制

```
Access-Control-Allow-Origin:*
//为允许全部
```

##  CORS预请求

### 允许的方法

GET HEAD POST

### 允许Content-type

```
text/plain
multipart/form-data
application/x-www-form-urlencoded
```

### 请求头限制

XMLHttpBequestUpload 对象均没有注册任何事件监听器

### 服务端进行头设置

```
' Access -Control-Allow-Origin' : '*',
' Access-Control-Allow-Headers':'X-Test-Cors',
' Access-Control-Allow-Methods': 'POST, PUT，Delete',
' Access-Control-Max-Age': '1000'
```

### 返回内容

```
'Content-type':'text/plain'
```

请求是已经发送到服务端了 只是浏览器看到没有这个`Access -Control-Allow-Origin`允许的话浏览器就给返回结果屏蔽了并在控制台给你报错。

### jsonp

1. jsonp的核心原理就是目标页面回调本地页面的方法,并带入参数

2. 我们常用的动态页面有jsp,php,aspx

3. 原理

   在网页里,你如果引入其他网页的js,那这个页面的js是可以调用你网页的代码的

   直接请求js 和 请求的动态页面(jsp,php,aspx)里输出的javascript代码 效果一样

#### 用jquery的ajax请求jsonp

```js
$.ajax({
     url:'',
     type:"GET",
            dataType:"jsonp",
            jsonp: false, 
            jsonpCallback: "showjson", //这里的值需要和回调函数名一样
            success:function(data){
                console.log("Script loaded and executed.");
            },
            error:function (textStatus) { //请求失败后调用的函数
                console.log(JSON.stringify(textStatus));
            }
});         
```

### http长连接

HTTP1.1规定了默认保持长连接（HTTP persistent connection ，也有翻译为持久连接），数据传输完成了保持TCP连接不断开（不发RST包、不四次握手）

```
Connection : keep-alive
keep-alive : timeout = 20
```

### http缓存 Cache-Control

```js
 if (request.url === '/script.js' ){
 		response. writeHead(200,{
 			'Content-Type': 'text/javascript'
   		'Cache-Control': 'max-age=20'
 		})
 		response.end("console. log("script loaded twice"))
 }
```

#### 可缓存性

```
public 任何都可以进行访问缓存
private 
no-cache 任何都不可以访问缓存
```

#### 到期

```
max-age = <seconds>
s-maxage = <seconds>
max-stale = <seconds> //浏览器用不到的
```

#### 重新验证

```
must-revalidate
proxy-revalidate
```

#### 其他

```
no-store 永远要去服务器拿
no-transform 不要随便改动服务器返回的内容
```

##  Content-Security-Policy 安全策略

限制资源获取 报告资源获取权限 

### 限制方式

​	default-src限制全局

​	资源类型有：connect-src、mainfest-src、img-src、font-src、media-src、style-src、frame-src、script-src

#### meta头也可以进行限制

```
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">
```

详细介绍：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP

## 数据协商

### Accept

请求头用来告知（服务器）客户端可以处理的内容类型

```
Accept
Accept-Encoding
Accept-Language
User-Agent
```

### Content

在响应中，Content-Type标头告诉客户端实际返回的内容的内容类型。

## 资源验证

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgOk8sNaVRCJLntUz-20200922195951787.png)

### Last-Modified 上次修改事件

配合If-Modified-Since 或者 If-Unmodified-Since使用

对比上次修改时间已验证资源是否需要更新

验证是否使用缓存

### Etag 数据签名

配合If-Match 或者 If-Non—Match使用

对比这个签名来判断是否需要修改 

## Cookie

通过 Set-Cookie设置保存到浏览器中的数据 下次请求会带上 

max-age 设置过期时间 

域名设置 是不可以跨域的 比如设置的是 a.text.com。那么就只有在这个域名下才可以访问 比如你设置的是 text.com 那么这下面的所有二级域名都可以进行访问cookie

### 302重定义

```
response.writeHead(302,{
	'Location':'/new'
})
```

只有定义为302才进行跳转 临时的

302:是临时定义 总会先访问在跳转

301:是永久的 也就是第一次访问是 先访问在跳转 第二次及以后是直接跳转