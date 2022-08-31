---
title: node服务端解决socket.io跨域问题express,koa
categories: 技术笔记
tags:
  - node
  - socket
  - express
  - koa
  - 跨域
  - cors
excerpt: 分别使用koa,express搭建socket.io服务端并解决跨域问题
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201204152101.png'
articleLink: 62a686f9
date: 2020-12-04 15:00:15
---

### 开始

我们今天主要记录下 `socket.io`跨域的问题

我们服务端采用 `node`分别使用 `express`,`koa` 搭建

首先我们下载 `socket.io`

```
$ npm i socket.io
```

首先是 `express` 搭建

```
// 导入express 
const express = require("express")
const app = express()
// 开启socket服务
let server = app.listen(12345)
// socket 初始化
const io = require("socket.io")(server)
// 监听
io.on("connection", (socket) => {
    console.log("有人链接")
})
app.listen(3322, () => {
    console.log("服务开启成功。。。。")
})
```

然后我们使用 `koa` 搭建一下

```
// 引入依赖
const koa = require("koa");
// 初始化koa
const app = new koa();
// 开启 http 
var server = require("http").createServer(app.callback());
// 初始化 socket
const io = require("socket.io")(server);
// 监听
io.on("connection", (socket) => {
    console.log("有人链接");
});
server.listen(3000);
```

然后服务端我们就搭建好了

我们来搭建客户端页面

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/3.0.3/socket.io.js"></script>
    </head>
    <body></body>
    <script>
    		// 链接服务
        let socket = io('ws://localhost:5522');
        // 正常 如果正常链接 后端会打印有人链接
    </script>
</html>
```

### 问题

![socket跨域](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201204150917562.png)

```
Access to XMLHttpRequest at 'http://localhost:3000/socket.io/?
EIO=4&transport=polling&t=NOibQzP' 
from origin 'http://127.0.0.1:5501'
has been blocked by CORS policy: 
No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

会报这个错误 （跨域问题） 我们通常的解决方案是什么呢？

拿koa来说 你肯定会安装一个`koa-cors` 中间件来解决跨域 但是这只能解决你路由数据的跨域 

并不能解决 `socket.io` 的跨域 

### 解决方案

我们需要在我们初始化 `socket.io` 的时候 加上一句`cors:true` 即可

`express` 

```
// socket 初始化
const io = require("socket.io")(server, { cors: true })
```

`koa`

```
// 初始化 socket
const io = require("socket.io")(server, { cors: true });
```

我们在来看后台控制它是否打印了 “有人链接了” 的字样

![socket.io跨域](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201204151513801.png)

