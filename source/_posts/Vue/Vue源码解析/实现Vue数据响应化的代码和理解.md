---
title: 实现Vue数据响应化的代码和理解
categories: 技术笔记
tags:
  - vue
  - 数据响应式
excerpt: 实现Vue数据响应化的代码和理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg/img20200923094246.png'
articleLink: 5f3fb704
date: 2020-12-08 14:38:48
---

### 实现效果

![shujuxiangyihua](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/shujuxiangyihua.gif)

实现数据的响应式渲染 在html中使用双大括号进行渲染数据可以使用类似v-text等指令

## Vue原理分析

- `new Vue()` 首先执行初始化，对data执行响应化处理，这个过程发生在`Observer`中

- 同时对模板执行编译，找到其中动态绑定的数据，从data中获取并初始化视图，这个过程发生在

  `Compile`中

- 同时定义一个更新函数和`Watcher`，将来对应数据变化时Watcher会调用更新函数

- 由于data的某个key在一个视图中可能出现多次，所以每个key都需要一个管家Dep来管理多个

  `Watcher`

- 将来`data`中数据一旦发生变化，会首先找到对应的`Dep`，通知所有`Watcher`执行更新函数

![vue原理](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201208145809520.png)

### 涉及类型介绍

- QVue:框架构造函数
- Observer:执行数据响应化(分辨数据是对象还是数组) 
- Compile:编译模板，初始化视图，收集依赖(更新函数、watcher创建) 
- Watcher:执行更新函数(更新dom)
- Dep:管理多个Watcher，批量更新

### 构造函数

我们先创建我们的QVue构造函数

作用就是执行初始化，对data执行响应化处理

```
class QVue {
    constructor(options) {
        // 保存选项
        this.$options = options
        this.$data = options.data
        // 响应化处理
        observe(this.$data)
    }

}
```

`observe`我们需要对数据进行一个遍历然后进行全部的响应化处理

```
//  根据类型决定如何做响应化
class Observer {
    constructor(value) {
        this.value = value
        // 判断类型
        if (typeof value === 'object') {
            this.walk(value)
        }
    }
    // 对象数据响应化
      Object.keys(value).forEach(key => {
     		 defineReactive(value, key, value[key])
      })
  
    // 数组数据响应化（请看上一篇）
}

```

`defineReactive`使用`Object.defineProperty()`进行数据的响应化

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

我们这时候就可以去访问我们的页面了发现数据显示出来了

然后我们发现更改数据的时候不对

```
// 我们需要带上$data 才能访问到数据
app.$data.coun
```

我们需要进行数据的代理然后进行一个数据访问

```
// 代理函数
function proxy(vm, sourceKey) {
    Object.keys(vm[sourceKey]).forEach((key) => {
        // 将 $data中的key 代理到vm 属性中 
        Object.defineProperty(vm, key, {
            get() {
                return vm[sourceKey][key]
            },
            set(newValue) {
                vm[sourceKey][key] = newValue
            }
        })
    })
}
```

### 编译 Compile

![Compile](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg/image-20201208153114649.png)

```
// 编译器
// 递归遍历dom 树
// 判断节点类型  如果是文本 侧判断是不是 插值绑定
// 如果元素 则遍历其属性判断是否是指令或者是事件然后递归

class Compile {
    // el 是宿主元素
    // vm是qvue 实例 
    constructor(el, vm) {
        this.$vm = vm
        this.$el = document.querySelector(el)
        if (this.$el) {
            // 编译
            this.compile(this.$el)
        }
    }

    compile(el) {
        // 遍历 el 树
        const childNodes = el.childNodes
        Array.from(childNodes).forEach(node => {
            if (this.isElement(node)) {
                console.log("编译元素")
                this.compileElement(node)
            } else if (this.isInter(node)) {
                console.log("编译插值表达式")
                this.compileText(node)
            }
            // 递归 子节点
            if (node.childNodes && node.childNodes.length > 0) {
                this.compile(node)
            }
        });
    }

    isElement(node) {
        return node.nodeType === 1
    }
    isInter(node) {
        // 首先是 文本 然后 是 {{xxxx}}
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent)
    }

    // 
    compileText(node) {

        this.update(node, RegExp.$1, 'text')
    }

    compileElement(node) {
        //遍历 节点属性
        const nodeAttrs = node.attributes
        Array.from(nodeAttrs).forEach((attr) => {
            // 规定指令 q-xx
            const attrName = attr.name
            const exp = attr.value
            if (this.isDirective(attrName)) {
                const dir = attrName.substring(2)
                this[dir] && this[dir](node, exp)
            }
        })
    }

    isDirective(attr) {
        return attr.indexOf('q-') === 0
    }

    //  更新
    update(node, exp, dir) {
        // 初始化
        // 更新函数 xxupdate
        const fn = this[dir + 'Updater']
        fn && fn(node, this.$vm[exp])

        // 更新 
        // 封装更新函数 可以更新dom 元素
        new Watcher(this.$vm, exp, function (val) {
            fn && fn(node, val)
        })
    }

    textUpdater(node, value) {
        node.textContent = value
    }

    // q-text
    text(node, exp) {
        this.update(node, exp, 'text')
    }

}
```

### 依赖收集

视图中会用到data中某key，这称为依赖。

同一个key可能出现多次，每次都需要收集出来用一个 Watcher来维护它们，此过程称为依赖收集。

多个Watcher需要一个Dep来管理，需要更新时由Dep统一通知。 

看下面案例，理出思路:

```
new Vue({
    template:
        `<div>
            <p>{{name1}}</p>
            <p>{{name2}}</p>
            <p>{{name1}}</p>
         <div>`,
    data: {
        name1: 'name1',
        name2: 'name2'
    }
});
```

![Watcher](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201208153411545.png)

#### 思路

- defineReactive时为每一个key创建一个Dep实例
- 初始化视图时读取某个key，例如name1，创建一个watcher1
- 由于触发name1的getter方法，便将watcher1添加到name1对应的Dep中
- 当name1更新，setter触发时，便可通过对应Dep通知其管理所有Watcher更新

#### 创建watcher 

```
const watchers = [];//临时用于保存watcher测试用
// 监听器:负责更新视图 class Watcher {
constructor(vm, key, updateFn) { // kvue实例
this.vm = vm;
// 依赖key
this.key = key;
// 更新函数
this.updateFn = updateFn;
// 临时放入watchers数组
    watchers.push(this)
  }
// 更新 update() {
    this.updateFn.call(this.vm, this.vm[this.key]);
  }
}
```

在更新函数中进行创建 watcher

```
// 调用update函数执插值文本赋值 compileText(node) {
    // console.log(RegExp.$1);
    // node.textContent = this.$vm[RegExp.$1];
    this.update(node, RegExp.$1, 'text')
}
text(node, exp) {
this.update(node, exp, 'text') 
}
html(node, exp) {
    this.update(node, exp, 'html')
}
update(node, exp, dir) {
    const fn = this[dir+'Updater']
    fn && fn(node, this.$vm[exp])
    new Watcher(this.$vm, exp, function(val){
        fn && fn(node, val)
    })
}
textUpdater(node, val) {
    node.textContent = val;
}
htmlUpdater(node, val) {
    node.innerHTML = val
}
```

#### 声明Dep

```
class Dep {
    constructor () {
        this.deps = []
    }
    addDep (dep) {
        this.deps.push(dep)
}
    notify() {
        this.deps.forEach(dep => dep.update());
} }
```

创建watcher时触发getter

```
class Watcher {
  constructor(vm, key, updateFn) {
    Dep.target = this;
    this.vm[this.key];
    Dep.target = null;
} }
```

依赖收集，创建Dep实例

```
defineReactive(obj, key, val) {
  this.observe(val);
const dep = new Dep()
Object.defineProperty(obj, key, {
    get() {
      Dep.target && dep.addDep(Dep.target);
return val },
    set(newVal) {
      if (newVal === val) return
      dep.notify()
} })
}
```

### 个人整体思路：

- 第一步你需要先去解析数据然后让他显示在页面上面
- 使用 `Object.defineProperty`对数据进行一个响应化处理
- 对属性进行解析
- 重点监听watcher和Dep管理
- 更新数据

[完整代码](https://wwa.lanzous.com/iwIYjj5wkub)

