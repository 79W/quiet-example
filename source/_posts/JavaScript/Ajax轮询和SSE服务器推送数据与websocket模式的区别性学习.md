---
title: Ajax轮询和SSE服务器推送数据与websocket模式的区别性学习
categories: 技术笔记
tags:
  - Ajax
  - SSE
  - websocket
excerpt: 学习Ajax轮询和SSE服务器推送数据与websocket模式对三种模式的方法进行一个区别和好处与坏处的区分进行更好的解决方案
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201003003012.png'
articleLink: d22a754ad4d43302
date: 2020-10-02 23:16:33
---

我们试想一下我们做个实时聊天的窗口有几种方法？

在我们不刷新页面并且可以试试更新页面内容的方法 你这时候是不是想到了ajax没错确实可以

## Ajax轮询

什么是轮询？顾名思义就是我轮着问你，规定一个时间然后我就问你 有新数据了吗？ 有新数据了吗？ 有新数据了吗？ 当有新数据的时候就更新页面。

但是性能会很差。。。

并且这是前台操作的 后端只需要查询数据库 设置好路由即可

**Node后端**

简单的说就是 后端设置一个路由然后进行查询数据获取想要的数据的操作

```
const Koa = require("koa");
const static = require("koa-static");
const Router = require("koa-router");
const mysql2 = require("mysql2");
const koaBody = require("koa-body");
let app = new Koa();
app.use(static(__dirname+"/static"));
app.use(koaBody());
//连接数据库
const connection = mysql2.createConnection({
    host:"localhost",
    user:"root",
    password:"123",
    database:"js04"
})
let router = new Router();
//访问此路由进行获取数据
router.get("/getData",async ctx=>{
  let [rows] = await connection.promise().query("SELECT * FROM message");
  ctx.body = rows;
})

app.use(router.routes());
app.listen(3000);
```

**前端**

前端我要访问这个路由然后获取数据  而ajax 是不会刷新页面的。

```
$.ajax({
		//访问路由
    url: "/getData",
    success(res) {
    //res 就是回的数据
        console.log(res);
    }
})
```

怎么进行轮询呢？

上面我们说什么 规定一个时间然后 每隔多长时间进行询问

那么这不就是定时器吗

我们将上面的ajax进行封装成方法里面 然后用定时器进行 轮询访问获取数据

```
    setInterval(() => {
        render();
    }, 500);
   
```

这样我们是服务器资源很浪费会一直的进行访问api进行获取数据 

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201003002711.png)

我们说了 客户端不断的向服务端进行询问那么有没有 服务端主动向 客户端进行发送消息的呢 ？

##  SSE (server send event) 服务器推送数据

其是基于http协议的，本质上是保持一个http长连接，客户端向服务端发送请求，在浏览器与服务器建立连接之后，等有数据更新后，服务端向浏览器主动发送消息。这样可以减少数量，减少服务器压力。

而在我们服务端使用sse的时候要进行一些设置

- 设置头部

  `"Content-type","text/event-stream"`

- 返还数据格式

  ​	`data:`  声明数据开始

  ​	`\r\n\r\n ` 标志数据结尾

**服务器端Node**

我们设置两个路由一个是返回首页 第二个路由是进行一些数据的返回也就是使用sse

```
const http = require("http");
const fs = require("fs");
// res.write();  res.end();
let server = http.createServer((req,res)=>{
    let url = req.url;
    //路由返回页面
    if(url=="/"){
        let data =  fs.readFileSync("index.html");
        res.end(data);
    }else if(url=="/sse"){
        res.setHeader("content-type","text/event-stream;charset=utf-8");
        // 服务端端定时推送数据到客户端；
        setInterval(() => {
            res.write("data:时间是"+new Date()+"\r\n\r\n");
        }, 1000);
    }
})
server.listen(4000);

```

**客户端**

我们需要创建一个 `EventSource`对象 然后在这个对象里面有几个事件

1. open：当成功建立连接时产生
2. message：当接收到消息时产生
3. error：当出现错误时产生

```
let source = new EventSource("/sse");
source.onopen = function(){
    console.log("连接成功....");
    console.log(source.readyState);
    //这里的 readyState 会有几种状态
    //- 0 CONNECTING (0) 
    //- 1 OPEN (1)
    //- 2 CLOSED (2)
}
source.onmessage = function(e){
    console.log("获取到的数据是：",e.data);
    document.querySelector(".exchange").innerHTML = e.data;
}
source.onerror = function(err){
    return console.log(err);
}
```

这里的 readyState 会有几种状态

- CONNECTING (0) 
- OPEN (1)
- CLOSED (2)

以上的两种方法虽然都能实现但是对性能都不是比较的友好 有没有更好的解决方案呢？

## websocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议

浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

我们后端使用的是 Node 所以我们需要借助 ws 模块创建 websocket 实例

ws是一种易于使用，快速且经过全面测试的WebSocket客户端和服务器实现的方法。

### 安装

```
npm install ws
```

### 简单使用

首先我们需要导入ws并调用Server 服务端

我们进行设置端口

**服务端**

```
var WebSocketServer = require('ws').Server,
wss = new WebSocketServer({ port: 8181 });
wss.on('connection', function (ws) {
    console.log('client connected');
    ws.on('message', function (message) {
      	//监听接收的数据
        console.log(message);
    });
  	// setInterval(() => {
        let somedata = {
            name:"张三",
            age:20
        }
        //发送数据
        ws.send(JSON.stringify(somedata));
    // }, 1000);
});
```

**客户端**

建立握手

```
var ws = new WebSocket("ws://localhost:8181");
```

打开协议

```
ws.onopen = function () {}
```

发送数据到服务端

```
ws.send("客户端数据");
```

关闭协议:关闭协议后不能发送数据

```
ws.close();
```

接收消息

```
ws.onmessage = function(e){
 		// console.log(e.data);
}
```

完整点的代码

```
// 握手协议；
var ws = new WebSocket("ws://localhost:8181");
ws.onopen = function(){
    console.log("连接成功....");
}
ws.onmessage = function(e){
    // console.log(e.data);
    console.log(JSON.parse(e.data));
}
function sub(){
    ws.send("发送到服务端的数据");
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201003002756.png)

整体就是简单的了解客户端与服务端之间的交互问题后面会更加详细的进行学习