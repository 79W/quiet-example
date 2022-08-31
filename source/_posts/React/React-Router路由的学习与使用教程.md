---
title: React-Router路由的学习与使用教程
categories: 技术笔记
tags:
  - React
  - Router
  - 路由
excerpt: React-Router路由的学习与使用教程
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201025092701.png'
articleLink: a686bc074df42625
date: 2020-10-25 09:26:26
---

## 路由

当应用变得复杂的时候，就需要分块的进行处理和展示，传统模式下，我们是把整个应用分成了多个页面，然后通过 URL 进行连接。

通俗的说就是 访问不同的url展示的数据是不相同的

### SPA

Single Page Application : 单页面应用，整个应用只加载一个页面（入口页面），后续在与用户的交互过程中，通过 DOM 操作在这个单页上动态生成结构和内容.

**优点：**

- 有更好的用户体验（减少请求和渲染和页面跳转产生的等待与空白），页面切换快
- 重前端，数据和页面内容由异步请求（AJAX）+ DOM 操作来完成，前端处理更多的业务逻辑

**缺点：**

- 首屏处理慢
- 不利于 SEO

## React Router

我们现在学习 基于 Web 的路由库 [react-router-dom ](https://reactrouter.com/web/guides/quick-start)  

#### 安装

```
npm i react-router-dom
```

### Router

如果我们希望页面中某个部分的内容需要根据 URL 来动态显示

需要用到 Router 组件 ，该组件是一个容器组件

只需要用它包裹 URL 对应的根组件即可

而 Router 为我们提供了两种 方式 `BrowserRouter` 和 `HashRouter`

```
import { BrowserRouter } from 'react-router-dom'

ReactDOM.render(
  <BrowserRouter>
    //内容
  </BrowserRouter>,
  document.getElementById('root')
);
```

也就是说我们哪里需要使用路由进行替换的地方就进行嵌套

而 `BrowserRouter` 和 `HashRouter` 这两个的区别就是在路由上面 带不带#的区别

开发中更多的是使用 `BrowserRouter`

### Route 组件

通过该组件来设置应用单个路由信息，Route 组件所在的区域就是就是当 URL 与当前 Route 设置的 path 属性匹配的时候，后面 component 将要显示的区域

通俗的说就是 你访问这个 url了 我需要给你返回什么东西 

```
import React, { useState } from 'react';
import { Link, Route, Switch } from 'react-router-dom'
//引入页面
import Index from './view/index'
function App(props) {
    return (
        <div className="App">
        		// 定义路由
             <Route path='/myindex' component={Index} />
        </div>
    );
}
export default App;
```

在上面代码可以看出来 当访问 `http://localhost:3000/myindex` 

就可以看到 我们所导入的页面内容 

#### path: string

可以是 path-to-regexp 能够理解的任何有效的 URL 路径 (用户访问的url路由 )

```
<Route path="/users" component={User} />
```

没有定义 `path` 的 `<Route>` 总是会被匹配。

#### component

指定只有当位置匹配时才会渲染的 React 组件，该组件会接收 route props作为属性。

```
const User = ({ match }) => {
  return <h1>Hello {match.params.username}!</h1>
}

<Route path="/user/:username" component={User} />
```

#### render: func

使用 `render` 可以方便地进行内联渲染和包装，而无需进行上文解释的不必要的组件重装。

你可以传入一个函数，以在位置匹配时调用，而不是使用 `component` 创建一个新的 React 元素。

`render` 渲染方式接收所有与 `component` 方式相同的 route props。

```
// 方便的内联渲染
<Route path='/myindex' render={(props) => (
		<Index {...props} />
)} />
```

> 警告：`<Route component>` 优先于 `<Route render>`，因此不要在同一个 `<Route>` 中同时使用两者。

#### children: func

有时候不论 `path` 是否匹配位置，你都想渲染一些内容。在这种情况下，你可以使用 `children` 属性。除了不论是否匹配它都会被调用以外，它的工作原理与 `render` 完全一样

`children` 渲染方式接收所有与 `component` 和 `render` 方式相同的 route props

除非路由与 URL 不匹配，不匹配时 `match` 为 `null`

这允许你可以根据路由是否匹配动态地调整用户界面

```
const ListItemLink = ({ to, ...rest }) => (
	// 这里使用一个 children
  <Route path={to} children={({ match }) => (
  	// 这里进行一个判断 是否为null
    <li className={match ? 'active' : ''}>
      <p>我是有无数据<p>
    </li>
  )} />
)
```

> 警告：`<Route component>` 和 `<Route render>` 优先于 `<Route children>`，因此不要在同一个 `<Route>` 中同时使用多个。

#### exact: bool

如果为 `true`，则只有在 `path` 完全匹配 `location.pathname` 时才匹配。

```
<Route exact path="/admin" component={OneComponent} />
```

通俗的来说就是 如果没有 开启 `exact` 那么你访问 `/admin/info` 也会携带着 `/admin` 的内容

如果你加上了 `exact`  那么 `/admin`  就是自己    `/admin/info` 也不会渲染  `/admin`  的东西

```
<Route path='/myindex' render={(props) => (
		<Index {...props} />
)} />
<Route path='/myindex/info' render={(props) => (
		<About {...props} />
)} />
```

我们定义了两个路由分别是 `/myindex`  和 `/myindex/info`

我们先访问 一个查看

![react-router](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201025082843577.png)

打印的结果没有错 那么我们在访问 第二个路由看看是什么结果

![react-router](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201025083158224.png)

我们发现 你访问成功 属于 `/myindex` 的内容也打印出来了 这就是没有添加 `exact` 的结果。

那么我们给 `/myindex` 路由添加上 `exact` 在看看 是什么结果

```
<Route path='/myindex' exact render={(props) => (
			<Index {...props} />
)} />
<Route path='/myindex/info' render={(props) => (
			<About {...props} />
)} />
```

我们直接访问 `/myindex/info ` 看看 还有没有 `myindex` 的内容了

![react-router](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201025083520662.png)

这时候就没有 `/myadmin` 的内容了

### Link 组件

为你的应用提供声明式的、可访问的导航链接。

```
import { Link } from 'react-router-dom';
```

```
 <Link to='/'>首页</Link>
 <span>｜</span>
 <Link to='/about'>关于我们</Link>
 <span>｜</span>
 <Link to='/join'>加入我们</Link>
 <span>｜</span>
 <Link to='/login'>登陆</Link>
```

![react-router-link](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201025083909708.png)

#### to: string

一个字符串形式的链接地址，通过 `pathname`、`search` 和 `hash` 属性创建

```
<Link to='/courses?sort=name' />
```

#### to: object

一个对象形式的链接地址，可以具有以下任何属性：

- `pathname` - 要链接到的路径
- `search` - 查询参数
- `hash` - URL 中的 hash，例如 #the-hash
- `state` - 存储到 location 中的额外状态数据

```
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: {
    fromDashboard: true
  }
}} />
```

#### others

你还可以传递一些其它属性，例如 `title`、`id` 或 `className` 等。

```
<Link to="/About" className="nav" title="a标签title">About</Link>
```

### NavLink 组件

一个特殊版本的 `<Link>`，它会在与当前 URL 匹配时为其呈现元素添加样式属性。

```
import { NavLink } from 'react-router-dom';

<NavLink to="/about">About</NavLink>
```

NavLink 与 Link 类似，但是它提供了两个特殊属性用来处理页面导航

#### activeStyle: object

当前 URL 与 NavLink 中的 to 匹配的时候，激活 activeStyle 中的样式

```
const activeStyle = {
  fontWeight: 'bold',
  color: 'red'
};

<NavLink to="/faq" activeStyle={activeStyle}>FAQs</NavLink>
```

#### activeClassName: string

当元素处于激活状态时应用的类，默认为 `active`。

它将与 `className` 属性一起使用。

与 activeStyle 类似，但是激活的是 className

```
<NavLink to="/faq" activeClassName="selected">FAQs</NavLink>
```

#### isActive: func

添加额外逻辑以确定链接是否处于激活状态的函数。如果你要做的不仅仅是验证链接的路径名与当前 URL 的路径名相匹配，那么应该使用它。

默认情况下，匹配的是 URL 与 to 的设置，通过 isActive 可以自定义激活逻辑，isActive 是一个函数，返回布尔值。

```
// 只有当事件 id 为奇数时才考虑激活
const oddEvent = (match, location) => {
  if (!match) {
    return false;
  }
  const eventID = parseInt(match.params.eventID);
  return !isNaN(eventID) && eventID % 2 === 1;
}

<NavLink to="/events/123" isActive={oddEvent}>Event 123</NavLink>
```

#### exact: bool

如果为 `true`，则只有在位置完全匹配时才应用激活类/样式。

```
<NavLink exact to="/profile">Profile</NavLink>
```

### Switch组件

该组件只会渲染首个被匹配的组件

`<Switch>` 只会渲染一个路由

仅仅定义一系列 `<Route>` 时，每一个与路径匹配的 `<Route>` 都将包含在渲染范围内

```
<Route path="/about" component={About} />
<Route path="/:user" component={User} />
<Route component={NoMatch} />
```

如果 URL 是 `/about`，那么 `<About>`、`<User>` 和 `<NoMatch>` 将全部渲染，因为它们都与路径匹配

这是通过设计，允许我们以很多方式将 `<Route>` 组合成我们的应用程序

例如侧边栏和面包屑、引导标签等。

但是，有时候我们只想选择一个 `<Route>` 来呈现。比如我们在 URL 为 `/about` 时不想匹配 `/:user`（或者显示我们的 404 页面）

```
import { Switch, Route } from 'react-router';

<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/about" component={About} />
  <Route path="/:user" component={User} />
  <Route component={NoMatch} />
</Switch>
```

现在，当我们在 `/about` 路径时，`<Switch>` 将开始寻找匹配的 `<Route>`

我们知道，`<Route path="/about" />` 将会被正确匹配，这时 `<Switch>` 会停止查找匹配项并立即呈现 `<About>` 

### Redirect组件

使用 `<Redirect>` 会导航到一个新的位置。

的位置将覆盖历史堆栈中的当前条目，例如服务器端重定向（HTTP 3xx）。

```
import { Route, Redirect } from 'react-router-dom';

<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard" />
  ) : (
    <PublicHomePage />
  )
)} />
```

#### to: string

要重定向到的 URL，可以是 path-to-regexp 能够理解的任何有效的 URL 路径。

```
<Redirect to="/somewhere/else" />
```

### withRouter组件

如果一个组件不是路由绑定组件，那么该组件的 props 中是没有路由相关对象的，

虽然我们可以通过传参的方式传入，但是如果结构复杂，这样做会特别的繁琐。

幸好，我们可以通过 withRouter 方法来注入路由对象

把不是通过路由切换过来的组件中，将react-router 的 history、location、match 三个对象传入props对象上

 导入我们的 `withRouter`

```
import {  Route, withRouter } from 'react-router-dom'
```

定义一个组件

```
function About(props) {
    console.log(props)
    return (
        <p>详细信息</p>
    );
}
```

然后使用 `withRouter` 进行包裹

```
About = withRouter(About)
```

然后我们直接调用就可以了

```
<Route path='/about' component={About} />
```

然后我们可以看到 控制台已经打印出来了

![withRouter](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201025092307410.png)