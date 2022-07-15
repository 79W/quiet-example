---
title: WebPack使用教程和基本Api的理解
categories: 技术笔记
tags:
  - WebPack
excerpt: WebPack使用教程和基本Api的理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201031163048.png'
articleLink: 67c272220a7b3631
date: 2020-10-31 10:06:36
---

## WebPack

本质上，`webpack` 是一个现代 `JavaScript` 应用程序的静态模块打包器(module bundler)。

当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

### 安装

```
npm install -D webpack webpack-cli
```

> 注：不推荐全局安装

`webpack-cli` : 提供 webpack 命令、工具，类似 `create-react-app`

`webpack` : webpack 代码，类似 `react`

### 使用

我们可以直接使用 `npx`

```
// 查看版本
npx webpack -v
```

打包

```
npx webpack ./index.js
```

`./index.js` 为入口文件 里面引入了其他js文件

上面命令会使用 `webpack` 默认的一些配置对模块文件进行打包，并把打包后的文件输出到默认创建的 `./dist` 目录下，打包后的文件名称默认为 `main.js`。

模块文件打包以后，就可以在不支持 es6 模块语法的浏览器环境下引入使用了。

**打包文件分析**

- 把分散的模块文件打包到一个文件中，不需要外部引入了
- 内置了一个小型模块加载器(类似 `requireJS`)，实现了打包后的代码隔离与引用

以上就是 webpack 最基础的使用于基本原理，当然强大的 `webpack` 远远不止这些功能。

### 打包配置

虽然，我们可以直接通过命令的来打包，但是推荐创建一个 `webpack.config.js` 的配置文件来实现更方便和强大的功能。

`webpack` 命令在运行的时候，默认会读取运行命令所在的目录下的 `webpack.config.js` 文件，通常我们会在项目的根目录下运行命令和创建配置文件。

```
module.exports = {
  ...	//配置项
}
```

![image-20201031101818681](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201031101818681.png)



### 核心配置

#### mode

参数（模式）： `"production" | "development" | "none"`

分别对应的是 ：生产模式,开发模式,没有模式

```
module.exports = {
  mode: 'production'
};
```

结果：是对生成的文件格式的显示 生产模式文件大小会更小一些 开发模式会显示的内容更多

#### entry

指定打包⼊口⽂文件，有三种不同的形式：`string | object | array`

`string` ：一对一：一个入口、一个打包文件

```
module.exports = {
  entry: './src/index.js'
}
```

我们设置好这个入口文件后我们直接运行

```
npx webpack
```

即可自动打包 不需要输入后面的路径了

`object` ：多对多：多个入口、多打包文件

```
module.exports = {
  entry: {
    'index1': "./src/index1.js",
    'index2': "./src/index2.js"
  }
}
```

`array`  ：多对一：多个入口、一个打包文件

```
module.exports = {
  entry: [
    './src/index1.js',
    './src/index2.js',
  ]
}
```

### output

打包后的文件位置

```
module.exports = {
  output: {
  	path: path.resolve(__dirname, "dist"),
    filename: "bundle.js",
		filename: "[name].js"
	}
}
```

- 可以指定一个固定的文件名称，如果是多入口多出口(`entry` 为对象)，则不能使用单文件出口，需要使用下面的方式
- 通过 `webpack` 内置的变量占位符：`[name]`

### 深入了解

在 `webpack` 中，有一个很重要的特性：模块不仅仅只是 `js` 的文件，`webpack` 可以把任意文件数据作为模块进行处理，包括：非 js 文本、css、图片等等

```
import txt from './a.txt';
console.log(txt);
```

但是 `webpack` 默认情况下只能处理 `js` 模块，如果需要处理其它类型的模块，则需要使用它提供的一些其它功能

![image-20201031104844279](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201031104844279.png)

- `loaders`：`webpack` 中灰常核心的内容之一，前面我们说的非 js 类型的模块处理就靠它了，不同类型的模块的解析就是依赖不同的 `loader` 来实现的
- `plugins`：`webpack` 中另外一个核心的内容，它主要是扩展 `webpack` 本身的一些功能，它们会运行在各种模块解析完成以后的打包编译阶段，比如对解析后的模块文件进行压缩等

### Loaders

```
module.exports = {
  ...,
  module: {
  	rules:[
  		{ 
  				test: /\.xxx$/, 
  				use: 'xxx-loader' 
  		},
  	]
	}
}
```

在 `rules` 列表里面每写一个对象代表一个处理规则

```
{ 
  test: /\.css$/, 
  use: 'css-loader' 
},
```

入这个规则就是用正则表达式进行匹配到 `css` 文件然后用 `css-load` 文件进行处理

####  raw-loader

```
npm install --save-dev raw-loader
```

允许将文件作为字符串导入。

```
module.exports = {
  ...,
  module: {
      rules: [
      {
        test: /\.(txt|md)$/,
        use: 'raw-loader'
    	}
    ]
	}
}
```

#### file-loader

把识别出的资源模块，移动到指定的输出⽬目录，并且返回这个资源在输出目录的地址(字符串)

```
npm install --save-dev file-loader
```

```
rules: [
  ...,
	{
		test: /\.(png|jpe?g|gif)$/,
    use: {
      loader: "file-loader",
      options: {
        // placeholder 占位符 [name] 源资源模块的名称
        // [ext] 源资源模块的后缀
        name: "[name]_[hash].[ext]",
        //打包后的存放位置
        outputPath: "./images"
        // 打包后文件的 url
        publicPath: './images',
      }
    }
	}
]
```

##### 占位符

`[ext]` ：目标文件/资源的文件扩展名。

`[name]`：文件/资源的基本名称。

`[path]` ：资源相对于webpack / config的路径`context`。

`[folder]`：资源的文件夹在。

`[query]` ：资源查询，即`?foo=bar`。

`[emoji]`：随机表情符号表示形式`content`。

`[hash]`：指定用于对文件内容进行哈希处理的哈希方法。

### url-loader

可以处理理 `file-loader` 所有的事情，但是遇到图片格式的模块，可以选择性的把图片转成 `base64`  格式的字符串，并打包到 `js` 中，对⼩体积的图片⽐较合适，⼤图⽚不合适。

```bash
npm install --save-dev url-loader
```

```javascript
rules: [
  ...,
	{
		test: /\.(png|jpe?g|gif)$/,
    use: {
      loader: "url-loader",
      options: {
        // placeholder 占位符 [name] 源资源模块的名称
        // [ext] 源资源模块的后缀
        name: "[name]_[hash].[ext]",
        //打包后的存放位置
        outputPath: "./images"
        // 打包后文件的 url
        publicPath: './images',
        // 小于 100 字节转成 base64 格式
        limit: 100
      }
    }
	}
]
```

#### css-loader

分析 `css` 模块之间的关系，并合成⼀个 `css`

```
npm install --save-dev css-loader
```

```
rules: [
  ...,
	{
		test: /\.css$/,
    use: {
      loader: "css-loader",
      options: {
  			// 启用/禁用 url() 处理
  			url: true,
  			// 启用/禁用 @import 处理
  			import: true,
        // 启用/禁用 Sourcemap
        sourceMap: false
      }
    }
	}
]
```

#### style-loader

把 `css-loader` 生成的内容，用 `style` 标签挂载到⻚面的 `head` 中

```bash
npm install --save-dev style-loader
```

```js
rules: [
  ...,
	{
		test: /\.css$/,
    use: ["style-loader", "css-loader"]
	}
]
```

同一个任务的 `loader` 可以同时挂载多个，处理顺序为：从右到左，也就是先通过 `css-loader` 处理，然后把处理后的 `css` 字符串交给 `style-loader` 进行处理

```js
rules: [
  ...,
	{
		test: /\.css$/,
    use: [
  		{
  			loader: 'style-loader',
  			options: {}
  		},
      'css-loader'
		]
	}
]
```

还有很多很多的 [loader](https://webpack.js.org/loaders/raw-loader/) 可以去官方看看写的也很清楚

### Plugins

扩展 `webpack` 本身的一些功能，它们会运行在各种模块解析完成以后的打包编译阶段，比如对解析后的模块文件进行压缩等

#### HtmlWebpackPlugin

在打包结束后，⾃动生成⼀个 `html` ⽂文件，并把打包生成的 js 模块引⼊到该 `html` 中

```
npm install --save-dev html-webpack-plugin
```

```
// webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
	...
  plugins: [
     new HtmlWebpackPlugin({
       title: "My App",
       filename: "app.html",
       template: "./src/html/index.html"
     }) 
  ]
};
```

```
<!--./src/html/index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title><%=htmlWebpackPlugin.options.title%></title>
</head>
<body>
    <h1>html-webpack-plugin</h1>
</body>
</html>
```

在 `html` 模板中，可以通过 `<%=htmlWebpackPlugin.options.XXX%>` 的方式获取配置的值

**更多的配置**

- `title`: ⽤来生成⻚面的 `title` 元素
- `filename`: 输出的 `HTML` ⽂件名，默认是 `index.html`， 也可以直接配置子目录
- `template`: 模板⽂件路径，⽀持加载器（`loader`），⽐如 `html!./index.html`
- `inject`: `true | 'head' | 'body' | false`，注⼊所有的资源到特定的 `template` 或者 `templateContent` 中，如果设置为 `true` 或者 `body`，所有的 `javascript` 资源将被放置到 `body` 元素的底部，`'head'` 将放置到 `head` 元素中
- `favicon`: 添加特定的 `favicon` 路径到输出的 `HTML` 文件中
- `minify`: `{} | false`， 传递 `html-minifier` 选项给 `minify` 输出
- `hash`: `true | false`，如果为 `true`，将添加 `webpack` 编译生成的 `hash` 到所有包含的脚本和 `CSS` ⽂件，对于解除 `cache` 很有用
- `cache`: `true | false`，如果为 `true`，这是默认值，仅在文件修改之后才会发布文件
- `showErrors`: `true | false`，如果为 `true`，这是默认值，错误信息会写入到 `HTML` ⻚面中
- `chunks`: 允许只添加某些块 (⽐如，仅 unit test 块)
- `chunksSortMode`: 允许控制块在添加到⻚面之前的排序方式，⽀持的值:`'none' | 'default' |{function}-default:'auto'`
- `excludeChunks`: 允许跳过某些块，(⽐如，跳过单元测试的块)

#### clean-webpack-plugin

删除（清理）构建目录

```bash
npm install --save-dev clean-webpack-plugin
```

```js
const {CleanWebpackPlugin} = require('clean-webpack-plugin');
module.exports = {
	...
  plugins: [
    ...,
    new CleanWebpackPlugin(),
    ...
  ]
}
```

#### mini-css-extract-plugin

提取 `CSS` 到一个单独的文件中

```bash
npm install --save-dev mini-css-extract-plugin
```

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
module.exports = {
	...,
  module: {
  	rules: [
  		{
  			test: /\.s[ac]ss$/,
  			use: [
  				{
  					loader: MiniCssExtractPlugin.loader
					},
          'css-loader',
          'sass-loader'
        ]
			}
  	]
	},
  plugins: [
    ...,
    new MiniCssExtractPlugin({
    	filename: '[name].css'
    }),
    ...
  ]
}
```

#### sourceMap

我们实际运行在浏览器的代码是通过 `webpack` 打包合并甚至是压缩混淆过的代码，所生成的代码并不利于我们的调试和错误定位，我们可以通过 `sourceMap` 来解决这个问题，`sourceMap` 本质是一个记录了编译后代码与源代码的映射关系的文件，我们可以通过 `webpack` 的 `devtool` 选项来开启 `sourceMap`

```
module.exports = {
  mode: 'production',
  devtool: 'source-map',
  ...
}
```

首先，编译后会为每一个编译文件生成一个对应的 `.map` 文件，同时在编译文件中添加一段对应的 `map` 文件引入代码

## WebpackDevServer

每次的代码修改都需要重新编译打包，刷新浏览器，特别麻烦，我们可以通过安装 `webpackDevServer` 来改善这方面的体验

```bash
npm install --save-dev webpack-dev-server
```

启动命令：

```bash
npx webpack-dev-server
```

或者，`package.json` 中添加 `scripts`

```js
...,
"scripts": {
  "server": "webpack-dev-server"
}
```

修改 `webpack.config.js`

```js
module.exports = {
  ...,
  devServer: {
  	// 生成的虚拟目录路径
  	contentBase: "./dist",
  	// 自动开启浏览器
  	open: true,
  	// 端口
  	port: 8081
	}
}
```

启动服务以后，`webpack` 不在会把打包后的文件生成到硬盘真实目录中了，而是直接存在了内存中(同时虚拟了一个存放目录路径)，后期更新编译打包和访问速度大大提升

## Hot Module Replacement

在之前当代码有变化，我们使用的 `live reload`，也就是刷新整个页面，虽然这样为我们省掉了很多手动刷新页面的麻烦，但是这样即使只是修改了很小的内容，也会刷新整个页面，无法保持页面操作状态。`HMR` 随之就出现了，它的核心的局部（模块）更新，也就是不刷新页面，只更新变化的部分

```js
module.exports = {
  ...,
  devServer: {
  	// 生成的虚拟目录路径
  	contentBase: "./dist",
  	// 自动开启浏览器
  	open: true,
  	// 端口
  	port: 8081,
  	// 开启热更新
  	hot:true,
  	// 即使 HMR 不生效，也不去刷新整个页面(选择开启)
    hotOnly:true,
  	proxy: {
      '/api': {
      	target: 'http://localhost:8787'
    	}
    }
	}
}
```

开启 `HMR` 以后，当代码发生变化，`webpack` 即会进行编译，并通过 `websocket` 通知客户端(浏览器)，我们需要监听处理来自 `webpack` 的通知，然后通过 `HMR` 提供的  `API` 来完成我们的局部更新逻辑

<!--./fn1.js-->

```js
export default function() {
    console.log('start1!');
}
```

<!--index.js-->

```js
import fn1 from './fn1.js';
box1.onclick = fn1;

if (module.hot) {//如果开启 HMR
    module.hot.accept('./fn1.js', function() {
      // 更新逻辑
      box1.onclick = fn1;
    })
}
```

上面代码就是 当 ./fn1.js 模块代码发生变化的时候，把最新的 fn1 函数绑定到 box1.onclick 上

从上面就可以看到，`HMR` 其实就是以模块为单位，当模块代码发生修改的时候，通知客户端进行对应的更新，而客户端则根据具体的模块来更新我们的页面逻辑(这些逻辑需要自己去实现)，好在当前一些常用的更新逻辑都有了现成的插件

**css热更新**

样式热更新比较简单，`style-loader` 中就已经集成实现了，我们只需要在 `use` 中使用就可以了

## Proxy

当下前端的开发都是前后端分离开发的，前端开发过程中代码会运行在一个服务器环境下(如当前的 `WebpackDevServer`)，那么在处理一些后端请求的时候通常会出现跨域的问题。`WebpackDevServer` 内置了一个代理服务，通过内置代理就可以把我们的跨域请求转发目标服务器上(`WebpackDevServer` 内置的代理发送的请求属于后端 - `node`，不受同源策略限制)，具体如下：

<!--后端代码，以 node 为例-->

```js
const Koa = require('koa');
const KoaRouter = require('koa-router');

const app = new Koa();
const router = new KoaRouter();

router.get('/api/info', async ctx => {
    ctx.body = {
        username: 'zMouse',
        gender: 'male'
    }
})

app.use( router.routes() );
app.listen(8787);
```

<!--前端代码-->

```js
axios({
  url: 'http://localhost:8787/api/info'
}).then(res => {
  console.log('res',res.data);
})
```

默认情况下，该代码运行以后会出现跨域请求错误，修改 `webpack` 配置

```js
module.exports = {
  ...,
  devServer: {
  	// 生成的虚拟目录路径
  	contentBase: "./dist",
  	// 自动开启浏览器
  	open: true,
  	// 端口
  	port: 8081,
  	proxy: {
      '/api': {
      	target: 'http://localhost:8787'
    	}
    }
	}
}
```

通过 `proxy` 设置，当我们在当前 `WebpackDevServer` 环境下发送以 `/api` 开头的请求都会被转发到 http://localhost:8787 目标服务器下

<!--修改前端代码-->

```js
axios({
  //url: 'http://locahost:8081/api/info',
  url: '/api/info'
}).then(res => {
  console.log('res',res.data);
})
```

注意 `url` 地址要填写 `WebpackDevServer` 域，比如当前 `WebpackDevServer` 开启的是 http://localhost:8081，也就是我们当前前端代码运行的环境，那么请求的 `url` 也必须发送到这里，当我们的请求满足了 `proxy` 中设置的 `/api` 开头，那么就会把请求转发到 `target` ，所以最后的实际请求是：http://lcoahost:8787/api/info

