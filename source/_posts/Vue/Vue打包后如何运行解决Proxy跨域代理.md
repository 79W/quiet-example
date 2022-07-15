---
title: Vue打包后如何运行解决Proxy跨域代理
categories: 技术笔记
tags:
  - Proxy
  - vue
  - 跨域
excerpt: Vue打包后如何运行解决Proxy跨域代理
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201111195847.png'
articleLink: de96662a
date: 2020-11-11 19:39:33
---

问题：我们在写vu的时候需要跨域请求服务器接口 这时候我们会使用 `Proxy`进行代理访问 但是当我们打包的时候 `devServer` 不会进行打包也就是`Proxy`代理服务器是 `webpack` 跑的

![proxy](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201111200039864.png)

解决：我们需要将在服务端也进行代理访问的配置来解决这个问题

> vue打包后的文件`dist` 是不可以直接双击打开的需要一个服务器来运行

我们在使用vue解决跨域问题时候 我们总是使用`Proxy`代理解决跨域问题

我们创建的`vue.config.js`文件中进行配置

```
module.exports = {
    devServer: {
        // 设置服务器
        host: '127.0.0.1',
        port: 8080,
        // 设置跨域代理
        proxy: {
            '/api': {
                target: 'http://10.205.46.178:3000',// 要跨域的域名
                changeOrigin: true, // 是否开启跨域
                pathRewrite: {
                    '^/api': '' // 在请求的时候 凡是/api开头的地址都可以跨域
                }
            },
        }
    }
};
```

但是当我们进行打包的时候 `devServer`只会在本地跑的时候才会可以使用 （这个webpack 给我们创建的一个服服务器）

我们打包后的文件是不存在这个代理服务器

我们拿`node-koa`来演示 我们需要使用node 在做一个 代理服务器来访问我们需要跨域的接口

我们需要安装一个插件`koa2-proxy-middleware` 这也是主要的需要来进行代理访问

```
npm i koa2-proxy-middleware
```

我们安装好后我们可以参照 他的文档进行配置

```
const proxyOptions = {
    targets: {
        '/api/(.*)': {
            target: 'http://10.205.46.178:3000',
            changeOrigin: true,
            pathRewrite: {
                '^/api': '' // 在请求的时候 凡是/api开头的地址都可以跨域
            }
        },
    }
}
const myProxy = proxy(proxyOptions)
```

然后我们需要注册到`koa`服务器上面

我们来看下完整的代码

```
// 安装koa服务器
const Koa = require('koa')
// 静态资源的访问
const serve = require('koa-static')
//注册 koa 服务
const app = new Koa()
// 代理访问
const proxy = require('koa2-proxy-middleware')
// 代理访问的配置
const proxyOptions = {
    targets: {
    			// 带有 api 的请求全部转发
        '/api/(.*)': {
            target: 'http://10.205.46.178:3000',
            changeOrigin: true,
            pathRewrite: {
                '^/api': '' // 在请求的时候 凡是/api开头的地址都可以跨域
            }
        },
    }
}
// 将配置项导入
const myProxy = proxy(proxyOptions)
// 我们注册到服务器上面
app.use(myProxy)
// 访问资源的解析
const bodyparser = require('koa-bodyparser')
app.use(bodyparser({
    enableTypes: ['json', 'form', 'text']
}))
// 静态资源访问的路经
app.use(serve(__dirname + '/dist'));
// 开启服务器端口
app.listen(8080)
```

然后我们在用node服务进行运行的时候就可以正常访问接口了！



总结：就是我们在vue的配置文件中写`Proxy`的配置只是在 测试时候进行代理，而当我打包后就需要使用服务端在进行配置`Proxy`跨域代理才能进行正常的访问