---
title: HTTP基础之cookie的理解（四）
categories: 技术笔记
tags:
  - HTTP
  - cookie
excerpt: HTTP基础之cookie的理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922194900.png'
articleLink: edf5ccbef2751918
date: 2019-08-18 18:23:19
---

## Set-Cookie 响应头

向用户代理（一般是浏览器）发送Cookie信息 

```
Set-Cookie: <cookie名> = <cookie值>
HTTP/1.0 200 OK
Content-type:text/html
Set-Cookie:yummy_cookie = choe
GET /index.html HTTP/1.1
Host:www.baidu.com
Cookie:yu = coo
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgr9cjVJU4Y76aWwg.png)

## Cookie请求头

服务器通过头部告知客户端保持cookie信息

## Cookie是什么

服务器发送到用户浏览器并保存在本地的一小块数据 再次发生会被连接给服务器再次发生会携带cookie 可以告知服务器两次请求是否来自一个人

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgNpAmTe5WOoP4fGy.png)



## Cookie的作用域

Domain和Path标识定义了cookie的作用域 即cookie应该发送给那些url

Domain标识制定了那些主机可以接受cookie

1. 如果不指定 默认为当前文档的主机
2. 如果指定了则一般包含子域名 

Path 表识指定了主机的那些路径可以接受cookie（该url路径必须在于请求url）以符号%x2f（‘/’）作为路径分隔符，子路径也会被匹配

## Cookie的有效期

Max-Age和Expires标识定义了Cookie的有效期，即Cookie的生命周期。 

1. 会 话 期 Cookie

   会话期Cookie是最简单的Cookie。

   则览器关闭之后Cookie会被自动删除，也就是说Cookie仅在会话期内有效。

   会话期Cookie不需要指定过期时间(Expires)或者有效期(Max-Age)。 

2. 持久性Cookie

   持久性Cookie可以指定一个特定的过期吋间(Expires)或有效期(Max-Age) 

## Cookie的应用

1. 会话管理
2. 个性化设置
3. 浏览器行为跟踪

## 创建Cookie

js可以使用`document.cookie`属性来访问和更新cookie

```
document.cookie = newcookie
// newcookie 为字符串形式
//这个方法只能改一个
```

## 读取Cookie

js可以使用`document.cookie`属性来访问和更新cookie

```
allcookie = document.cookie
```

allcookie 被给一个字符串的值 该字符串包含全部cookie 每条 key = value

```
//用分号；分割得到数组然后打印出来
```

## 删除Cookie

删除cookie只需要设置expires标识为以前的时间

```
document.cookie = "expires = Thi,01 Jan 1970.  GMT"
```

注意删除的是 当删除时不必指定cookie的值