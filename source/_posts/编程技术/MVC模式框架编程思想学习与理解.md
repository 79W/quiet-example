---
title: MVC模式框架编程思想学习与理解
categories: 技术笔记
tags:
  - MVC
excerpt: MVC模式框架编程思想学习与理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200930105522.png'
articleLink: f00c740c20f23330
date: 2020-09-30 10:21:33
---

## MVC模式

MVC 模式代表 Model-View-Controller（模型-视图-控制器） 模式。

这种模式用于应用程序的分层开发。

- **Model（模型）** - 模型代表一个存取数据的对象或 JAVA POJO。它也可以带有逻辑，在数据变化时更新控制器。
- **View（视图）** - 视图代表模型包含的数据的可视化。
- **Controller（控制器）** - 控制器作用于模型和视图上。它控制数据流向模型对象，并在数据变化时更新视图。它使视图与模型分离开。

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1200px-ModelViewControllerDiagram2.svg_-20200930102821862.png)



**总结**

MVC是一种编程思想可以不完全按照这个文件夹命名你可以有自己的文件 比如你把路由分出来等

总体分为三块 这三块是紧密联系在一起的，但又是互相独立的，每一层内部的变化不影响其他层。

每一层都对外提供接口（Interface），供上面一层调用。

这样一来，软件就可以实现模块化，修改外观或者变更数据都不用修改其他层，大大方便了维护和升级。

**优点**

- 耦合性低
- 重用性高
- 部署快，生命周期成本低
- 可维护性高

**缺点**

- 完全理解MVC比较复杂
- 调试困难
- 不适合小型，中等规模的应用程序
- 增加系统结构和实现的复杂性
- 视图与控制器间的过于紧密的连接并且降低了视图对模型数据的访问

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200930104835.png)

