---
title: Vue-Router路由的基本用法
categories: 技术笔记
tags:
  - Vue
  - Router
excerpt: Vue-Router路由的基本用法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201102141659.png'
articleLink: 62f8aa8319fc3602
date: 2020-11-02 11:17:36
---

## 路由

#### 后端路由

根据不同的用户url请求返回不同的内容 

本质上是url请求地址与服务器资源之间的对应关系

![image-20201102112528568](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201102112528568.png)

#### 前端路由

根据不同的用户事件显示不同的页面内容

本质上是 用户事件与事件处理函数之间的对应关系

![image-20201102112350189](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201102112350189.png)

## VueRouter

### 快速使用

😘 第一步我们需要引入Vue 和 VueRouter

```
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

😇 第二步我们使用 `router-link` 来创建导航

`to`  属性指定链接 

`router-link`标签会被渲染成一个`a`标签

```
<router-link to="/foo">Go to Foo</router-link>
<router-link to="/bar">Go to Bar</router-link>
```

🤓 第三步我们需要给他定义一个路由的出口

也就说我说我们匹配到的组件渲染在哪里

```
<router-view></router-view>
```

以上就是我们在`html`内写的所有内容了

在`JavaScript`中我们需要进行实例化路由和定义路由我们继续

🧐 第四步我们定义两个组件

```
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }
```

🤩 第五步我们根据需要定义路由

每个路由应该映射一个组件 `component` 来接收组件

```
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]
```

🥳 第六步我们实例化`router` 然后传入`routes` 配置（配置有很多目前只是基础的）

```
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})
```

🥰 第七步我们在`app`根节点挂载实例

```
let vm = new Vue({
			el:'#app',
			// 挂载路由
			router
		})
```

### 路由重定向

有时候我们的需求是当你登陆了那么就不显示登陆页面了就直接到用户中心的页面

所以我们这时候需要使用冲定向进行跳转

重定向也是通过 `routes` 配置来完成，下面例子是从 `/a` 重定向到 `/b`：

```
 { path: '/a', redirect: '/b' }
```

### 路由嵌套

我们首先要确定谁是父组件谁谁被嵌套的

我们在父组件中写上路由规则

```
const Bar = { template: `<div>
			<h1>bar</h1>
			// 定义 a 标签 注意跳转链接
			<router-link to="/bar/tab1">tab1</router-link>
			<router-link to="/bar/tab2">tab2</router-link>
			// 给子路由渲染的位置
			<router-view></router-view>
		</div>` }
```

我们需要定义这两个子组件

```
const Tab1 = {template: '<div>tab1</div>'}
const Tab2 = {template: '<div>tab2</div>'}
```

✨然后我们最重要的地方 定义路由规则

我们需要在原有的路由规则 添加 `children` 属性

```
// 定义路由
const routes = [
    { path: '/foo', component: Foo },
    { path: '/bar', component: Bar ,children:[
        { path: '/bar/tab1', component: Tab1 },
        { path: '/bar/tab2', component: Tab2 },
    ]}
]
```

我们的父组件是 `Bar` 然后在里面嵌套了两个 子路由组件`Tab1` `Tab2`

![luyouqiantao](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgluyouqiantao.gif)

### 动态路由

我们先定义两个 `router-link`

```
<router-link to="/user/1">user1</router-link>
<router-link to="/user/2">user2</router-link>
```

我们访问的 url是以user开始/后面是一个动态的值

我们定义路由

```
const routes = [
		{ path: '/user/:id', component: User },
]
```

可以看出来我们路由定义的和之前不一样了 

我们这次给 `user/`后面加了个`:id` 

然后我我们在 user组件中接收这个 id 值

```
const User = { template: '<div>userID：{{$route.params.id}} </div>' }
```

我们是用`$route.params.id` 来进行接收的

### 路由传参

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

#### 布尔模式

如果 `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。

我们对上面的 User 组件进行改写

```
const User = {
  props: ['id'],
  template: '<div>UserID： {{ id }}</div>'
}
```

然后将我们的路由上添加一条 `props:true`

```
const routes = [
		{ path: '/user/:id', component: User, props: true },
]
```

#### 对象模式

如果 `props` 是一个对象，它会被按原样设置为组件属性。

当 `props` 是静态的时候有用。

```
const routes: [
    { path: '/user/id', component: User, props: { name: "qiaoBlog" } }
]
```

#### 函数模式

你可以创建一个函数返回 `props`。

这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。

```
const routes: [
    { path: '/search', component: SelectUser, props: (route) => ({ query: route.query.q }) }
]
```

URL `/search?q=vue` 会将 `{query: 'vue'}` 作为属性传递给 `SearchUser` 组件。

请尽可能保持 `props` 函数为无状态的，因为它只会在路由发生变化时起作用。

如果你需要状态来定义 `props`，请使用包装组件，这样 Vue 才可以对状态变化做出反应。

### 命名路由

你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。

```
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

我们将上面的路由 的名字定义为了`user`

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象：

```
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

`params` 里面的参数要和你在路由里面定义的要一致

### 编程式导航

除了使用 `<router-link>` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

#### `router.push(location, onComplete?, onAbort?)`

> 注意：在 Vue 实例内部，你可以通过 `$router` 访问路由实例。因此你可以调用 `this.$router.push`。

这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。

🍣 **声明式**： `<router-link :to="...">`

🍤 **编程式**：`router.push(...)`

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。

```
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})
```

#### `router.go(n)`

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

