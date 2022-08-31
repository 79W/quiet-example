---
title: 'node.js内置模块fs,path,http的学习'
categories: 技术笔记
tags:
  - node
  - 学习笔记
excerpt: '学习node的基础内置模块fs,path,http的学习'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921221118.png'
articleLink: 766c3139ce2e1902
date: 2020-07-02 14:18:19
---

## fs模块

### 导包

```js
//导包
const fs = require('fs');
```

### 读取文件内容

```js
//读取文件内容 异步
//参数
//1.文件路径
//2.可有可无 这个是编码
//3.这个是回调函数
fs.readFile('文件名','utf-8', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

### 写入文件内容

```js
//写入文件内容 异步
//1.文件路径
	//如果没有文件夹会报错 如果没有文件会帮你创建
//2.要写入的内容
//3.回调函数
fs.writeFile('文件.txt', data, (err) => {
  if (err) throw err;
  console.log('文件已被保存');
});
```

### 同步异步

```js
//同步
console.log("乔越1")
for(var i = 0 ; i<100 ;i++){
	console.log(i)
}
console.log("乔越2")
```

```js
//异步
console.log("乔越1")
setTimeout(()=>{
  console.log("乔越2")
},2000)
```

### 写入读写同步操作

```js
// 这个是有个返回值 这个值就是文件内的内容
//读操作 同步
//体验不是很好 很少用尽量不用
fs.readFileSync('<目录>');
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgJoM2dQ3w6ruIqN9-20200921221423062.png)

## path路径模块

### 相对路径

node的相对路径是相对运行软件的路径

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgD6B34RqCoPjcMeY-20200921221657961.png)

```js
//1.导包
const fs = require('fs');
//2.调用
fs.readFile('file/t.txt',(err,data)=>{})
```

### 绝对路径

```js
//1.导包
const fs = require('fs');
//2.调用
//地址使用绝对路径
fs.readFile('C:\\file\\t.txt',(err,data)=>{})
```

### 路径变量

```js
console.log(__dirname)//所在文件夹的绝对路径
console.log(__filename) //拿到当前的绝对路径
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgR1jYmh2btIK7vUe-20200921221730980.png)

```js
//自己拼接路径
console.log(__dirname+"\\etc\\t.txt");
```

### 基本使用path

```
//导入模块
const path = require('path')
//使用方法
//join 把路径片段链接成一个新的路径
const fullpath = path.join(__dirname,'etc','1.txt')

```

## http模块

### 创建服务器

```js
//导入http模块
const http = require('http');
//创建一个服务器
//这个方法有一个返回值 返回值就代表这个服务器
const server = http.createServer((request,response)=>{
		//设置编码问题 响应头
  	response.setHeader('Content-type','text/html;charset=utf-8')
  	//设置返回给用户看的内容
		response.end("hellow world")
})
//开启服务器
//端口
server.listen(8087,()=>{
	console.log("服务器开启了：8087端口")
})
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaBc2jmHvpLgDE9P.png)

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgTj2PDGH8Kua9wFy-20200921224521475.png)

### 端口了解

一台电脑可以部署很多个服务器

我们可以通过ip地址（127.0.0.1/localhost）找到这个电脑 通过不同的端口号来区分不同的服务器。

#### 注意

默认的http端口80端口索引自己在创建服务器就不能在用80端口来创建服务器了 我们应该使用没有被人占用的端口

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgmRtKDEhwYFjbTVa.png)

### 创建一个静态服务器

```js
//创建一个静态服务器
//要求是 用户访问服务器上的文件 没有就返回404页面
//1.导入包
const fs = require('fs') //文件操作
const http = require('http') //服务器
const path = require('path') //路径
//2. 创建服务器
const server = http.createServer((require,Response)=>{
    //3.设置用户访问路径
    //拼接请求访问资源路径
    const fullpath = path.join(__dirname,'web',require.url)
    //能根据你访问的路径知道是什么类型的资源 
    fs.readFile(fullpath,(err,data)=>{
        if(err == null){
            Response.end(data)

        }else{
            Response.end("404")
        }
    })

})
//4.开启服务器
server.listen(2279,()=>{
    console.log('服务器开启成功')
})

```

