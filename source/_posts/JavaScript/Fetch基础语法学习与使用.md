---
title: Fetch基础语法学习与使用
categories: 技术笔记
tags:
  - Fetch
excerpt: Fetch基础语法学习与使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922202957.png'
articleLink: d3b12b7964be3801
date: 2020-09-01 19:38:38
---

## Fetch

Fetch不是ajax 他的目的是为了代替ajax 是js原生api 基于fetch可以实现服务互相通讯

1. es2018年规范中新增 api 所以浏览器的支持不是特别好 
2. 想要好一点的 可以使用 fetch pollyfill
3. 返回的是promise对象

第一个参数为 请求地址
第二个参数为 

	1. method(String):http请求方法 默认get
 	2. body(String):http请求参数
      	1. post 
      	2. heade
      	3. get 不能使用body
 	3. headers(object):请求头 {}
 	4. credentials(String): 默认为`omit`,忽略的意思，也就是不带cookie;还有两个参数，`same-origin`，意思就是同源请求带cookie；`include`,表示无论跨域还是同源请求都会带cookie

```js
fetch(url, options).then(function(response) { 
// handle HTTP response
}, function(error) {
 // handle network error
})
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1200-20200922202710808.png)

1. status(number): HTTP返回的状态码，范围在100-599之间
2. statusText(String): 服务器返回的状态文字描述，例如`Unauthorized`,上图中返回的是`Ok`
3. ok(Boolean): 如果状态码是以2开头的，则为true
4. headers: HTTP请求返回头
5. body: 返回体，这里有处理返回体的一些方法
   1. text(): 将返回体处理成字符串类型
   2. json()： 返回结果和 JSON.parse(responseText)一样
   3. blob()： 返回一个Blob，Blob对象是一个不可更改的类文件的二进制数据
   4. arrayBuffer()
   5. formData()



### 兼容性

如caniuse所示，IE浏览器完全不支持fetch，移动端的很多浏览器也不支持,所以，如果要在这些浏览器上使用Fetch，就必须使用fetch polyfill

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1200-20200922202703183.png)

### fetch和xhr的不同

fetch虽然底层，但是还是缺少一些常用xhr有的方法，比如能够取消请求（abort）方法
fetch在服务器返回4xx、5xx时是不会抛出错误的，这里需要手动通过，通过response中的ok字段和status字段来判断



