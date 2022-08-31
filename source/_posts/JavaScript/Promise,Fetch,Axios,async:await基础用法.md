---
title: 'Promise,Fetch,Axios,async/await基础用法'
categories: 技术笔记
tags:
  - Promise
  - Fetch
  - Axios
  - async
  - await
excerpt: 'Promise,Fetch,Axios,async/await基础用法'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201102110249.png'
articleLink: 4dbc469478123602
date: 2020-11-02 09:30:36
---

## Promise

是异步编程的一种解决方案 从语法上来说 `Promise` 是一个对象从他可以获取异步操作的消息

- 可以避免多层异步调用嵌套的问题（回调地狱）
- Promise 提供了简洁的API 使控制异步操作变得更加简单

#### 快速使用

我们创建一个`Promise` 对象然后传递一个方法里面有两个参数分别是在成功`resolve`时候调用和在失败`reject`的时候调用

```
var p = new Promise(function(resolve,reject){
    // 这里使用异步实现任务
    var flag = true
    // 模拟异步
    setTimeout(function(){
    if(flag){
        // 成功
        resolve("请求成功")
      }else{
        reject("请求失败")
    }
    },1000)
})
```

然后我们接收结果是使用`then` 的形式接收回调

P以 then 接收回调然后 传递两个函数 分别接收成功和失败

```
p.then(function(data){
  	console.log(data)
},function(error){
  	console.log(error)
})
```

## Fetch

更加简单更加的强大,灵活可以看作XHR的升级版 是基于Promise 实现的

#### 快速使用

我们需要传递一个api接口

- 第一个then是返回Promise实例对象
- 第二个then是Promise返回的then（也就是上面的Promise返回然后then里面接收数据）

```
fetch('https://www.79bk.cn/').then((data)=>{
	// text() 方法属于fetchAPI的一部分 
	// 返回的是 一个Promise对象
	// 用于获取后台返回的数据
	return data.text()
}).then((ret)=>{
	// 这里才是真正的获取到的数据
	console.log(ret)
})
```

> 注意：我们接收的时候是 data.text() 是字符的结果我们普遍接受的是 Josn 那么我们将使用  data.json() 

我们可以传递一些配置项

比如我们需要请求方式（POST,GET）还会有一些参数给后端

我们只需要在`fetch`的第二个参数上传递一个对象 然后里面写上相关的配置项即可

```
fetch('https://www.79bk.cn/',{
	// 请求方式
	method:'post',
	// 如果是post可以写一些参数
	body:JSON.stringify({
		name:'qiaoBlog'
	}),
	// 请求头
	headers:{}
}).then((data)=>{
	return data.text()
}).then((ret)=>{
	console.log(ret)
})
```

## Axios

易用、简洁且高效的http库 支持node端和浏览器端 支持 Promise 丰富的配置项

#### 快速使用

```
axios.get('https://www.79bk.cn/').then(function(ret){
		// 获取数据
		console.log(ret.data)
})
```

传递参数 我们可以将相关配置放在 第二个参数中 如下所示

```
axios.get('https://www.79bk.cn/',{
	params:{
		name:"qiaoBlog"
	}
}).then(function(ret){
		console.log(ret.data)
})
```

我们上面演示了`get` 形式的发送数据

接下来我们演示一下`post`

```
axios.post('https://www.79bk.cn/',{
		name:"qiaoBlog"
}).then(function(ret){
		console.log(ret.data)
})
```

我们可以直接将参数以对象形式直接放在第二个参数的位置

## async/await

是ES7引入的新语法可以更加的方便进行异步的操作

async关键字用在函数上（返回的是Promise实例对象）

await关键字用于async 函数中（可以得到异步结果）

#### 快速使用

```
// 创建一个异步函数
async function myfun(){
	// await 后面跟的是 Promise
	var ret = await new Promise(function(res,rej){
		setTimeout(function(){
			res("成功")
		},1000)
	})
	return ret
}
// 我们返回的就是 Promise 所以可以then
myfun().then(function(data){
	console.log(data)
})
```

多个异步 我们怎么使用呢？

```
async function myfun(){
		const info = await Promise(function(res,rej){})
		const ret = await Promise(function(res,rej){})
		return ret
}
```

我们直接可以在一个 `async`中使用多个 `await`