---
title: Fetch-Api基本用法和学习和理解
categories: 技术笔记
tags:
  - Fetch
excerpt: Fetch-Api基本用法和学习和理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201007100146.png'
articleLink: 61a363200f2f2707
date: 2020-10-07 09:34:27
---

## Fetch

Fetch 是基于 Promise 语法结构，而且它的设计足够低阶，这表示它可以在实际需求中进行更多的弹性设计

Fetch API 规范明确了用户代理获取资源的语义。原生支持 `Promise`[1](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)，调用方便，符合语义化。可配合使用 ES2016 中的 `async` / `await` 语法，更加优雅。

一个基本的 fetch 请求

这里我们通过网络获取一个 JSON 文件并将其打印到控制台。

最简单的用法是只提供一个参数用来指明想 `fetch()` 到的资源路径

然后返回一个包含响应结果的promise（一个 `Response`对象）

```
fetch('https://api.qiaobug.com/')
  .then(function(response) {
        //返回到是一个对象
        return response.json();
  })
  .then(function(myJson) {
     	  //这里才是打印返回结果
        console.log(myJson);
});
```

### Request 对象

### Headers 对象

允许您对HTTP请求和响应头执行各种操作。

这些操作包括检索，设置，添加和删除。

**添加**

`append()`方法可以追加一个新值到已存在的headers上，或者新增一个原本不存在的header。

```
myHeaders.append(name,value);
```

name：要追加给Headers对象的HTTP header名称.

value：要追加给Headers对象的HTTP header值.

**删除**

**`delete()`** 方法可以从Headers对象中删除指定header.

```
myHeaders.delete(name);
```

name：需删除的HTTP header名称.

**示例**

```
let myHeaders = new Headers();

myHeaders.append('Content-Type', 'text/xml');

myHeaders.get('Content-Type');
// should return 'text/xml'
```

也可以直接写在内部

```
myHeaders = new Headers({
  "Content-Type": "text/plain",
  "Content-Length": content.length.toString(),
  "X-Custom-Header": "ProcessThisImmediately",
});
```

### Response对象

呈现了对一次请求的响应数据。

**Response.text()**

读取 response，并以文本形式返回 response

```
response.text().then(function (text) {

});
```

**Response.json()**

接收一个 `Response` 流，并将其读取完成

它返回一个 Promise，Promise 的解析 resolve 结果是将文本体解析为 `JSON`

```
response.json().then(data => {

});
```

返回一个被解析为`JSON`格式的promise对象

### 基本用法

```
fetch("/test").then(res=>{
		return res.json();
}).then(res=>{
		console.log(res);
}).catch(err=>{
		console.log(err);
})
```

- 第一个参数是请求的url
- 第二个参数 options对象 常用配置
  - `method`: 请求使用的方法，如 `GET`、`POST`。
  - `headers`: 请求的头信息，形式为 `Headers` 的对象或包含 `ByteString` 值的对象字面量。
  - `body`: 请求的 body 信息
    - 注意 GET 或 HEAD 方法的请求不能包含 body 信息。
  - `mode`: 请求的模式，如 `cors、` `no-cors` 或者 `same-origin。`
    - `same-origin`表示必须同源，绝对禁止跨域，这个是老版本浏览器默认的安全策略。
    - `no-cors`这个就很特殊了，字面意思是禁止以CORS的形式跨域，其实它的效果是，对外域的请求可以发送，外域服务器无论设不设`Access-Control-Allow-Origin: *`都会接收请求并处理请求，但是浏览器不接收响应，即使外域返回了内容，浏览器也当做没接到

### Post请求示例

要创建一个 `POST` 请求，或者其他方法的请求，我们需要使用 `fetch` 选项：

- method ： HTTP 方法，例如 `POST`，
- body： request body，其中之一：
  - 字符串（例如 JSON 编码的），
  - `FormData` 对象，以 `form/multipart` 形式发送数据，
  - `Blob`/`BufferSource` 发送二进制数据，
  - URLSearchParams，以 `x-www-form-urlencoded` 编码形式发送数据，很少使用。

JSON 形式是最常用的。

例如，下面这段代码以 JSON 形式发送 `user` 对象：

```
let user = {
  name: 'John',
  surname: 'Smith'
};

let response = await fetch('/article/fetch/post/user', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json;charset=utf-8'
  },
  body: JSON.stringify(user)
});

let result = await response.json();
alert(result.message);
```

示例2

```
fetch("/getData",{
		//请求方式
    method:"post",
    // 请求参数为Json
    body:JSON.stringify({name:"zhangsan",age:20}),
    headers:{
    		//设置头部
    		"content-type":"application/json",
    }
}).then(res=>{
		return res.json();
}).then(res=>{
		console.log(res);
})
```

请注意，如果请求的 `body` 是字符串，则 `Content-Type` 会默认设置为 `text/plain;charset=UTF-8`。

但是，当我们要发送 JSON 时，我们会使用 `headers` 选项来发送 `application/json`，这是 JSON 编码的数据的正确的 `Content-Type`。

