---
title: JavaScritp同步与异步和Promise学习理解
categories: 技术笔记
tags:
  - JavaScript
  - 同步
  - 异步
  - 阻塞
  - 动画针
  - Promise
excerpt: 'JavaScritp同步与异步学习理解,动画针的理解,Promise,async,await'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922211756.png'
articleLink: 866b19eeaf194022
date: 2020-09-22 08:56:40
---

## 同步异步

 同步和异步是一种消息通知机制

### 同步阻塞

A调用B,B处理获得结果,才返回给A。

A在这个过程中,一直等待的处理结果,没有拿到结果之前,需要A(调用者)一直等待和确认调用结果是否返回,拿到结果然后继续往下执行。

做一件事,没有拿到结果之前,就一直在这等着,一直等到有结果了再去做下边的事

### 异步非阻塞

A调用B,无需等待B的结果,B通过状态,通知等来通知A或回调函数来处理。

做一件事,不用等待事情的结果,然后就去忙别的了,有了结果,再通过状态来告诉我,或者通过回
调函数来处理。

有一个同学小明去图书馆借书 图书馆需要去找这个树 找是需要时间的 如果又来了一个同学去借书 小明在等的时候就是堵塞了 如果这个等待时间 去干其他事了 那就是非阻塞 针对的主体是 小明

同步和异步 是针对的图书管理员 异步是 有没有这本书 都会告诉你一声 而同步是 你得没事就得问管理员 你找到没有 你找到没有

简单理解下

### 同步的过程

```
console.log("1")
function fn(){
	alert("弹窗")
	consol.log("2")
}
fn()
consol.log("3")
```

结果

1. 先打印出来1
2. 然后弹窗出来如果你不点击确定 那么将不会打印2和3这时候就堵塞上了 你不点什么也干不了
3. 点击后出现2 3

### 异步的过程

```
console.log("1")
function fn(){
	setTimeout(()=>{
			console.log("2")
	},1000)
}
fn()
consol.log("3")
//===>结果 1 ---> 3 ---> 2
```

### 案例

移动一个方块到指定的位置

```
function move(ele, arg, target) {
        let start = parseInt(window.getComputedStyle(ele, null)[arg])
        let dis = (target - start) / Math.abs(target - start)
        let speed = dis * 5
        function fn() {
            let now = parseInt(window.getComputedStyle(ele, null)[arg])
            if (now === target) {

            } else {
                ele.style[arg] = now + speed + "px"
                window.requestAnimationFrame(fn)
            }
        }
        fn()
    }
```

首先我想向右移动

```
let ele = document.querySelector('#box1')
move(ele, "left", 200)
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjsleft2.gif)

很好的完成了运动

然后我又想向下移动

```
move(ele, "top", 200)
```

理想的状态是这样移动

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922093410.png)

而实际上是 left 和 top 同时执行了！

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjsyidong13.gif)

解决就是加个回调函数

```
    function move(ele, arg, target, cb) {
        let start = parseInt(window.getComputedStyle(ele, null)[arg])
        let dis = (target - start) / Math.abs(target - start)
        let speed = dis * 5
        function fn() {
            let now = parseInt(window.getComputedStyle(ele, null)[arg])
            if (now === target) {
                // 运动完成
                cb && cb("运动下一个")
            } else {
                ele.style[arg] = now + speed + "px"
                window.requestAnimationFrame(fn)
            }
        }
        fn()
    }
    let ele = document.querySelector('#box1')
    move(ele, "left", 200, function (res) {
        console.log(res)
        move(ele, "top", 200)
    })

```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imghuidiaoyidong.gif)

但是这样写你会陷到无限回调的里面 这样后期很难维护

只时候就可以使用 Promise 了 专门解决回调地狱的

### 知识点：

#### window.requestAnimationFrame() 针动画

##### **屏幕刷新频率：**

屏幕每秒出现图像的次数。普通笔记本为60Hz

##### **动画原理：**

计算机每16.7ms刷新一次，由于人眼的视觉停留，所以看起来是流畅的移动。

##### **优势：**

由系统决定回调函数的执行时机。60Hz的刷新频率，那么每次刷新的间隔中会执行一次回调函数，不会引起丢帧，不会卡顿

```
var progress = 0;
    //回调函数
    function render() {
     progress += 1; //修改图像的位置
     if (progress < 100) {
     //在动画没有结束前，递归渲染
     window.requestAnimationFrame(render);
     }
    }
    //第一帧渲染
    window.requestAnimationFrame(render);
```

##### **CPU节能：**

setTimeout：当页面被隐藏或最小化时，setTimeout 仍然在后台执行动画任务

requestAnimationFrame：当页面处理未激活的状态下，该页面的屏幕刷新任务也会被系统暂停，因此跟着系统步伐走的requestAnimationFrame也会停止渲染，当页面被激活时，动画就从上次停留的地方继续执行，有效节省了CPU开销。

由于页面处于不可见或不可用状态，刷新动画是没有意义的，完全是浪费CPU资源。

## Promise

用于表示一个异步操作的最终完成 (或失败), 及其结果值.

它允许你为异步操作的成功和失败分别绑定相应的处理方法（handlers）

这让异步方法可以像同步方法那样返回值

但并不是立即返回最终执行结果

而是一个能代表未来出现的结果的promise对象

一个 `Promise`有以下几种状态:

- *pending*: 初始状态，既不是成功，也不是失败状态。
- *fulfilled*: 意味着操作成功完成。
- *rejected*: 意味着操作失败。

### 小案例

```
function loadImg(){
	return new Promise((resolve,reject)=>{
			let img = new Image()
			img.src = "https://pic3.zhimg.com/v2-cf1c78890006e078a538842a0caa7127_1440w.jpg?source=172ae18b"
			img.onload = function(){
					resolve("图片加载成功")
			}
			img.onerror = function(){
					reject("图片加载失败")
			}
	})
}
```

那么这样就可以进行再次用.then进行处理了！

```
loadImg().then(res=>{
		console.log("成功")
},err=>{
		console.log("失败")
})
```

## async和await

一个异步函数由`async`关键字定义。

在一个异步函数中可以使用`await`关键字。

`async`和`await`关键字可以使得对有等待时间的（异步）

以`Promise` 为基础的函数的定义更加简洁优雅，减少特意配置对于`promise `的链式调用。

总结： 异步函数 ----> 同步写法

async函数的函数体可以被认为是由0个或者多个await表达式分割开来。

当代码运行到await表达式的时候，该进程会进入等待模式并转让控制权，直到被等待的，异步的承诺(promise)被满足(resolved)或者拒绝(rejected)为止。

```
		async function myfn() {
        // await 后面一定要跟一个 Promise 对象
        await new Promise((res, rej) => {
            setTimeout(() => {
                console.log(111)
                res()
            }, 1000);
        })
        await new Promise((res, rej) => {
            setTimeout(() => {
                console.log(222)
                res()
            }, 1000);
        })
        await new Promise((res, rej) => {
            setTimeout(() => {
                console.log(333)
                res()
            }, 1000);
        })
    }
    myfn()
```

如果 是 reject的话就会停止运行

那么就可以是用 try来捕捉错误

总：async和await是用来简化 Promise 看着更加简洁



