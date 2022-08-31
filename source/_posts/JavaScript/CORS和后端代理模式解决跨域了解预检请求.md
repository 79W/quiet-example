---
title: CORS和后端代理模式解决跨域了解预检请求
categories: 技术笔记
tags:
  - CORS
  - 跨域
excerpt: CORS和后端代理模式解决跨域了解预检请求
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201006153501.png'
articleLink: 2381da97511d0606
date: 2020-10-06 14:41:06
---

## CORS解决跨域

CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出`XMLHttpRequest`请求，从而克服了AJAX只能同源使用的限制。

### 示例

浏览器发现这次跨源AJAX请求是简单请求，就自动在头信息之中，添加一个`Origin`字段。

```
GET /cors HTTP/1.1
Origin: http://api.79bk.cn
Host: api.qiaobug.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

`Origin`字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）

如果`Origin`指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应

如果`Origin`指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。

```
Access-Control-Allow-Origin: http://api.79bk.cn
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

### 设置访问域名

`('Access-Control-Allow-Origin', '*')`表示任意域名都可以访问，默认不能携带cookie了。(必须字段)

```
//这样写，只有www.baidu.com 可以访问。
('Access-Control-Allow-Origin', 'http://www.baidu.com'); 
//这个表示任意域名都可以访问。
('Access-Control-Allow-Origin', '*'); 
```

### 设置允许requset设置的头部

如果浏览器请求包括`Access-Control-Request-Headers`字段，则`Access-Control-Allow-Headers`字段是必需的。

它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。

```
('Access-Control-Allow-Headers', 'Content-Type, Content-Length');
```

### 允许客户端获取的头部key

```
('Access-Control-Expose-Headers'，'Content-Type, Content-Length, Authorization, Accept')
```

CORS请求时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到6个基本字段：

`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`

如果想拿到其他字段，就必须在`Access-Control-Expose-Headers`里面指定。

### 是否允许Cookie

`Access-Control-Allow-Credentials`

该字段可选。

它的值是一个布尔值，表示是否允许发送Cookie。

默认情况下，Cookie不包括在CORS请求之中。

设为`true`，即表示服务器明确许可，Cookie可以包含在请求中，一起发给服务器。

这个值也只能设为`true`，如果服务器不要浏览器发送Cookie，删除该字段即可。

## 预检请求

非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。

浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。

只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。

#### 简单请求

`GET`、`POST`、`HEAD`、或者 `content-type` 的值为：`text/plain` 、`multipart/form-data`、`application/x-www-form-urlencoded` 均为简单请求可以直接发送请求

#### 预检请求

`PUT` 、`DELETE`、 `CONNECT`、 `OPTIONS` 、`TRACE` 、`PATCH` 需要发送预检请求 

下面是这个"预检"请求的HTTP头信息。

```
OPTIONS /cors HTTP/1.1
Origin: http://api.79bk.cn
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.qiaobug.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

"预检"请求用的请求方法是`OPTIONS`，表示这个请求是用来询问的。

##### Access-Control-Max-Age

该字段可选，用来指定本次预检请求的有效期，单位为秒。

设置此字段后在此期间，不用发出另一条预检请求。

## JSONP

jsonp的核心原理就是目标页面回调本地页面的方法,并带入参数

jsonp只能是get请求 我们需要与后端进行约定

原理就是 Html 引入 `<script>` 是一个道理 只是我们用后端回调的方法进行输出一些数据 

前端发送请求

```
let o = document.createElement("script");
o.src = "http://localhost:4000/getAjax?callback=cbfn";
document.querySelector("head").appendChild(o);
```

定义回调函数 并打印 携带的参数 

```
function cbfn(res){
    console.log(res);
}
```

后端 获取回调函数的名称然后进行处理 返回一个函数 `cbfn()` 然后前端接收到会直接执行这一就会打印这个参数

```
router.get("/getAjax",(ctx,next)=>{
		//接收参数 -->也就是函数名称
    let cb = ctx.query.callback;
    //定义变量参数
    let obj = {
        a:20,
        b:20
    }
    //返回去 拼接的结果就是 cbfn(obj)
    ctx.body = `${cb}(${JSON.stringify(obj)})`;
})
```

## 后端代理模块

利用 koa-server-http-proxy中间件实现代理

跨域是浏览器规范，通过同服务器请求数据，不通过浏览器请求，也能解决浏览器限制

如：我想请求的是 `https://www.79bk.cn/api`的接口但是本地直接请求会提示跨域 

我们可以用本地 Node 服务端创建个服务器然后转发过去 

![中间件](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img64A8F37C-2396-455E-A6EB-055B8F06B843.png)

**安装**

```
npm i koa-server-http-proxy
```

配置中间件

```
app.use(proxy('/api', {
  target: 'https://api.79bk.cn',
  pathRewrite: { '^/api': '' },
  changeOrigin: true
}))
```

解释一下哈 

我们 本地服务器配置后我们访问的时候

```
http://127.0.0.1:3000/api/info
```

那我当我们配置好后  服务端会给我们转发

```
https://api.79bk.cn/info
```

也就是说 后端识别到api 那么就会给我替换掉并访问 Api

