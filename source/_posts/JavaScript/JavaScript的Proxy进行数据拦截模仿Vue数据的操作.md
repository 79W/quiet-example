---
title: JavaScript的Proxy进行数据拦截模仿Vue数据的操作
categories: 技术笔记
tags:
  - JavaScript
  - Proxy
  - Vue
  - 数据拦截
excerpt: JavaScript的Proxy进行数据拦截模仿Vue数据的操作
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200923094246.png'
articleLink: 4d1513f1604e4123
date: 2020-09-23 09:41:41
---

## Object.defineProperty()

方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

简单的说就是这个可以监听这个数组的获取操作和修改操作

存取描述符*是由 getter 函数和 setter 函数所描述的属性。

一个描述符只能是这两者其中之一不能同时是两者。

### 参数设置

**configurable**

当且仅当该属性的 `configurable` 键值为 `true` 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。

**enumerable**

当且仅当该属性的 `enumerable` 键值为 `true` 时，该属性才会出现在对象的枚举属性中。

**value**

该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。

**writable**

当且仅当该属性的 `writable` 键值为 `true` 时，属性的值，也就是上面的 `value`，才能被`赋值运算符`改变。

**get**

当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 `this` 对象（由于继承关系，这里的`this`并不一定是定义该属性的对象）。该函数的返回值会被用作属性的值。

**set**

当属性值被修改时，会调用此函数。该方法接受一个参数（也就是被赋予的新值），会传入赋值时的 `this` 对象。

### 描述符默认值汇总

- 拥有布尔值的键 `configurable`、`enumerable` 和 `writable` 的默认值都是 `false`。
- 属性值和函数的键 `value`、`get` 和 `set` 字段的默认值为 `undefined`。

### 示例

```js
    // 数据劫持
    let data = {
        message: "测试数据"
    }
    Object.defineProperty(data, 'message', {
        configurable: true,//默认 false
        enumerable: false, //默认 false
        get() {
            console.log("get...")
            return "测试数据"
        },
        set(newValue) {
            console.log("set...", newValue)
        }
    })
    
    data.message = "修改"
    delete data.message
```

## proxy

对象用于定义基本操作的自定义行为（如属性查找、赋值、枚举、函数调用等）。

整体来说和`Object.defineProperty()`的作用差不多 但是比它的功能更加多

### 常用方法

**get()**

用于拦截对象的读取属性操作。

- 访问属性: `proxy[foo]和` `proxy.bar`
- 访问原型链上的属性: `Object.create(proxy)[foo]`

约束：

```js
var obj = {};
Object.defineProperty(obj, "a", { 
  configurable: false, 
  enumerable: false, 
  value: 10, 
  writable: false 
});

var p = new Proxy(obj, {
  get: function(target, prop) {
    return 20;
  }
});

p.a; //会抛出TypeError
```

**set()**

设置属性值操作的捕获器。

- 指定属性值：`proxy[foo] = bar` 和 `proxy.foo = bar`
- 指定继承者的属性值：`Object.create(proxy)[foo] = bar`

约束

- 若目标属性是一个不可写及不可配置的数据属性，则不能改变它的值。
- 如果目标属性没有配置存储方法，即 `[[Set]]` 属性的是 `undefined`，则不能设置它的值。
- 在严格模式下，如果 `set()` 方法返回 `false`，那么也会抛出一个 `TypeError` 异常。

**deleteProperty()**

用于拦截对对象属性的 `delete` 操作。

删除属性: `delete proxy[foo]` 和 `delete proxy.foo`

**has()**

是针对 [`in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in) 操作符的代理方法。

这个钩子可以拦截下面这些操作:

- 属性查询: `foo in proxy`
- 继承属性查询: `foo in Object.create(proxy)`
- `with` 检查`: with(proxy) { (foo); }`

约束：

如果违反了下面这些规则,  proxy 将会抛出 `TypeError`:

- 如果目标对象的某一属性本身不可被配置，则该属性不能够被代理隐藏.
- 如果目标对象为不可扩展对象，则该对象的属性不能够被代理隐藏

下面的操作违反了规则

```js
var obj = { a: 10 };
Object.preventExtensions(obj);
var p = new Proxy(obj, {
  has: function(target, prop) {
    return false;
  }
});

'a' in p; // TypeError is thrown
```

### 示例

```js
    // proxy
    let obj = {
        name: "张三",
        age: 20
    }
    let newObj = new Proxy(obj, {
    		//获取
        get(target, key) {
            console.log("proxy1")
            return target[key]
        },
        //设置
        set(target, key, newValue) {
            target[key] = newValue
            console.log("proxy2")
            return true
        },
        //in操作
        has(target, key) {
            console.log("has")
            return true
        },
    })
    // newObj.name;
    // newObj.name = "李四"
    console.log("name" in newObj)
```

## 编写vue的数据渲染

### 需求

1. 将html里面以  `{{ }}` 的进行替换
2. 多层div嵌套式替换
3. 将标签的进行替换 v-html
4. 将v-model 为实时替换

整体就是模仿vue 的数据操作但是肯定会有很多bug 基础的东西可以了

```html
 <div id="app">
        {{message}}
        <div>
            <h1>{{message}}</h1>
        </div>
        <div v-html="htmlData"></div>
        <input v-model="modelData" value="" /> {{modelData}}
  </div>
```

```js
    let vm = new Kvue({
        el: '#app',
        data: {
            message: "测试数据",
            htmlData: "html数据",
            modelData: "绑定数据"
        }
    })
    setTimeout(() => {
        vm.$options.data.message = "修改数据"
    }, 1000)
```

完整代码

```js
    class Kvue extends EventTarget {
        constructor(options) {
            super()
            this.$options = options
            this.compile()
            this.observe(this.$options.data)
        }
        observe(data) {
            let keys = Object.keys(data)
            keys.forEach(key => {
                this.defineProperty(data, key, data[key])
            })
        }
        defineProperty(data, key, value) {
            let _this = this
            Object.defineProperty(data, key, {
                enumerable: true,
                configurable: true,
                get() {
                    console.log("get...")
                    return value
                },
                set(newValue) {
                    // let event = new Event(key)
                    let event = new CustomEvent(key, {
                        detail: newValue
                    })
                    _this.dispatchEvent(event)
                    console.log("set...")
                    value = newValue;
                }
            });
        }
        compile() {
            let el = document.querySelector(this.$options.el)
            this.compileNode(el)
        }
        compileNode(el) {
            let childNodes = el.childNodes
            childNodes.forEach(node => {
                if (node.nodeType === 1) {
                    // 标签 
                    let attrs = node.attributes;
                    console.log(attrs);
                    [...attrs].forEach(attr => {
                        console.log(attr)
                        let attrName = attr.name
                        let attrValue = attr.value
                        if (attrName.indexOf('v-') === 0) {
                            attrName = attrName.substr(2)
                            if (attrName === "html") {
                                node.innerHTML = this.$options.data[attrValue]
                            } else if (attrName === "model") {
                                node.value = this.$options.data[attrValue]
                                node.addEventListener('input', e => {
                                    // console.log(e.target.value)
                                    //因为监听
                                    this.$options.data[attrValue] = e.target.value
                                })
                            }
                        }
                    });
                    if (node.childNodes.length > 0) {
                        this.compileNode(node)
                    }
                } else if (node.nodeType === 3) {
                    // 文本
                    let reg = /\{\{\s*(\S+)\s*\}\}/g
                    let textContent = node.textContent
                    if (reg.test(textContent)) {
                        console.log(RegExp.$1)
                        let $1 = RegExp.$1
                        // node.textContent = this.$options.data[$1]
                        node.textContent = node.textContent.replace(reg, this.$options.data[$1])
                        this.addEventListener($1, e => {
                            // 重新渲染
                            // console.log(this.$options.data[$1])
                            // console.log(e.detail)
                            let oldValue = this.$options.data[$1]
                            let reg = new RegExp(oldValue);
                            node.textContent = node.textContent.replace(reg, e.detail)
                        })
                    }
                }
            });
        }
    }
```

