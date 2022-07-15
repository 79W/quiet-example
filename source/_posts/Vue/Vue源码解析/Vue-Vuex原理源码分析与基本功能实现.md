---
title: Vue-Vuex原理分析与基本功能实现
categories: 技术笔记
tags:
  - vue
  - vuex
excerpt: Vue-Vuex原理源码分析与基本功能实现
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201207164115.png'
articleLink: 1bea8174
date: 2020-12-07 16:18:20
---

## vuex原理解析

我们首先来分析一下

- 创建响应式的state，保存mutations、actions和getters
- 实现commit根据用户传入type执行对应mutation
- 实现dispatch根据用户传入type执行对应action，同时传递上下文
- 实现getters，按照getters定义对state做派生

### 快速开始 

我们我需要先导出一个 Store声明 和一个 install

```
let Vue;
class Store {
    constructor(options) {
    		// 初始化 我们将值 进行响应化
    		//（也就是怎么改这个值进行页面的刷新）
        this.state = new Vue({
            data: options.state
        })
    }
}

function install(_Vue) {
    Vue = _Vue
    Vue.mixin({
        beforeCreate() {
        // 这里还是采用一个混入 只有根才能拿到这个 store 实例
            if (this.$options.store) {
            // 配置全局
                Vue.prototype.$store = this.$options.store
            }
        }
    })
}
// 导出
export default {
    Store,
    install
}
```

目前我们可以在页面上显示数据了但是还不可以使用也就是说我们不可以进行更改数据

接下来我们来编写修改数据的方法

#### commit

```
// type 类型
// payload 参数
commit(type, payload) {
  const entry = this._mutations[type]
  if (entry) {
  	entry(this.state, payload)
  }
}
```

`_mutations` 我们需要在 `Store`中声明

```
this._mutations = options.mutations
```

这样我们就可以使用 commit 方法来进行修改数据了

#### dispatch

```
dispatch(type, payload) {
  const entry = this._actions[type]
  if (entry) {
  		// 这里的 this 会乱 上下文会是win
  		entry(this, payload)
  }
}
```

`_actions` 我们需要在 `Store`中声明

```
this._actions = options.actions
```

当我们写完这个时候 会有一个报错

```
Uncaught TypeError: Cannot read property '_mutations' of undefined
    at commit (Qvuex.js?ec80:16)
    at eval (index.js?3738:23)
```

![vuex](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201207162806710.png)

这里的问题是 我们的 `this`上下文乱了我们需要在上面使`bind`来指定`this`的指向

### 实例代码

```
let Vue;
class Store {
    constructor(options) {
        this._mutations = options.mutations
        this._actions = options.actions
        this.state = new Vue({
            data: options.state
        })
        // 绑定 commit dispatch 的上下文 store
        this.commit = this.commit.bind(this)
        this.dispatch = this.dispatch.bind(this)
    }
    // type 类型
    // payload 参数
    commit(type, payload) {
        const entry = this._mutations[type]
        if (entry) {
            entry(this.state, payload)
        }
    }
    // 
    dispatch(type, payload) {
        const entry = this._actions[type]
        if (entry) {
            // 这里的 this 会乱 上下文会是win
            entry(this, payload)
        }
    }
}

function install(_Vue) {
    Vue = _Vue
    Vue.mixin({
        beforeCreate() {
            if (this.$options.store) {
                Vue.prototype.$store = this.$options.store
            }
        }
    })
}

export default {
    Store,
    install
}
```

这里还有很多需要进行优化目前只是做了一个简单版的演示

#### 如：

- 阻止用户直接进行修改`state` 数据
- getters 如何实现？（类似计算属性）
- 。。。。

[资源下载](https://wwa.lanzous.com/iLtlXj4uqhg)