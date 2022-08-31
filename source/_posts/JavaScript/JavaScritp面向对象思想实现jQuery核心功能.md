---
title: JavaScritp面向对象思想实现jQuery核心功能
categories: 技术笔记
tags:
  - JavaScript
  - 面向对象
  - jQuery
excerpt: JavaScritp面向对象思想实现jQuery核心功能
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922211148.png'
articleLink: e669eb992a592921
date: 2020-09-21 08:01:29
---

## 面向对象实现JQ库核心功能

### 整体框架

有个类,类里面有方法 然后进行实例化,秦段页面进行直接调用

```js
class Jq {
    constructor(arg) {
		
    }
		// 点击事件
		click(fn) {
      fn()
		}
}
function $(arg) {
	return new Jq(arg);
}
```

因为jquery可以进行多种传递方式所以需要在 `constructor`里面进行获取是那种传递方式

```js
				constructor(arg, root) {
                if (typeof arg === "function") {
                    //  dom 结构加载完毕执行代码    
                    this.ready(arg)
                } else if (typeof arg === "string") {
                    // 传入id 或者 class
                    let ele = document.querySelectorAll(arg);
                    this.addELement(ele)
                } else {
                    //传入的是js原生节点
                    if (typeof arg.length == 'undefined') {
                        //一个对象节点
                        this[0] = arg
                        this.length = 1
                    } else {
                        //多个对象节点
                        this.addELement(arg)

                    }
                }

            }
```

### eq()

jquery有eq方法 而eq方法后还可以调用其他方法

如：

```js
$("div").eq(0).click(function () {})
```

这里就需要考虑原型链的问题了

**基本面向对象实现链式操作**

```js
let = {
	click(){
		console.log("click")
		return this
	}
	css(){
		console.log("css")
		return this
	}
}
```

```js
obj.click().css()
```

而在这个封装里面我们返回的不是this 了而是个对象

```js
// 获取指定一个
eq(index) {
		return new Jq(this[index], this)
}
```

### end()

获取上一次的操作节点

如：

```js
$('div').eq(1).end()
```

这个结果就是 获取 `$('div')` 的结果

如果直接获取第一个没有上一次的节点那么就需要给 `undefined`

我们需要在 `constructor` 内进行处理 

```js
constructor(arg, root) {
    // 是否有上一次操作的节点
    if (typeof root === 'undefined') {
    		this['prevObject'] = [document]
    } else {
    		this['prevObject'] = root
    }
}j
```

而这个 `root` 是在eq()` 当中传递过来的

```js
// 获取指定一个
eq(index) {
		return new Jq(this[index], this)
}
```

返回 this 当前的 如果没有返回那么就是 `undefined`

```js
//获取上一次操作的节点
end() {
		return this['prevObject']
}
```

### css处理

可能获取的参数

1.获取css 的样式

```js
$('.box1').css('background')
```

2.设置css样式 两个参数

```js
$('.box1').css('background','red')
```

3.设置css样式 对象

```js
$('.box1').css({'background':'red','margin':'10px'})
```

**那么可以根据这几个方式 进行编写css的方法**

```js
					css(...arg) {
                if (arg.length === 1) {
                    if (typeof arg[0] === 'string') {
                        if (arg[0] in $.cssHooks) {
                            return $.cssHooks[arg[0]].get(this[0]);
                        }
                        // 字符串
                        return this.getStyle(this[0], arg[0])
                    } else {
                        //传入的对象
                        for (let i = 0; i < this.length; i++) {
                            for (let j in arg[0]) {
                                this.setStyle(this[i], j, arg[0][j])
                            }
                        }
                    }
                } else {
                    // 多个参数
                    for (let i = 0; i < this.length; i++) {
                        this.setStyle(this[i], arg[0], arg[1]);
                    }
                }
                return this;
            }
```

获取样式

```js
// 获取样式
getStyle(ele, styleName) {
			return window.getComputedStyle(ele, null)[styleName]
}
```

设置样式

**这个特别注意下** `$.cssNumber`

```js
// 设置样式
setStyle(ele, styleName, styleValue) {
    if (typeof styleValue === 'number' && !(styleName in $.cssNumber)) {
    		styleValue = styleValue + "px"
    }
    if (styleName in $.cssHooks) {
    		$.cssHooks[styleName].set(ele, styleValue);
    } else {
    		ele.style[styleName] = styleValue;
    }
}
```

这个是出来这个css 值是否为 `number` 是否加上px的判断

那么有的需要加上px 有的不需要加上就需要用上这些参数了

判断是否在这里面如果不在 就需要加上px

```js
$.cssNumber = {
    animationIterationCount: true,
    columnCount: true,
    fillOpacity: true,
    flexGrow: true,
    flexShrink: true,
    fontWeight: true,
    gridArea: true,
    gridColumn: true,
    gridColumnEnd: true,
    gridColumnStart: true,
    gridRow: true,
    gridRowEnd: true,
    gridRowStart: true,
    lineHeight: true,
    opacity: true,
    order: true,
    orphans: true,
    widows: true,
    zIndex: true,
    zoom: true
}
```

### 完整js代码

```js
        class Jq {
            constructor(arg, root) {
                // 是否有上一次操作的节点
                if (typeof root === 'undefined') {
                    this['prevObject'] = [document]
                } else {
                    this['prevObject'] = root
                }

                if (typeof arg === "function") {
                    //  dom 结构加载完毕执行代码    
                    this.ready(arg)
                } else if (typeof arg === "string") {
                    // 传入id 或者 class
                    let ele = document.querySelectorAll(arg);
                    this.addELement(ele)
                } else {
                    //传入的是js原生节点
                    if (typeof arg.length == 'undefined') {
                        //一个对象节点
                        this[0] = arg
                        this.length = 1
                    } else {
                        //多个对象节点
                        this.addELement(arg)

                    }
                }

            }
            // 获取指定一个
            eq(index) {
                return new Jq(this[index], this)
            }
            get(index) {
                return this[index]
            }
            //获取上一次操作的节点
            end() {
                return this['prevObject']
            }
            // 处理多个节点
            addELement(ele) {
                ele.forEach((el, index) => {
                    this[index] = el;
                })
                this.length = ele.length
            }
            // 处理方法直接
            ready(arg) {
                window.addEventListener("DOMContentLoade", arg, false)
            }
            // 点击事件
            click(fn) {
                // fn()
                for (let i = 0; i < this.length; i++) {
                    this[i].addEventListener("click", fn, false)
                }
            }
            on(eventName, fn) {
                let reg = /\s+/g;
                eventName = eventName.replace(reg, " ")
                let arr = eventName.split(" ")
                for (let i = 0; i < this.length; i++) {
                    for (let j = 0; j < arr.length; j++) {
                        this[i].addEventListener(arr[j], fn, false)
                    }
                }

            }
            css(...arg) {
                if (arg.length === 1) {
                    if (typeof arg[0] === 'string') {
                        if (arg[0] in $.cssHooks) {
                            return $.cssHooks[arg[0]].get(this[0]);
                        }
                        // 字符串
                        return this.getStyle(this[0], arg[0])
                    } else {
                        //传入的对象
                        for (let i = 0; i < this.length; i++) {
                            for (let j in arg[0]) {
                                this.setStyle(this[i], j, arg[0][j])
                            }
                        }
                    }
                } else {
                    // 多个参数
                    for (let i = 0; i < this.length; i++) {
                        this.setStyle(this[i], arg[0], arg[1]);
                    }
                }
                return this;
            }
            // 获取样式
            getStyle(ele, styleName) {
                return window.getComputedStyle(ele, null)[styleName]
            }
            // 设置样式
            setStyle(ele, styleName, styleValue) {
                if (typeof styleValue === 'number' && !(styleName in $.cssNumber)) {
                    styleValue = styleValue + "px"
                }
                if (styleName in $.cssHooks) {
                    $.cssHooks[styleName].set(ele, styleValue);
                } else {
                    ele.style[styleName] = styleValue;
                }
            }
        }

        function $(arg) {
            return new Jq(arg);
        }
        $.cssNumber = {
            animationIterationCount: true,
            columnCount: true,
            fillOpacity: true,
            flexGrow: true,
            flexShrink: true,
            fontWeight: true,
            gridArea: true,
            gridColumn: true,
            gridColumnEnd: true,
            gridColumnStart: true,
            gridRow: true,
            gridRowEnd: true,
            gridRowStart: true,
            lineHeight: true,
            opacity: true,
            order: true,
            orphans: true,
            widows: true,
            zIndex: true,
            zoom: true
        }
        $.cssHooks = {};
```

