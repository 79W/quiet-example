---
title: node.js服务端express模块的中间件和跨域问题
categories: 技术笔记
tags:
  - node
  - 学习笔记
  - express
excerpt: express服务端的web框架学习中间件和跨域的问题
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921221118.png'
articleLink: 0814f08bb66b1805
abbrlink: 37083
date: 2020-07-05 14:19:18
---

## 中间件

*中间件*功能是可以访问请求对象（`req`），响应对象（`res`）和`next`应用程序的请求-响应周期中的功能的功能。该`next`功能是Express路由器中的功能，当调用该功能时，将在当前中间件之后执行中间件。

中间件功能可以执行以下任务：

- 执行任何代码。
- 更改请求和响应对象。
- 结束请求-响应周期。
- 调用堆栈中的下一个中间件。

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgCiZvS98tFdl35IN.png)

```js
//导入包
var express = require('express')
//创建服务器
var app = express()

//中间件
app.use((req, res, next)=> {
	console.log('LOGGED')
	req.qiaoyue = "乔越"
	next()
//	如果当前的中间件功能没有结束请求-响应周期，则必须调用next()将控制权传递给下一个中间件功能。否则，该请求将被挂起
})
//什么是中间件
//就是服务器开启之后和路由之前执行的一个函数
//这个函数 是可以操作 req res的 
//next()是去执行一个的函数

//路由
app.get('/', function (req, res) {
	console.log(req.qiaoyue)
	res.send('Hello World!')
})
//开启服务器
app.listen(5279,()=>{
	console.log("ok....")
})
```

## 跨域问题

###  什么是跨域

1. 浏览器使用 `ajax` 时，如果请求的**接口地址**和**当前打开的页面地址不同源**称之为跨域。

   协议和地址、端口都一样成为同源。有一个不同则为不同源。

2. 同源与不同源的意义

   浏览器安全策略。

### 解决跨域问题

#### 设置相应头 允许资源被访问

只需要在响应头处设置 `Access-Control-Allow-Origin` 为 `*` 即可。

```js
res.setHeader("Access-Control-Allow-Origin", "*");
//意思就是我允许被访问
```

#### 在中间件设置允许跨域

```js
app.use((req, res, next) => {
    // 设置响应头，允许资源被访问、共享
    res.setHeader("Access-Control-Allow-Origin", "*");
    next();
});
//因为是中间件会在路由前面被执行所有 可以设置在中间件内
```

#### 模块解决跨域问题

##### cors

这个和中间件的解决的方案效果是一模一样的

```js
const cors = require("cors");
app.use(cors());
```

### Jsonp

通过动态创建 `script` 标签，通过 `script` 标签的 `src` 请求没有域限制来获取资源

例如在 html 页面中，将 `script` 标签地址改为后端接口。

```js
app.get("/all", (req, res) => {
  console.log(Date.now() - req.requestTime);
  res.send("console.log('哈哈')");
});
```

```html
<script src="http://127.0.0.1:5279/all"></script>
```

那么当服务器开启时，我们就会看到返回来的内容会当作 `JavaScript` 代码执行。

因此此方式需要与前端进行配合才可使用。

例如前端通过 get 参数方式将函数名传递给后端。

```html
<script>
    function fn() {
        console.log('这是事先准备好的函数！')
    }
    function fn1(backData) {
            console.log(backData)
        }
</script>
<script src="http://127.0.0.1:5279/all?callback=fn"></script>
```

```js
app.get("/all", (req, res) => {
  let fn = req.query.callback;
  res.send(`fn()`);
});
app.get("/all", (req, res) => {
  let fn = req.query.callback;
  res.send(`${fn}({'name':'haha','price':100})`);
});
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgBVFlx8agroiOHCm.png)

#### ajax请求jsonp

如果这个接口api支持jsonp那么就可以使用ajax请求jsonp

jquery使用jsonp 只需要一个参数 `dataType: 'jsonp'`

 本质就是 自动给你添加一个 script 标签

```js
$.ajax({
    url: 'http://127.0.0.1:5279/all',
  //前台就是 接口api 支持 jsonp
    dataType: 'jsonp',
    success: function (backData) {
        console.log(backData)
    }
})
```
