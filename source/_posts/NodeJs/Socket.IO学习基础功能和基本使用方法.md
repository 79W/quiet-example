---
title: Socket.IO学习基础功能和基本使用方法
categories: 技术笔记
tags:
  - Mysql
  - Node
excerpt: Socket.IO学习基础功能和基本使用方法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201003002359.png'
articleLink: 252a807135263303
date: 2020-10-03 09:06:33
---

## Socket.IO

Socket.IO 由两部分组成：

- 一个服务端用于集成 (或挂载) 到 Node.JS HTTP 服务器： `socket.io`
- 一个加载到浏览器中的客户端： `socket.io-client`

开发环境下， `socket.io` 会自动提供客户端。正如我们所见，到目前为止，我们只需要安装一个模块：

```
npm install socket.io
```

这会安装模块并添加依赖到 `package.json`在 `index.js` 文件中添加该模块：

```
var app = require('express')();
var http = require('http').Server(app);
var io = require('socket.io')(http);
 
app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

我们通过传入 `http` （HTTP 服务器） 对象初始化了 `socket.io` 的一个实例。

 然后监听 `connection` 事件来接收 sockets， 并将连接信息打印到控制台。

在 index.html 的 `</body>` 标签中添加如下内容：

```
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();
</script>
```

这样就加载了 `socket.io-client`。 socket.io-client 暴露了一个 `io` 全局变量，然后连接服务器。

请注意我们在调用 `io()` 时没有指定任何 URL，因为它默认将尝试连接到提供当前页面的主机。

重新加载服务器和网站，你将看到控制台打印出 “a user connected”。

每个 socket 还会触发一个特殊的 `disconnect` 事件：

```
io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});
```

### 触发事件

Socket.IO 的核心理念就是允许发送、接收任意事件和任意数据。任意能被编码为 JSON 的对象都可以用于传输。

二进制数据 也是支持的。

这里的实现方案是，当用户输入消息时，服务器接收一个 `chat message` 事件。`index.html` 文件中的 `script` 部分现在应该内容如下：

```
 
<script src="/socket.io/socket.io.js"></script>
<script src="https://code.jquery.com/jquery-1.11.1.js"></script>
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
  });
</script>
```

在 `index.js` 中打印出 `chat message` 事件：

```
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    console.log('message: ' + msg);
  });
});
```

### 广播

接下来的目标就是让服务器将消息发送给其他用户。

要将事件发送给每个用户，Socket.IO 提供了 `io.emit` 方法：

```
io.emit('some event', { for: 'everyone' });
```

要将消息发给除特定 socket 外的其他用户，可以用 `broadcast` 标志：

```
io.on('connection', function(socket){
  socket.broadcast.emit('hi');
});
```

为了简单起见，我们将消息发送给所有用户，包括发送者。

```
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    io.emit('chat message', msg);
  });
});
```

在客户端中，我们捕获 `chat message` 事件，并将消息添加到页面中。现在客户端代码应该如下：

```
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
    socket.on('chat message', function(msg){
      $('#messages').append($('<li>').text(msg));
    });
  });
</script>
```

就这样，聊天应用就完成了，大约只有 20 行代码！

我们用Koa写一下

### Koa结合Socket.IO

**服务端**

```
const Koa = require("koa");
const static = require("koa-static");
const Router = require("koa-router");
let app = new Koa();
let router = new Router();
app.use(static(__dirname+"/static"));
const server = require("http").createServer(app.callback());
const io = require("socket.io")(server);
router.get("/",ctx=>{
    ctx.body = "some value...";
});
app.use(router.routes());
io.on("connection",(socket)=>{
    console.log("有socket连接");
    // socket.on("testFn",function(data){
    //     console.log(data);
    //     // socket.emit("clientFn",data); //发送给一个用户
    //    setTimeout(() => {
    //      socket.broadcast.emit("clientFn",data); //广播给出了自己之外的所有用户；
    //    }, 2000);
       
    // })
    socket.on("addData",function(data){
        console.log(data);
        socket.broadcast.emit("addInputData",data);
    })
})
server.listen(3000);
```

客户端

```
<script>
    let socket = io.connect("/");
    // socket.emit("testFn","我是客户端发送的内容");
    // socket.on("clientFn",function(data){
    //     console.log("接收到服务端的数据 :",data);
    // })
    document.querySelector(".btn").onclick = function(){
        let val = document.querySelector(".input-frame").value;
        socket.emit("addData",val);
        let ul = document.querySelector(".chat-show");
        let li = document.createElement("li");
        li.innerHTML = val;
        ul.appendChild(li);
    }
    socket.on("addInputData",function(data){
        // console.log(data);
        let ul = document.querySelector(".chat-show");
        let li = document.createElement("li");
        li.innerHTML = data;
        ul.appendChild(li);
    })
</script>
```

简单了解使用方法更多的Api 请查看官方文档