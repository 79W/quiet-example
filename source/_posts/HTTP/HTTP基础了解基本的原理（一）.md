---
title: HTTP基础了解基本的原理（一）
categories: 技术笔记
tags:
  - HTTP
excerpt: HTTP基础了解基本的原理
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922193901.png'
articleLink: 5b22488fd8001917
date: 2019-08-17 08:44:19
---

## HTTP是什么

HTTP，又被称为超文本传输协议（http，hypertext transfer protocol)是互联网上应用最为广泛的一种网络协议。

所有的WWW文件都必须遵守这个标准。

设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。

简单的说：就是服务端与客户端的协议（买东西与卖东西的三包协议）

## HTTP协议历史与标准

### 0.9

0.9协议是适用于各种数据信息的简洁快速协议，但是远不能满足日益发展的各种应用的需要。0.9协议就是一个交换信息的无序协议，仅仅限于文字。由于无法进行内容的协商，在双发的握手和协议中，并有规定双发的内容是什么，也就是图片是无法显示和处理的。 

### 1.0

到了1.0协议阶段，也就是在1982年，TimBerners-Lee提出了HTTP/1.0。在此后的不断丰富和发展中，HTTP/1.0成为最重要的面向事务的应用层协议。该协议对每一次请求/响应建立并拆除一次连接。其特点是简单、易于管理，所以它符合了大家的需要，得到了广泛的应用。

### 1.1

在1.0协议中，双方规定了连接方式和连接类型，这已经极大扩展了HTTP的领域，但对于互联网最重要的速度和效率，并没有太多的考虑。毕竟，作为协议的制定者，当时也没有想到HTTP协议会有那么快的普及速度。 

关于HTTP1.1协议的具体内容可以参考RFC 2616。 

### 2.0

HTTP2.0的前世是HTTP1.0和HTTP1.1。虽然之前仅仅只有两个版本，但这两个版本所包含的协议规范之庞大，足以让任何一个有经验的工程师为之头疼。网络协议新版本并不会马上取代旧版本。实际上，1.0和1.1在之后很长的一段时间内一直并存，这是由于网络基础设施更新缓慢所决定的。 

关于HTTP2.0协议的具体内容可以参考RFC 7540。 

## HTTP请求与相应消息

浏览器这样的客户端发出的叫做请求（requests）

被服务端回应的消息叫做响应（responses）

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgsfc35NM4iZkYeQr-20200922193429333.png)

## HTTP的基本原理

了解就行 

http是应用层的 

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5HubkIxilanEwJO.png)

## 概述

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgAlcktCqrFsVIghv.png)

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgRTgdpDoskK7vtzn.png)

### 代理（Proxies）

1. 缓存（可以公开也可以私有的像浏览器的缓存）
2. 过滤（像反病毒扫描 家长控制）
3. 负载均衡（让多个服务器服务不同的请求）
4. 认证（对不同资源进行权限管理）
5. 日志记录（允许存储历史信息）

## Http能控制什么

1. 缓存（可以公开也可以私有的像浏览器的缓存）

2. 开放同资源限制

3. 认证（对不同资源进行权限管理）

   基本的认证可以直接通过http提供 或者cookies来设置指定的会话

4. 代理和隧道

   通常情况下 服务器和客户端都处于内网的 对外网隐藏真实ip地址 

   因此http请求就要通过代理越过这个网络屏障

5. 会话

## http流

1. 打开一个tcp链接 ：tcp链接被用来发送一条或者多条请求 以及接受回应消息

2. 发送一个http报文：

   ```
   GET /HTTP/1.1
   Host:www.baidu.com
   Accept-Language:fr
   ```

3. 读取服务器端返回的报文信息:

   ```
   HTTP/1.1 200 OK
   Last-Modified:Tue,01 Dec 2009 20:18:22 GMT
   Content-Length:276764
   Content-Type:text/html
   ```

4. 关闭连接或者为后续重新请求重用链接

