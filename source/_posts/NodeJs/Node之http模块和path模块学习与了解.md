---
title: Node之http模块和path模块学习与了解
categories: 技术笔记
tags:
  - node
  - http
  - path
excerpt: Node之http模块和path模块学习与了解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200927215843.png'
articleLink: 8c990f12a9ff3227
date: 2020-09-27 21:52:32
---

## http模块

这是一个内置的模块 提供了一些http服务的功能

这里只做基本的功能介绍 不做详细的解释说明

### 创建个web服务器

1. 导入http模块：const http = require("http")；
2. 创建服务器：使用 const server = http.createServer() 创建服务器；
3. 绑定监听事件：通过 server.on('request', function(req, res) { 请求的处理函数 }) 绑定事件 并 指定 处理函数；
4. 启动服务器：通过 server.listen(端口, IP地址, 启动成功的回调函数) 来启动服务器；
5. res.end() 向客户端发送内容

```
//导入http核心模块
const http = require("http")
// 创建服务器
const server = http.createServer();
// 绑定request事件,监听客户端请求
server.on("request", function(req, res) {
    // end方法向客户端发送内容
    res.end("hello world 你好世界");
});
// 启动服务器
server.listen(3000, "127.0.0.1", function() {
    console.log("server running at http://127.0.0.1:3000");
});
```

然后运行我们就可以在浏览器进行访问 `http://127.0.0.1:3000/` 即可

### 响应中文内容乱码

在上面代码里面返回的 你好世界 这些中文字是乱码的状态

这时候我们就需要设置响应头部 `Content-Type`

```
// 设置头部
res.setHeader("content-type", "text/html;charset=utf-8")
```

### 路由

我们需要访问 `index` 的时候是首页 然后访问 `info` 的时候是个人信息的页面

也就是说 根据不同的URL返回不同的文本内容

我们需要使用 `req.url` 进行判断现在访问的到底是什么页面

```
server.on('request', function(req, res) {
		// 获取访问url
    const url = req.url
    // 防止中文乱码
		// 设置头部
    res.setHeader("content-type", "text/html;charset=utf-8")
    if (url === '/' || url === '/views/index.html') {
        res.end('首页')
    } else if (url === '/views/info.html') {
        res.end('个人信息')
    } else {
        res.end('请求的内容不存在！')
    }
})
```

这样我们就可以 访问不同的url 进入不同的页面了！

这时候我需要返回响应页面的 样式也就是html 就需要使用到 fs模块

我们通过使用 fs模块的读取信息进行 获取html里面的内容然后返回给我们的页面

```
// 读取html 文件
let indexData = fs.readFileSync('./moban01.html')
// 然后发送
res.end(indexData)
```

页面如果很大的话性能会有损失 那么我们可以使用流方式

```
//通过流方式
let infoData = fs.createReadStream("./info.html")
infoData.pipe(res)
```

引入js或者css的时候需要注意 mime 规则

什么意思呢 就是我一开始启动服务器的时候 设置的相应头 `content-type` 的值是  `text/html;charset=utf-8` 但是这个对css也设置这个相应头的话 css 文件就不会生效并且会给你报个警告

### path模块

我们可以引入path模块进行获取到文件后缀的拓展名

然后进行引入`mime`规则 设置 `content-type`

```
const path = require("path")
// 获取拓展名
let ext = path.extname(urlObj.pathname)
// 根据 mime 规则进行配置头部
res.setHeader("content-type", mime[ext])
```

#### path.extname(path)

返回 `path` 的扩展名 即 `path` 的最后一部分中从最后一次出现 `.`（句点）字符直到字符串结束

```
path.extname('index.html');
// 返回: '.html'
```

#### path.join([...paths])

会将所有给定的 `path` 片段连接到一起（使用平台特定的分隔符作为定界符）然后规范化生成的路径。

长度为零的 `path` 片段会被忽略。

如果任何的路径片段不是字符串，则抛出 `TypeError`。

```
path.join('目录1', {}, '目录2');
// 抛出 'TypeError: Path must be a string. Received {}'
```

path是一个对路径操作的模块 还有很多功能 可以去看看文档进行学习

这里你们会好奇这个 mime 是什么？

### mime

多用途互联网邮件扩展类型

是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。

总结来说：你得告诉浏览器是用什么方式去打开什么文件

在百科里面可以找到 类型大全也就是所有的类型的

是一个对象可以存到json里面然后用 `require`进行引入就可以直接转换成对象

所以我才可以 `mime[ext]`



[资源下载](https://wwa.lanzous.com/ipQMJgzc1ba)