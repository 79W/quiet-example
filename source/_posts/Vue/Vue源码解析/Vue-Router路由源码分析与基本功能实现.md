---
title: Vue-Router路由源码分析与基本功能实现
categories: 技术笔记
tags:
  - vue
  - router
excerpt: Vue-Router路由源码分析与基本功能实现
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201207163755.png'
articleLink: 88b3b824
date: 2020-12-07 15:37:33
---

## VueRouter实现原理

首先我们先进行分析Routr的基本需求

- 作为一个插件存在:实现VueRouter类和install方法
- 实现两个全局组件:router-view用于显示匹配组件内容，router-link用于跳转
- 监控url变化:监听hashchange或popstate事件
- 响应最新url:创建一个响应式的属性current，当它改变时获取对应组件并显示

## 个人理解

我理解的vueRouter就是根据utl字符的变化然后我们进行判断从而改变页面内容

而有两个基础的组件 `router-link` 用于跳转 还有一个就是 `router-view`用于显示挑战后的指定模版

#### 知识点

- mixin 混入
- install 插件
- 操作url
- util（工具）
- render 渲染

## 快速开始

我们创建一个自己命名的文件夹我们来写

```
let Vue; // 引用构造函数，VueRouter中要使用
// 保存选项
class VueRouter {
  constructor(options) {
    this.$options = options;
	} 
}
// 插件:实现install方法，注册$router 
VueRouter.install = function(_Vue) {
  // 引用构造函数，VueRouter中要使用 Vue = _Vue;
    Vue.mixin({
      beforeCreate() {
  // 只有根组件拥有router选项
    if (this.$options.router) {
          // vm.$router
          Vue.prototype.$router = this.$options.router;
        }
    } 
  });
};
export default VueRouter;
```

> 为什么要用混入方式写?主要原因是use代码在前，Router实例创建在后，而install逻辑又需要用 到该实例

### router-view

```
export default {
 	render(h) {
      // 获取指定 path 对应的 component
      const { routeMap, current } = this.$router
      let component = routeMap[current].component || null
      return h(component)
    }
  })
 }
```

### router-link

```
export default {
 	props: {
      to: {
        type: String,
        required: true
      },
    },
    render(h) {
      return h('a', { attrs: { href: '#' + this.to } }, this.$slots.default)
    } 
 }
```

### 监听url变化

定义响应式的current属性，监听hashchange事件

```
Vue.util.defineReactive(this, 'current', '/')
// 监听url 变化
window.addEventListener('hashchange', this.onHashChange.bind(this))
window.addEventListener('load', this.onHashChange.bind(this))
// 这里写个路由映射表
    this.routeMap = {}
    options.routes.forEach(route => {
      this.routeMap[route.path] = route
    });

  }
  onHashChange() {
    // 这里需要你去截取 url
    // console.log("看看：", window.location.hash.split('/')[1])
    // this.current = '/' + window.location.hash.split('/')[1]
    this.current = window.location.hash.slice(1)
  }
```

### 动态获取对应组件

```
export default {
  render(h) {
// 动态获取对应组件
let component = null; 
this.$router.$options.routes.forEach(route => {
      if (route.path === this.$router.current) {
        component = route.component
} });
    return h(component);
  }
}
```

**提前处理路由表**

提前处理路由表避免每次都循环

```
class VueRouter {
  constructor(options) {
// 缓存path和route映射关系
this.routeMap = {} this.$options.routes.forEach(route => {
      this.routeMap[route.path] = route
    });
} }
```

### 完整代码

```
let Vue;
// 实现一个插件 挂载 router
class QVueRouter {
  constructor(options) {
    this.options = options
    // 需要创建响应式的current 
    // this.current = '/'
    Vue.util.defineReactive(this, 'current', '/')
    // 监听url 变化
    window.addEventListener('hashchange', this.onHashChange.bind(this))
    window.addEventListener('load', this.onHashChange.bind(this))

    // 这里写个路由映射表
    this.routeMap = {}
    options.routes.forEach(route => {
      this.routeMap[route.path] = route
    });

  }
  onHashChange() {
    // 这里需要你去截取 url
    // console.log("看看：", window.location.hash.split('/')[1])
    // this.current = '/' + window.location.hash.split('/')[1]
    this.current = window.location.hash.slice(1)
  }
}
QVueRouter.install = function (_Vue) {
  // 保存构造函数 
  Vue = _Vue
  // 挂载router
  // 如何获取根节点获取router
  Vue.mixin({
    // 会在所有的组件进行执行
    beforeCreate() {
      // 确保根实例在进行执行
      if (this.$options.router) {
        Vue.prototype.$router = this.$options.router
      }
    }
  })
  // router-link 
  Vue.component('router-link', {
    props: {
      to: {
        type: String,
        required: true
      },
    },
    render(h) {
      return h('a', { attrs: { href: '#' + this.to } }, this.$slots.default)
    }
  })
  // router-view
  Vue.component('router-view', {
    render(h) {
      // 获取指定 path 对应的 component
      const { routeMap, current } = this.$router
      let component = routeMap[current].component || null
      // this.$router.options.routes.forEach(route => {
      //   if (route.path === this.$router.current) {
      //     component = route.component
      //   }
      // })
      return h(component)
    }
  })
}

export default QVueRouter
```

> 缺少 路由嵌套问题没有解决 这可以参考官方源码

此文章只是说明了简单的vue路由的实现

只实现了 可以使用 link进行跳转 和使用 view 进行渲染（不支持嵌套渲染）

[资源下载](https://wwa.lanzous.com/iLtlXj4uqhg)