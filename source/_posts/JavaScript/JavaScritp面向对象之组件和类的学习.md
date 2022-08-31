---
title: JavaScritp面向对象之组件和类的学习
categories: 技术笔记
tags:
  - JavaScript
  - 面向对象
  - 组件
  - 类
excerpt: JavaScritp面向对象之组件和类的学习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922205953.png'
articleLink: b1cb4d4875ae4020
date: 2020-09-20 20:36:40
---

## 组件

### 原则

- 对内：封闭
- 对外：开放（api 配置）

### 合并配置

#### assign()

`Object.assign()` 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

```js
const target = { a: 1, b: 2 };
const source = { a: 2, b: 5 };
const returnedTarget = Object.assign(target, source);
console.log(returnedTarget)//=>{ a: 2, b: 5 }
```

### 事件委托

#### 什么是事件委托

1. 支持为同一个DOM元素注册多个同类型事件
2. 可将事件分成事件捕获和事件冒泡机制

#### EventTarget.addEventListener()

**EventTarget.addEventListener()** 方法将指定的监听器注册到 `EventTarget`上，当该对象触发指定的事件时，指定的回调函数就会被执行。

```
target.addEventListener(type, listener, options);
```

- type: 必须,String类型,事件类型
- listener: 必须,函数体或者JS方法
- options: 可选,boolean类型。指定事件是否发生在捕获阶段。默认为false,事件发生在冒泡阶段

```
<div id="div1"></div>

window.onload = function(){
    let div1 = document.getElementById('div1');
    div1.addEventListener('click',function(){
        console.log('打印第一次')
    })
    div1.addEventListener('click',function(){
        console.log('打印第二次')
    })
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200920221336.png)

#### 示例

```html
<table id="outside">    
    <tr><td id="t1">one</td></tr>
    <tr><td id="t2">two</td></tr>
</table>
```

```js
// 改变t2的函数
function modifyText() {
  var t2 = document.getElementById("t2");
  if (t2.firstChild.nodeValue == "three") {
    t2.firstChild.nodeValue = "two";
  } else {
    t2.firstChild.nodeValue = "three";
  }
}

// 为table添加事件监听器
var el = document.getElementById("outside");
el.addEventListener("click", modifyText, false);
//==> one three
```

在上个例子中，`modifyText()` 是一个 `click` 事件的监听器，通过使用`addEventListenter()`注册到table对象上。在表格中任何位置单击都会触发事件并执行`modifyText()`。

### 自定义事件

首先定义一个你存放这些事件的地方

```js
// 定义一个方法存储的地方
let handle = {}
```

然后写一个可以将方法添加到这个容器里面

```js
// 添加方法
function addEvent(eventName,fn){
    // 判断有没有这个方法
    if(typeof handle[eventName] === 'undefined'){
    		handle[eventName] = []
    }
    // 将方法添加进去
    handle[eventName].psuh(fn)
}
```

也可以从这个容器里面删除这个方法

```js
function removeEvent(eventName,fn){
      // 是否有这个方法
      if(!(eventName in handle)){
          return
       }
      // 循环查找这个函数
      for(let i = 0;i< handle[eventName].length;i++){
          if(handle[eventName][i] === fn){
              handle[eventName].splice(i,1);
              //因为就有一个所以 删除完成后就可以结束了
              break;
          }
      }
}
```

最后可以批量执行方法事件

```js
// 执行
function trigger(eventName){
    // 是否有这个方法
    if(!(eventName in handle)){
   		 return
    }
    // 将这个方法内的其他方法拿出来执行
    handle[eventName].forEach(element => {
    		element()
    });
}
```

#### 用class改写自定义事件

```js
class MyEvent{
        constructor(){
            this.handle = {}
        }
        // 添加
        addEvent(eventName,fn){
            // 判断有没有这个方法
            if(typeof this.handle[eventName] === 'undefined'){
                this.handle[eventName] = []
            }
            // 将方法添加进去
            this.handle[eventName].psuh(fn)
        }
        // 删除
        removeEvent(eventName,fn){
            // 是否有这个方法
            if(!(eventName in this.handle)){
                return
            }
            // 循环查找这个函数
            for(let i = 0;i< this.handle[eventName].length;i++){
                if(this.handle[eventName][i] === fn){
                    this.handle[eventName].splice(i,1);
                    //因为就有一个所以 删除完成后就可以结束了
                    break;
                }
            }
        }
        // 执行
        trigger(eventName){
            // 是否有这个方法
            if(!(eventName in handle)){
                return
            }
            // 将这个方法内的其他方法拿出来执行
            handle[eventName].forEach(element => {
                element()
            });
        }

    }
```

自己封装的和 下面的 EventTarget类 功能是一样

### EventTarget类

`EventTarget` 是一个 DOM 接口，由可以接收事件、并且可以创建侦听器的对象实现。

#### EventTarget.addEventListener()

方法将指定的监听器注册到 EventTarget上，当该对象触发指定的事件时，指定的回调函数就会被执行。

```js
target.addEventListener(type, listener, options);
```

#### EventTarget.removeEventListener()

删除使用 `EventTarget.addEventListener()`方法添加的事件。使用事件类型，事件侦听器函数本身，以及可能影响匹配过程的各种可选择的选项的组合来标识要删除的事件侦听器。

```js
element.addEventListener("mousedown", handleMouseDown, true);
```

被移除之后，如果此事件正在执行，会立即停止。

#### EventTarget.dispatchEvent

向一个指定的事件目标派发一个事件 并以合适的顺序**同步调用**目标元素相关的事件处理函数。标准事件处理规则(包括事件捕获和可选的冒泡过程)同样适用于通过手动的使用`dispatchEvent()`方法派发的事件。

```js
cancelled = !target.dispatchEvent(event)
```

- `event` 是要被派发的事件对象。
- `target` 被用来初始化 事件 和 决定将会触发 目标.

### 通过继承拓展组件功能



