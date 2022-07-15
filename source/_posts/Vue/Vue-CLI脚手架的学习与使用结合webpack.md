---
title: Vue-CLI脚手架的学习与使用结合webpack
categories: 技术笔记
tags:
  - Vue
  - Cli
  - 脚手架
excerpt: Vue-CLI脚手架的学习与使用结合webpack
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201103150605.png'
articleLink: 35af160494493603
date: 2020-11-03 10:33:36
---

### 单文件组件

我们面临的问题

- **全局定义 (Global definitions)** 强制要求每个 component 中的命名不得重复
- **字符串模板 (String templates)** 缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 `\`
- **不支持 CSS (No CSS support)** 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
- **没有构建步骤 (No build step)** 限制只能使用 HTML 和 ES5 JavaScript，而不能使用预处理器，如 Pug (formerly Jade) 和 Babel

我么可以使用文件扩展名为 `.vue` 的 **single-file components (单文件组件)** 为以上所有问题提供了解决方法，并且还可以使用 webpack 或 Browserify 等构建工具。

单文件组件组成的结构

- template 组件的模版区域
- script 业务逻辑区
- style 样式

![单文件组件的示例](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201103151134.png)

### webpack中配置vue组件加载器

我们需要先安装几个工具

```
npm i vue-loader vue-template-compiler -D
```

然后我们需要在 `webpack.config.js`配置文件中添加 `vue-loader`

```
const VueLoaderPlugin = require('vue-loader/lib/plugin')
module.exports = {
	module:{
		rules:[
			//规则
			{test:/\.vue$/,loader:'vue-loader'}
		]	
	},
	plugins:[
		// 引入插件
		new VueLoaderPlugin(),
	]
}
```

### 在webpack项目中使用vue

我们需要安装vue

```
npm i vue
```

然后在 src -> index.js 入口文件 中导入

```
import Vue from 'vue'
```

我们可以导入 vue单文件组件

```
import App from './App.vue'
```

我们需要挂载起来

```
const vm = new Vue({
	el:'#app',
	// 渲染组件到el
	render:h=>(App)
	
})
```

## Vue脚手架

Vue CLI 是一个基于 Vue.js 进行快速开发的完整系统

#### 安装

```
npm install -g @vue/cli
```

安装之后，你就可以在命令行中访问 `vue` 命令 `vue --version` 来验证它是否安装成功。

#### 创建一个项目

```
vue create hello-world
```

你会被提示选取一个 preset。

你可以选默认的包含了基本的 Babel + ESLint 设置的 preset，也可以选“手动选择特性”来选取需要的特性。

![image-20201103144554097](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201103144554097.png)

这里我们选择了默认的情况 安装完成后我们可以 进入到目录下 运行

![image-20201103144433736](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201103144433736.png)

### 使用图形化界面

你也可以通过 `vue ui` 命令以图形化界面创建和管理项目：

```
vue ui
```

上述命令会打开一个浏览器窗口，并以图形化界面将你引导至项目创建的流程。

![image-20201103144709838](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201103144709838.png)

#### 创建好的项目目录结构

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201103151037.png)

