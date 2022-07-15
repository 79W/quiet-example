---
title: Vue2数据响应式的原理defineProperty来实现数据响应式监控
categories: 技术笔记
tags:
  - vue
  - 数据响应式
excerpt: Vue2数据响应式的原理defineProperty来实现数据响应式监控
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201208143715.png'
articleLink: b8576712
date: 2020-12-08 14:16:18
---

### 数据响应原理

数据变更能够响应在视图中，就是数据响应式。

vue2中利用`Object.defineProperty()`实现变更检测。

![defineProperty](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201208141916565.png)

我们简单的实现一下 使用 `Object.defineProperty()`对一个对象数据的监控对获取和更改值的操作进行一个反应

```
function defineReactive(obj, key, val) {
    Object.defineProperty(obj, key, {
    		// 获取数据进行执行
        get() {
          console.log(`get ${key}:${val}`);
          return val
        },
        // 改变数据执行
        set(newVal) {
          if (newVal !== val) {
          console.log(`set ${key}:${newVal}`);
          val = newVal
        } 
      }
    }) 
}

// 初始化一个数据对象
let obj = {}
defineReactive(obj, 'foo', 'foo')
// 获取
obj.foo
// 改变
obj.foo = 'foooooooooooo'
```

我们对一个对象数据如何进行初始化监听内 

```
let obj2 = { foo: 'foo', bar: 'barr', bas: { a: 2 }}
```

我们需要遍历每一个 key 然后定义响应化

```
// 遍历
function observe(obj) {
    if (typeof obj !== 'object' || obj == null) {
        // 希望是一个对象obj
        return
    }
    Object.keys(obj).forEach(key => {
    	// 遍历每一个key 然后进行响应化监听
    	defineReactive(obj, key, obj[key])
    })
}
```

有人会发现我们进行修改bas 的a数据是不可以的 

因为我们没有进行递归进行遍历进行响应化我们只需在`set` 的时候在去执行 `observe`函数即可

我们需要改下 `defineReactive` 函数

```
//对象响应式
function defineReactive(obj, key, val) {
    // 递归 
    observe(val)
    // 对传入对obj 进行拦截
    Object.defineProperty(obj, key, {
        get() {
            console.log('get')
            return val
        },
        set(newVal) {
            if (newVal !== val) {
                console.log('set')
                // 有可能 传进来的还是个对象
                observe(newVal)
                val = newVal
            }
        }
    })
}
```

还有一种情况就是我们的数据不是对象而是数组我们怎么办

```
let obj2 = { foo: 'foo', bar: 'barr', bas: { a: 2 }, arr: [1, 2, 3, 4, 5] }
```

我们需要在往`arr`数组中 `push` 数据的时候也进行一个监听

```
obj2.arr.push(23)
```

#### 分析：

`Object.defineProperty 对数组是无效的`

- 改变数组的方法 只有 7个 我们只需要覆盖原型7个方法
- 我需要在push 后还要做一个 更新的动作
- 替换数组原型方法 让他们在修改数据同时还可以通知更新

```
// 数组响应式
// 替换数组7个原型的方法
const orginalProto = Array.prototype;
// 备份一下
const arrayProto = Object.create(orginalProto);
// 我就写了两个
let me = ['push', 'pop']
me.forEach(method => {
    arrayProto[method] = function () {
        // 原始的操作不能丢
        orginalProto[method].apply(this, arguments)
        // 覆盖 通知更新 
        console.log('arr这里你可以干很多事 比如通知更新')
    }
})
```

在遍历的时候我们也需要进行一个判断 是不是数组然后进行一个操作

```
// 遍历
function observe(obj) {
    if (typeof obj !== 'object' || obj == null) {
        // 希望是一个对象obj
        return
    }
			// 判断是不是数组
    if (Array.isArray(obj)) {
        // 覆盖原型方法 变更操作
        obj.__proto__ = arrayProto
        console.log(obj.__proto__)
        // 对数组内数据进行数据的响应化
        for (let i = 0; i < obj.length; i++) {
            console.log("数据", obj[i])
            observe(obj[i])
        }
    } else {
        Object.keys(obj).forEach(key => {
            defineReactive(obj, key, obj[key])
        })
    }

}
```

这样我们就简单的实现一个数据的监控

[完整代码](https://wwa.lanzous.com/iwIYjj5wkub)

