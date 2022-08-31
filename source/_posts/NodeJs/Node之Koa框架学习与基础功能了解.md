---
title: Node之Koa框架学习与基础功能了解
categories: 技术笔记
tags:
  - node
  - Koa
excerpt: Node之Koa框架学习与基础功能了解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200928151032.png'
articleLink: f1cb90ce1b003228
date: 2020-09-28 09:52:32
---

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

总结就是 只给你封装了服务器的功能然后你想用其他功能就需要安装相关的插件 这样就是你你不用的东西就不需要安装更加的干净不会特乱 并且给你更好的处理错误

## 安装

```
npm i koa
```

必须经过的一步 `hello world`

```
//倒入 koa 模块
const Koa = require('koa');
//声明对象
const app = new Koa();
// 声明中间件
app.use(async ctx => {
  ctx.body = 'Hello World';
});
//开启服务端口
app.listen(3000);
```

koa是包含一组中间件函数的对象

可以将app.use里的函数理解成中间件

## async

Koa 调用“下游”，然后控制流回“上游”。

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200928112140.png)

当请求开始时首先请求流通过 `x-response-time` 和 `logging` 中间件 然后继续移交控制给 `response` 中间件

当一个中间件调用 `next()` 则该函数暂停并将控制传递给定义的下一个中间件

当在下游没有更多的中间件执行后 堆栈将展开并且每个中间件恢复执行其上游行为

```
const Koa = require('koa');
const app = new Koa();

// logger

app.use(async (ctx, next) => {
	//有这个后 去执行下一个 中间件
  await next();
  //如果下一个中间件没有 next() 那么就开始执行下面这两句代码
  const rt = ctx.response.get('X-Response-Time');
  console.log(`${ctx.method} ${ctx.url} - ${rt}`);
});

// x-response-time

app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx.set('X-Response-Time', `${ms}ms`);
});

// response

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

## app.use(function)

将给定的中间件方法添加到此应用程序。`app.use()` 返回 `this`, 因此可以链式表达.

```
//这里的middleWare函数就是一个中间件
let middleWare = async (ctx,next)=>{
    console.log("first middleWare");
    ctx.body = "hello world";
}
app.use(middleWare);
```

总结就是说我写一个方法让 用 use 添加到这个程序里面

## 错误处理

要执行自定义错误处理逻辑，如集中式日志记录，您可以添加一个 “error” 事件侦听器：

```
app.on('error', err => {
  	log.error('server error', err)
});
```

如果 req/res 期间出现错误，并且 _无法_ 响应客户端，`context`实例仍然被传递：

```
app.on('error', (err, ctx) => {
  log.error('server error', err, ctx)
});
```

当发生错误并且仍然可以响应客户端时，也没有数据被写入 socket 中，Koa 将用一个 500 “内部服务器错误” 进行适当的响应。

## Context

Koa Context 将 node 的 `request` 和 `response` 对象封装到单个对象中，为编写 Web 应用程序和 API 提供了许多有用的方法。

```
app.use(async ctx => {
  ctx; // 这是 Context
  ctx.request; // 这是 koa Request
  ctx.response; // 这是 koa Response
});
```

 例如 `ctx.type` 和 `ctx.length` 委托给 `response` 对象

`ctx.path` 和 `ctx.method` 委托给 `request`对象

### Api

`Context` 具体方法和访问器.

#### ctx.req

Node 的 `request` 对象.

#### ctx.res

Node 的 `response` 对象.

绕过 Koa 的 response 处理是 **不被支持的**.

避免使用以下 node 属性：

- `res.statusCode`
- `res.writeHead()`
- `res.write()`
- `res.end()`

#### ctx.request

koa 的 `Request` 对象.

#### ctx.response

koa 的 `Response` 对象.

#### ctx.throw([status], [msg], [properties])

用来抛出一个包含 `.status` 属性错误的帮助方法，其默认值为 `500`。

```
ctx.throw(400, 'name required');
```

等效于

```
const err = new Error('name required');
err.status = 400;
err.expose = true;
throw err;
```

**koa对Request和Response进行了别名对处理 也就是可以直接调用 和 Request与Response 效果一样**

## 响应(Response)

Koa `Response` 对象是在 node 的原生响应对象之上的抽象，提供了诸多对 HTTP 服务器开发有用的功能。

### Api

#### response.header

响应头对象。

#### response.socket

响应套接字。 作为 `request.socket` 指向 net.Socket 实例。

#### response.status

获取响应状态。默认情况下，`response.status` 设置为 `404` 而不是像 node 的 `res.statusCode` 那样默认为 `200`。

#### response.status=

通过数字代码设置响应状态 可以查阅这个状态列表随时更正.

```
ctx.response.status = 200;
```

#### response.message

获取响应的状态消息. 默认情况下, `response.message` 与 `response.status` 关联.

#### response.body

获取响应主体。

#### response.get(field)

不区分大小写获取响应头字段值 `field`。

```
const etag = ctx.response.get('X-RateLimit-Limit');
```

#### response.set(field, value)

设置响应头 `field` 到 `value`:

```
ctx.set('Cache-Control', 'no-cache');
```

#### response.type

获取响应 `Content-Type`, 不含 "charset" 等参数。

```
const ct = ctx.type;
// => "image/png"
```

#### response.redirect(url, [alt])

执行 [302] 重定向到 `url`.

要更改 “302” 的默认状态，只需在该调用之前或之后给 `status` 赋值。

```
ctx.status = 301;
ctx.redirect('/cart');
ctx.body = 'Redirecting to shopping cart';
```

## 请求(Request)

Koa `Request` 对象是在 node 的 原生请求对象之上的抽象，提供了诸多对 HTTP 服务器开发有用的功能。

### Api

#### request.header

请求头对象。

#### request.header=

设置请求头对象。

#### request.method

请求方法。

#### request.url

获取请求 URL.

#### request.path

获取请求路径名。

#### request.query

获取解析的查询字符串, 当没有查询字符串时，返回一个空对象。请注意，此 getter 不支持嵌套解析。

例如 "color=blue&size=small":

```
{
  color: 'blue',
  size: 'small'
}
```



## 常用中间件了解

### koa-router

是匹配url到相应处理程序的活动。

简单的说就是你前台访问什么url这个进行一个匹配然后执行相应的代码

#### 安装

```
npm i koa-router
```

#### 使用

```
const Koa = require("koa");
const Router = require("koa-router");
let app = new Koa();
let router = new Router();
//路由
router.get("/",async (ctx,next)=>{
    ctx.body = "首页";
});
router.get("/info",async (ctx,next)=>{
    ctx.body = "个人信息";
});
//注册中间件
app.use(router.routes());
app.listen(8888);
```

#### Restful

路由这里推荐使用Restful架构Api

Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。基于这个风格设计可以更简洁，更有层次;

条件：

- 程序或者应用的事物都应该被抽象为资源
- 每个资源对应唯一的URI(uri是统一资源标识符)
- 使用统一接口对资源进行操作
- 对资源的各种操作不会改变资源标识
- 所有操作都是无状态的

### koa-views

用于加载html模板文件 也就是加载静态文件使用

#### 安装

```
npm i koa-views
```

#### 使用

```
const Koa = require("koa");
const Router = require("koa-router");
const views = require("koa-views");
let app = new Koa();
let router = new Router();
// 注册 模版中间件
app.use(views(__dirname+"/views"),{
// 这里选择模版引擎
    extension:"pug"
});
//这是路由
router.get("/index",async (ctx,next)=>{
// 这里就可以直接返回 index.pug 模版了
    await ctx.render("index.pug");
});
//注册路由中间件
app.use(router.routes());
app.listen(8888);
```

总结：将模版文件放在一个文件夹里面然后利用 koa-views 插件进行配置中间件  后期在路由内可以直接返回 你在文件内写的相对应的模版了

### koa-static

是用于加载静态资源的中间件，通过它可以加载css、js等静态资源

#### 安装

```
npm i koa-static
```

#### 使用

```
const static = require("koa-static");
app.use(static(__dirname+"/static")) //加载静态文件的目录
```

总结：对于静态文件相关操作 前台可以直接写路径即可然后这里会按照着这个写的路径下去找你在前端页面所写的路径

如你前台写的是 `/js/a.js` 那么就会去 `/static/js/a.js` 如果你没有放在 static 文件下面是找不到的 因为你在配置 中间件的时候就设置的 static 文件

本篇只对koa进行学习性了解 有很多功能没有一一的去写 可以看看官方文档