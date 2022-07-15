---
title: Vue-Router导航守卫使用方法的理解
categories: 技术笔记
tags:
  - Vue
  - Router
  - 导航守卫
excerpt: Vue-Router导航守卫使用方法的理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201107184108.png'
articleLink: 33363a25
date: 2020-11-07 18:16:18
---

## 导航守卫

就是通过导航守卫来控制路由变化的过程中跳转或取消的方式守卫导航

有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的

### 组件内守卫

定义在组件内的与路由有关的生命周期函数（守卫）

#### beforeRouteEnter

当路由解析完成，并中指定的组件渲染之前（组件 `beforeCreate`  之前），不能这里通过 `this` 访问组件实例，需要通过 `next` 回调来进行调用

因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```
beforeRouteEnter (to, from, next) {
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  	next(vm => {
      // vm...
    })
}
```

> 注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫

#### beforeRouteUpdate

在当前路由改变，但是该组件被复用时调用

```
beforeRouteUpdate (to, from, next) {
    // 可以访问组件实例 `this`
},
```

#### beforeRouteLeave

导航离开该组件的对应路由时调用

```
beforeRouteLeave (to, from, next) {
    // 可以访问组件实例 `this`
}
```

#### 路由守卫参数

`to`：即将要进入的目标 路由对象（$route）

`from`：当前导航正要离开的路由对象（$route）

`next`：路由确认回调函数，类似 <u>Promise</u> 中的 <u>resolve</u> 函数，一定要确保调用 <u>next</u> 函数，但是后续的导航行为将依赖 <u>next</u> 方法的调用参数

- **`next()`** : 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)
- **`next(false)`** : 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址

- **`next('/') 或者 next({ path: '/' })`** : 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航

### 路由独享的守卫

可以在路由配置上直接定义 `beforeEnter` 守卫，相对来说，应用不多

```
const router = new VueRouter(
  { 
    routes: [ 
      { 
        path: '/foo', 
        component: Foo, 
        beforeEnter: (to, from, next) => { 
          // ... 
      	}
    	} 
    ] 
  }
)
```

### 全局守卫

全局守卫是注册在 <u>router</u> 对象（new VueRouter({...})）上的

```
const router = new VueRouter({ ... })
```

#### 全局前置守卫 - beforeEach

当一个导航触发时，全局前置守卫按照创建顺序调用

```
router.beforeEach((to, from, next) => {
  // ...
})
```

#### 全局解析守卫 - beforeResolve

在所有组件内守卫和异步路由组件被解析之后被调用

```
router.beforeResolve((to, from, next) => {
  // ...
})
```

#### 全局后置钩子 - afterEach

导航被确认后调用

```
router.afterEach((to, from) => {
  // ...
})
```

> 因为导航已经被确认，所以没有 `next`



### 完整的导航解析流程完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。