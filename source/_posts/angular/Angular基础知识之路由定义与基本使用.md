---
title: Angular基础知识之路由定义与基本使用
categories: 技术笔记
tags:
  - Angular
  - 路由
excerpt: Angular基础知识之路由定义与基本使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201213154246.png'
articleLink: fcac8882
date: 2020-12-13 14:19:20
---

## 路由

我们需要安装项目的时候选择上`router` 或者我们后期进行安装路由插件

```
// 生成一个带路由模块的 应用
ng new routing-app --routing
```

### 定义基本路由

我们会看到 Src 目录下面有 `app-routing.module.ts` 文件

我们可以在 `const routes: Routes = [];` 数组中进行定义路由对象

```
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
];
```

- 第一个属性 `path` 定义了该路由的 URL 路径。
- 第二个属性 `component` 定义了要让 Angular 用作相应路径的组件。

我们在页面中使用`router-outlet` 来接收路由组件的内容

```
<!-- The routed views render in the <router-outlet>-->
<router-outlet></router-outlet>
```

路由的跳转 使用 `routerLink` 来进行跳转

```
  <ul>
    <li><a routerLink="/first-component" routerLinkActive="active">First Component</a></li>
    <li><a routerLink="/second-component" routerLinkActive="active">Second Component</a></li>
  </ul>
```

### 动态路由

我们可以设置动态路由比如新闻页需要传递id 

```
{ path: 'news/:id', component: News }
```

### 路由信息

我们可以在这里接受传递的参数锚点等数据

我们需要在组件中导入路由插件

```
import { Router, ActivatedRoute, ParamMap } from '@angular/router';
```

然后我们注册

```
constructor(
  private route: ActivatedRoute,
) {}
```

然后我们就可以在各种地方获取路由的信息了

```
ngOnInit() {
  this.route.queryParams.subscribe(params => {
    this.name = params['name'];
  });
}
```

### 路由顺序

路由是从`routes` 数组中从上到下进行匹配的如果没有匹配到我们可以使用一个通配符来进行跳转404

```
{ 
	path: '**', 
	component: page404   // Wildcard route for a 404 page
}
```

这两个星号 `**` 告诉 Angular，这个 `routes` 定义是通配符路由

通配符路由是最后一个路由，因为它匹配所有的 URL。

### 设置重定向

```
{ path: '',   redirectTo: '/first-component', pathMatch: 'full' }
```

当我们访问根路由的时候会重定向到 `first-component` 路由

这里的 `path: ''` 表示使用初始的相对 URL（ `''` ）

重定向路由需要一个 `pathMatch` 属性，来告诉路由器如何用 URL 去匹配路由的路径

默认路由应该只有在整个URL 等于 `''` 时才重定向到 `first-component`

### 嵌套路由

**创建组件**

我们在这个 组件中我们设置了两个 `routerLink` 来进行跳转

并且使用 `router-outlet` 来接收子组件的内容

```
<h2>First Component</h2>

<nav>
  <ul>
    <li><a routerLink="child-a">Child A</a></li>
    <li><a routerLink="child-b">Child B</a></li>
  </ul>
</nav>

<router-outlet></router-outlet>
```

**配置路由 **

这是一个嵌套子路由我们需要在当前组件中设置`children` 数组

里面还是接收对象来配置

```
const routes: Routes = [
  {
    path: 'first-component',
    component: FirstComponent, 
    children: [
      {
        path: 'child-a', 
        component: ChildAComponent, 
      },
      {
        path: 'child-b',
        component: ChildBComponent, 
      },
    ],
  },
];
```

我们配置好后 因为 `child-a` 和 `child-b` 是`first-component` 路由的子路由 所以我们访问的时候是`/first-component/child-a` 进行访问的 



