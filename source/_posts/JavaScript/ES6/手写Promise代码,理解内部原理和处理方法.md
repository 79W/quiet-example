---
title: 手写Promise代码,理解内部原理和处理方法
categories: 技术笔记
tags:
  - Promise
excerpt: 手写Promise代码,理解内部原理和处理方法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20210317124024.png'
articleLink: 70da818f
date: 2021-03-17 12:44:00
---

### Promise是什么？

是一种新的解决异步编程的解决方案

他可以获取异步获取的结果可以解决回调地狱

### Promise/A+规范

1. executor同步执行,传入两个函数resolve, reject,需要内部定义两个函数
2. resolvereject和都需要传入成功的原因和失败的结果参数
3. 定义then方法,传入两个函数参数,成功的回调以及失败的回调
4. 内部有三个状态 pending==> resolve pending=> reject(支持能从 pending改变)
5. 如果是 resolve需要执行then的第一个参数( function)
6. 如果是 reject需要执行then后面第二个参数( function)
7. onFulfilled onRejected执行的时候还需要有参数,成功的结果或者失败的原因
8. 只有在pending的时候状态才能改变
9. 处理 executor内部的错误,调用 reject同时传入错误对象进去
10. 处理异步调用的 resolve&并且能够多次调用then方法(then传进来的方法存起来,状态改变时候再去调用)
11. then方法的两个参数必须都是异步执行的,不能再当前执行栈执行

### 代码实现

我们根据A+规范进行编码就可以完成一个合格的promise代码

**我们先实现同步版本的Promise**

我们需要定义三个状态 分别为 初始化 成功 失败 

我们根据Promise的特性还有`then` 方法

```
// 首先我们来定义状态
let PENDING = 'PENDING' //进行初始化
let RESOLVE = 'RESOLVE' //成功
let REJECT = 'REJECT' //失败

class Promise{
    constructor(executor){
        // 存状态
        this.state = PENDING
        // 存返回值
        this.value = undefined
        // 成功 
        const resolve = (value)=>{

        }
        // 失败
        const reject = (reason)=>{

        }

    }
    // 传入两个方法 第一个为成功 第二个为失败
    then(onFulfilled,onRejected){
        
    }
}
```

根据规则我们 只能从初始化转向成功或者失败就不可以在进行改变了

我们接下来将成功和失败方法进行编辑

我们只需要将状态更改和将状态返回值赋值操作

```
// 成功 
const resolve = (value)=>{
  // 只有是初始化的时候状态才能进行改变
  if(this.state === PENDING){
    this.state = RESOLVE
    this.value = value
  }
}
```

```
// 失败
const reject = (reason)=>{
  // 要保证状态的唯一性质
  if(this.state === PENDING){
    this.state = RESOLVE
    this.value = reason
  }
}
```

我将成功和失败编写完成后（同步代码）我们开始编写`then`代码

`then`有三种状态成功和失败和初始化 （并且需要传递进来两个方法）

我们先编写成功和失败的处理方法

```
// 传入两个方法 第一个为成功 第二个为失败
then(onFulfilled,onRejected){
	// 状态是失败的时候处理
  if(this.state === REJECT){
  	onRejected(this.value)
  }
  //状态是成功的时候处理
  if(this.state === RESOLVE){
    onFulfilled(this.value)
  }
}
```

当我们完成到这里就可以同步的进行调用了但是和原生Promise还差个异步处理

**我们接下来完成异步处理办法**

处理办法就在 `then` 中我们还有个状态没有进行编写 也就是初始化状态 `PENDING`

因为在`PENDING` 状态的时候是进行操作的时候所以我们只需要将这时候的方法存储起来就可以了

然后当我们完成后改变状态的时候将所有方法都拿出来进行执行

```
// 存入事件
this.onFulfilledCallbacks = []
this.onRejectedCallbacks = []
```

我们在 `then` 中编写`PENDING` 时候的处理方法

```
// 这里处理异步操作
if(this.state === PENDING){
  // 将成功和失败的事件存起来
  this.onFulfilledCallbacks.push(()=>{
      const x = onFulfilled(this.value)
  })
  
  this.onRejectedCallbacks.push(()=>{
    queueMicrotask(()=>{
        const x = onRejected(this.value)
    })
  })
}
```

这里我们用了一个 `queueMicrotask` 方法 是将方法放在微任务队列中进行调用

然后我们将`PENDING`时候的操作方法都存起来了这时候就是调用了

我们只需要在成功和失败的方法中调用就可以了 

```
// 成功 
const resolve = (value)=>{
  // 只有是初始化的时候状态才能进行改变
  if(this.state === PENDING){
    this.state = RESOLVE
    this.value = value
    // 当调用成功的时候执行函数
    this.onFulfilledCallbacks.forEach((cb)=>cb())
  }
}
```

```
// 失败
const reject = (reason)=>{
  // 要保证状态的唯一性质
  if(this.state === PENDING){
    this.state = RESOLVE
    this.value = reason
    // 当调用失败的时候执行函数
    this.onRejectedCallbacks.forEach((cb)=>cb())
  }
}
```

然后我们就差一个链式调用了

因为现在我们所用`then`进行链式调用的时候返回的是`undefined` 因为现在`then`没有设置返回值呢

我们只需要在返回一个 `promise`但是这里也要参考规范然后进行编写 

我们在 `then`中在声明一个`promise` 然后将他进行返回

```
		// 传入两个方法 第一个为成功 第二个为失败
    then(onFulfilled,onRejected){
        if(typeof onFulfilled !== 'function'){
            onFulfilled = (v)=>v
        }
        if(typeof onRejected !== 'function'){
            onRejected = (err)=>{
                throw err
            }
        }
        // 因为要链式调用所以要返回一个promise
        const returnPromise = new Promise((resolve,reject)=>{
            // 因为是同步代码所以可以
            if(this.state === RESOLVE){
                // onFulfilled(this.value)
                // 使用微任务包裹起来
                queueMicrotask(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onFulfilled(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                })
            }
            if(this.state === REJECT){
                // onRejected(this.value)
                queueMicrotask(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onRejected(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                    
                })
            }
            // 这里处理异步操作
            if(this.state === PENDING){
                // 将成功和失败的事件存起来
                this.onFulfilledCallbacks.push(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onFulfilled(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                    // onFulfilled(this.value)
                })
                this.onRejectedCallbacks.push(()=>{
                    // onRejected(this.value)
                    queueMicrotask(()=>{
                        // 因为当链式的时候失败了下一个就执行失败的
                        try{
                            // 这里根据a+规范需要判断是否是标准类型或者是promise
                            // 所以这里抽离出来一个函数进行验证执行
                            const x = onRejected(this.value)
                            resolvePromise(returnPromise,x,resolve,reject)
                        }catch(error){
                            reject(error)
                        }
                    })
                })
            }
        })
        // 返回一个Promise
        return returnPromise
    }
```

这里我们 可以看到一个陌生的方法 `resolvePromise` 这是因为我们根据规范的出

如果返回值是基本类型那么下一个默认是成功

还有可能是一个`promise`所有我们抽离出来一个函数进行判断

```
// 抽离出来
function resolvePromise(promise,x,resolve,reject) {
    if(promise === x){
        return reject(new TypeError('类型错误'))
    }
    // 
    let called = false
    // 是以then来区分是不是 promise
    if(typeof x === 'object' && x !== null || (typeof x === 'function')){
        try{
            const then = x.then
            if(typeof then === 'function'){
                then.call(x,(y)=>{
                    // y 可能还是个 promise
                    if(called)return
                    called = true
                    resolvePromise(promise,y,resolve,reject)
                },(x)=>{
                    if(called)return
                    called = true
                    reject(x)
                })
            }else{
                if(called)return
                called = true 
                resolve(x)
            }
        }catch(error){
            if(called)return
            called = true
            // 错误就返回错误
            reject(error)
        }
        
    }else{
        if(called)return
        called = true 
        resolve(x)
    }
}
```

这时候我们的`Promise`就算完成了我们在处理一下细节比如用一些 异常捕获来处理异常

```
// 最终代码
// 首先我们来定义状态
let PENDING = 'PENDING' //进行初始化
let RESOLVE = 'RESOLVE' //成功
let REJECT = 'REJECT' //失败
// 抽离出来
function resolvePromise(promise,x,resolve,reject) {
    if(promise === x){
        return reject(new TypeError('类型错误'))
    }
    // 
    let called = false
    // 是以then来区分是不是 promise
    if(typeof x === 'object' && x !== null || (typeof x === 'function')){
        try{
            const then = x.then
            if(typeof then === 'function'){
                then.call(x,(y)=>{
                    // y 可能还是个 promise
                    if(called)return
                    called = true
                    resolvePromise(promise,y,resolve,reject)
                },(x)=>{
                    if(called)return
                    called = true
                    reject(x)
                })
            }else{
                if(called)return
                called = true 
                resolve(x)
            }
        }catch(error){
            if(called)return
            called = true
            // 错误就返回错误
            reject(error)
        }
        
    }else{
        if(called)return
        called = true 
        resolve(x)
    }
}
class Promise{
    constructor(executor){
        // 存状态
        this.state = PENDING
        // 存返回值
        this.value = undefined
        // 存入事件
        this.onFulfilledCallbacks = []
        this.onRejectedCallbacks = []
        // 成功 
        const resolve = (value)=>{
            // 只有是初始化的时候状态才能进行改变
            if(this.state === PENDING){
                this.state = RESOLVE
                this.value = value
                // 当调用成功的时候执行函数
                this.onFulfilledCallbacks.forEach((cb)=>cb())
            }
        }
        // 失败
        const reject = (reason)=>{
            // 要保证状态的唯一性质
            if(this.state === PENDING){
                this.state = RESOLVE
                this.value = reason
                // 当调用失败的时候执行函数
                this.onRejectedCallbacks.forEach((cb)=>cb())
            }
        }
        // 如果没有调用那么直接捕获错误进行调用错误返回
        try{
            executor(resolve,reject)
        }catch(error){
            reject(error)
        }
    }
    // 传入两个方法 第一个为成功 第二个为失败
    then(onFulfilled,onRejected){
        if(typeof onFulfilled !== 'function'){
            onFulfilled = (v)=>v
        }
        if(typeof onRejected !== 'function'){
            onRejected = (err)=>{
                throw err
            }
        }
        // 因为要链式调用所以要返回一个promise
        const returnPromise = new Promise((resolve,reject)=>{
            // 因为是同步代码所以可以
            if(this.state === RESOLVE){
                // onFulfilled(this.value)
                // 使用微任务包裹起来
                queueMicrotask(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onFulfilled(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                })
            }
            if(this.state === REJECT){
                // onRejected(this.value)
                queueMicrotask(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onRejected(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                    
                })
            }
            // 这里处理异步操作
            if(this.state === PENDING){
                // 将成功和失败的事件存起来
                this.onFulfilledCallbacks.push(()=>{
                    // 因为当链式的时候失败了下一个就执行失败的
                    try{
                        // 这里根据a+规范需要判断是否是标准类型或者是promise
                        // 所以这里抽离出来一个函数进行验证执行
                        const x = onFulfilled(this.value)
                        resolvePromise(returnPromise,x,resolve,reject)
                    }catch(error){
                        reject(error)
                    }
                    // onFulfilled(this.value)
                })
                this.onRejectedCallbacks.push(()=>{
                    // onRejected(this.value)
                    queueMicrotask(()=>{
                        // 因为当链式的时候失败了下一个就执行失败的
                        try{
                            // 这里根据a+规范需要判断是否是标准类型或者是promise
                            // 所以这里抽离出来一个函数进行验证执行
                            const x = onRejected(this.value)
                            resolvePromise(returnPromise,x,resolve,reject)
                        }catch(error){
                            reject(error)
                        }
                    })
                })
            }
        })
        // 返回一个Promise
        return returnPromise
    }
}
```

