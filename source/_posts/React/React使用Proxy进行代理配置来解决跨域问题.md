---
title: React使用Proxy进行代理配置来解决跨域问题
categories: 技术笔记
tags:
  - React
  - Proxy
excerpt: React使用Proxy进行代理配置来解决跨域问题
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg/img20201117110318.png'
articleLink: d1514e5f
date: 2020-11-17 10:41:39
---

我们之前配置 `proxy` 是直接在package.json中设置proxy属性

```
"proxy": {
    "/api/**": {
      "target": "地址",
      "changeOrigin": true
    }
  }
```

而现在我们这样去配置会 报错

```
When specified, "proxy" in package.json must be a string.
Instead, the type of "proxy" was "object".
Either remove "proxy" from package.json, or make it a string.
error Command failed with exit code 1.
```

**这是因为在新版本的react项目中，在package.json中的proxy只能设置成字符串，如下**

```
"proxy": "地址"
```

我们可以通过 中间件的方式进行配置proxy

```
npm install http-proxy-middleware --save
```

安装middleware插件后，在src目录中新建setupProxy.js文件，在文件中放入如下代码：

```
const { createProxyMiddleware } = require("http-proxy-middleware")
// 配置 代理请求
// 版本有改动
module.exports = function (app) {
    app.use(createProxyMiddleware('/api', {
        'target': '地址',
        'secure': true,
        'changeOrigin': true,
        'pathRewrite': {
            '^/api': ''
        },
        // 设置域名cookie
        'cookieDomainRewrite': '地址'
    }))
};
```

打包后我们需要再次配置一次代理 也就是说这个代理只是我们 开发期间使用的代理模式（可以参考我以前文章有个VUE打包代理配置是一样的）

