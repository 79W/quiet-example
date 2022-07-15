---
title: HTTP基础之消息类型消息结构报文（二）
categories: 技术笔记
tags:
  - HTTP
  - 消息结构
excerpt: HTTP基础之消息类型消息结构报文请求头
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922193901.png'
articleLink: 0fefb65819e82917
date: 2019-08-17 14:22:29
---

## MIME类型

MIME(Multipurpose Internet Mail Extensions)多用途互联网邮件扩展类型。

![image-20200817142722141](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200817142722141.png)

## HTTP/2.0

### http/1.x报文有一些性能上的缺点

1. 消息头不像消息主体一样会被压缩
2. 两个报文之间的header通常非常相似 但他们仍然在连接中重复传输
3. 无法复用 当同一个服务器打开几个连接时 tcp热连接比冷连接更加优秀

HTTP/2.0引入了一个额外的步骤 他将http/1.x消息分成帧并嵌入到流中

## 请求头

请求允许客户端向服务器端传递附加信息。请求头由名称（不分区大小写）后跟一个冒号后跟具体的值（不带换行符）组成。

1. 通用头：同时适用于请求和响应信息 但与最终消息主体中传输的数据无关的消息头
2. 请求头：包含更多有关要获取对资源或客户端本身消息的消息头
3. 实体头：包含有关实体主体的更多信息 

## 请求主体

请求消息的最后一部分时请求主体

1. GET HEAD DELETE OPIONS 不需要请求主体
2. POST （包含HTML表单数据）
   1. 单一资源主体：由一个单文件组成该请求主体header定义：Content-Type和Content-Length
   2. 多资源主体：由多部分组成请求主体 每一部分包含不同的信息位

## 起始行

1. 请求方法：执行的动作

   ![image-20200817145734876](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200817145734876.png)

2. 请求地址：url 或者服务器ip+端口

3. HTTP版本：定义了剩余报文的结构 作为对期望的响应版本的指示符

   ```
   GET /index.html HTTP/1.1
   ```

## 响应头

1. 通用头：适用于请求和响应的消息
2. 响应头：包含响应补充信息 如其位置或者服务器本身的消息头
3. 实体头：主体信息

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgMvFkfeJlnapycjb.png)

## HTTP报文

客户端与服务端的数据交换的方式

### 请求

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img4sL6PGV5Dyov7UC.png)

### 响应

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgDf56kwGOWBli2S7.png)

## HTTP消息结构

### 请求头

1. start line：一行起始行用于描述要执行的请求 或者是对应的状态 成功或失败
2. HTTP headers ：一个可选的http头集合指明请求或来描述消息正文

### 消息正文

1. empty line：一个空行 有关于请求的元数据已发送完毕
2. body：一个可选的包含请求相关数据的正文 响应相关的文档 大小有起始行发http头来指定

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgnOlK5WhM134fVDZ.png)

## 状态行

1. 协议版本 通常为HTTP/1.1

2. 状态码：表明请求是成功或是失败的

3. 状态文本：一个简短的纯粹的信息 通过状态码来表述

   ```
   HTTP/1.1 200 OK
   ```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgwBI5aRYZuLrdF8k-20200922194317071.png)

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgVGAu9STCwYx783g-20200922194333085.png)

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgzgvPdtcOwoLC137-20200922194352437.png)

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgubJgpBvcnXRCAo7-20200922194401064.png)