---
title: 'node.js服务端学习GET,POST,crawler模块,express'
categories: 技术笔记
tags:
  - node
  - 学习笔记
  - express
excerpt: express服务端的web框架学习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921221118.png'
articleLink: 4a553ab42c251903
date: 2020-07-03 09:25:19
---

## 静态传参数

前端传参：

1. Get ?id=12
2. post 不在url上面传参

### get 方式传参

访问方式：`http://127.0.0.1:5279/?id=1&age=22`

```js
//导入服务器模块
const http = require("http")
//导入 url处理模块
const url = require("url")

//创建服务器
const server = http.createServer((req,res)=>{
		//	我们可以通过url这个模块进行对get请求的参数进行分解
		let urlOBJ = url.parse(req.url,true)
		console.log(urlOBJ);
		//会有一个 query的对象参数里面保存着 这个get请求参数
		//发送到页面上	
		res.end(JSON.stringify(urlOBJ.query))
})
//开启服务器
server.listen(5279,()=>{
	console.log("服务器开启成功")
})
```

### post方式传参

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgpqd62wTgH1GIejs-20200921224630436.png)

```js
//导入服务器模块
const http = require("http")
//导入 post 接收处理的参数处理模块
const querystring = require("querystring")

//创建服务器
const server = http.createServer((req,res)=>{
		//	因为post的传递参数不是在url上面
		//我们要一点点的拿
		//存储 接收post参数的字符串
		let postData = ""
		//转化为对象
		let postObj = {}
		//一块块的接收
		req.on('data',(kuai)=>{
			//拼接起来
			postData += kuai
		})
		// 进行转换成对象的操作
		req.on('end',()=>{
			console.log(postData)
			//解析这个获取的参数 字符串
			//使用 querystring 这个模块进行转换
			postObj = querystring.parse(postData)
		})
		console.log(postObj)
		//发送到页面上	
		res.end(JSON.stringify(postObj))
})

//开启服务器
server.listen(5279,()=>{
	console.log("服务器开启成功")
})
```

### get和post的区别

1. get传参是通过url 而post是通过请求体传递的

2. get传参数据相对较小 而post传递的数据相对比较大

3. get 传值是在URL中 安全性不好

   post传参数相对安全性高一些

4. get 一般用于请求数据/获取数据

   post 一般用于提交数据 

## 爬虫

### crawler 模块

`https://www.npmjs.com/package/crawler`

1. 创建一个英文的文件夹

2. 进入文件夹 初始化npm

   `npm init -y`

3. 进入文件夹下载模块 npm i crawler

4. 使用模块 参考模块网站的使用说明文档

#### 爬取网站文字内容

```js
var Crawler = require("crawler");
var fs = require("fs")
 
var c = new Crawler({
	maxConnections : 10,
	// This will be called for each crawled page
	callback : function (error, res, done) {
		if(error){
			console.log(error);
		}else{
			var $ = res.$;
			// $ is Cheerio by default
			//a lean implementation of core jQuery designed specifically for the server
//			console.log($("title").text());
			
			fs.writeFile('./shuju2.json', $('body').text(),(err)=>{
				if(err == null){
					console.log("ok")
				}
			})
		}
		done();
	}
});
 
// Queue just one URL, with default callback
c.queue('https://ncov.dxy.cn/ncovh5/view/pneumonia');
```

### 爬取图片视频

反爬虫机制：

	1. 看你这是不是服务器请求 如果不是就不给你数据
 	2. 我们这个是node服务器有时候不会给我数据 

解决办法：

伪装 我们只需把这个node伪装成客户端浏览器请求的

```js
//导入 爬虫模块
var Crawler = require("crawler");
//导入文件操作模块
var fs = require('fs');
// 创建个爬虫
var c = new Crawler({
	encoding:null,
	jQuery:false,// set false to suppress warning message.
	callback:function(err, res, done){
		if(err){
			console.error(err.stack);
		}else{
//			对文件进行操作
			fs.createWriteStream(res.options.filename).write(res.body);
		}
		
		done();
	}
});
 
//c.queue({
////	图片地址
//	uri:"https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1593761366721&di=f7276c4620be977b3647123714c64494&imgtype=0&src=http%3A%2F%2Ft8.baidu.com%2Fit%2Fu%3D3571592872%2C3353494284%26fm%3D79%26app%3D86%26f%3DJPEG%3Fw%3D1200%26h%3D1290",
////	存放的地方路径 和文件名称
//	filename:"nodejs1.png"
//});

c.queue({
//	视频地址
	url:'http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4',
//	存储路径
	filename:"aaa.mp4",
//	伪装服务器
	headers:{'User-Agent':'requests'}
});
```

## express

快速，简单，极简的node web框架。

`https://www.npmjs.com/package/express`

```js
//导入模块
const express = require('express')
//创建对象
const app = express()
//调用
app.get('/', function (req, res) {
	//如果用内置http模块 返回的内容是 res.end()
	//现在用我们下载的模块是用 res.send()
	res.send('Hello World')
})
 //开启服务器 端口号
app.listen(5279,()=>{
	console.log("服务器开启成功")
})
```

### 创建一个静态的服务器

#### 访问根目录

```js
//创建一个静态资源服务器
//导包
const express = require('express')
//创建服务器对象
const app = express()
// 静态资源文件夹名称
app.use(express.static('文件名'))
//开启端口 
app.listen(5279)
```

#### 访问其他目录 url变了

```js
//访问 static 目录下的
app.use('/static', express.static('public'))
```

```
//结果
http://localhost:5279/static/images/kitten.jpg
http://localhost:5279/static/css/style.css
http://localhost:5279/static/js/app.js
http://localhost:5279/static/images/bg.png
http://localhost:5279/static/hello.html
```

### 完成get pai接口

```js
// api get接口
//接口地址：/joke
//请求方式：get 
//参数 无
// 一条随机话

//导包
const express = require('express')
//创建服务器
const app = express()

//写接口
app.get('/joke',(req,res)=>{
	let arr = ["有一次和男友吵架了在电话里哭，闺蜜来安慰我，突然，他盯着我的眼睛看。冒出一句：“你的睫毛膏用的什么牌子的，这么哭成这B样，都没掉”。我真是气打不一处来，电话一扔也不哭了。","在公园里看到一对很有爱的父女，父亲大约五十岁左右，女儿二十来岁，女儿很乖巧的给爸爸剥了一个茶叶蛋，说说什么互相开怀大笑，好温馨的家庭。但是，为什么后来他们就舌吻了呢？","昨天因为一件事骂儿子，说你妈妈是猪，你也是头猪。儿子却反过来说我：爸爸你怎么这么衰，娶了一头猪，还生了一只猪！你说你这熊孩子，这是不是找打。","一个男生暗恋一个女生很久了。一天自习课上，男生偷偷的传了小纸条给女生，上面写着“其实我注意你很久了”。不一会儿，女生传了另一张纸条，男生心急火燎的打开一看“拜托你不要告诉老师，我保证以后再也不嗑瓜子了”。。。。。。男生一脸懵逼","他如有所悟的道：“哦，怪不得我越来越丑了！ ”","我和老婆去游泳，她不会，然后我各种教他，一会她突然来一句，老公我好像饱了。","晚上跟老公在客厅看电视。很晚了老公说睡觉啊？！","我跟老公耍赖，“老公，我要你抱我进去！ ”","老公看了看我说：“算了，我还把床给你搬出来吧！！ ”","老婆：你送我100元的衣服，简直是对我的侮辱。道歉太不诚恳，我不接受。","老公：那么我应该怎么做才能令你接受呢？"]
	let index = Math.floor(Math.random()*11)
	//返回去
	res.send(arr[index])
})


//开启服务器
app.listen(5279,()=>{
	console.log("开启成功。。。。。")
})
```

### 返回json格式的api接口

```js

//导包
const express = require('express')
//创建服务器
const app = express()

//写接口
app.get('/name',(req,res)=>{

	//返回去
	res.send({
		name:"乔越",
		age:22
	})
})


//开启服务器
app.listen(5279,()=>{
	console.log("开启成功。。。。。")
})
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgJKfejvcWEU5YaOZ-20200921224646272.png)

### get接收参数

```js

//导包
const express = require('express')
//创建服务器
const app = express()
//写接口
app.get('/name',(req,res)=>{
//	返回json对象 这是 url的数据
	res.send(req.query)
})
//开启服务器
app.listen(5279,()=>{
	console.log("开启成功。。。。。")
})
```

### post

```js
//post请求
//导包
const express = require('express')
//创建服务器
const app = express()
//写接口
app.post('/name2',(req,res)=>{
//	返回json对象 这是 url的数据
	res.send(req.query)
	console.log(req.query)
})
//开启服务器
app.listen(5279,()=>{
	console.log("开启成功。。。。。")
})
```

#### post解析参数

```js

//导包
const express = require('express')
//导入 解析post的参数
var bodyParser = require('body-parser')
//创建服务器
const app = express()

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

//写接口
app.post('/login',(req,res)=>{
//	返回json对象 这是 url的数据
	res.send(req.body)
})
//开启服务器
app.listen(5279,()=>{
	console.log("开启成功。。。。。")
})
```

#### 新模块用来解析 post所传递的参数

##### body-parser 模块

在处理程序之前在中间件中解析传入的请求主体，该处理程序在`req.body`属性下可用。

1. 安装模块

   `npm install body-parser`

2. 导入模块

   ` var bodyParser **=** require('body-parser')`

3. 解析应用程序

   ` app.use(bodyParser.urlencoded({ extended**:** false }))`

   这步是在创建完服务器对象后进行调用

4. 在内部调用body返回参数

   `res.end(req.body)`

