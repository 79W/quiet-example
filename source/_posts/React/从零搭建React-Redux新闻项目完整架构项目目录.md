---
title: 从零搭建React-Redux新闻项目完整架构项目目录
categories: 项目案例
tags:
  - React
  - Redux
  - 资讯项目
  - 新闻项目
excerpt: 从零搭建React-Redux新闻项目完整架构项目目录
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgunnamed.png'
articleLink: 095fc4521e252027
date: 2020-10-27 14:20:20
---

### create-react-app

初次构建项目 `create-react-app` 会包含一些东西

安装 `create-react-app` 需要先初始化 `npm init -y`

```
$ npm i create-react-app
```

安装好后我们构建初级的 React 项目

```
$ npx create-react-app 名称
```

#### 目录结构 

```
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
```

#### 自带的包

```
"react": "^17.0.1",
"react-dom": "^17.0.1",
"react-scripts": "4.0.0",
"web-vitals": "^0.2.4"
```

我们主要安装需要的东西 

首先我们配置 `react-redux` 状态管理

### react-redux

首先我们安装上 相关的包

```
"redux": "^4.0.5",
"react-redux": "^7.2.2",
"redux-thunk": "^2.3.0",
```

```
$ npm i redux react-redux redux-thunk
```

我们这里做一个Reducer拆分的处理

我们在 src 中创建一个文件夹 名为 ： `store`  这里放我们 Reducer 数据相关

里面有个 `index.js` 构建我们的 `store` 对象 并且把中间件弄好

```
// 创建 createStore对象  合并 数据Reducer applyMiddleware传入中间件
import { createStore, combineReducers, applyMiddleware } from "redux";
// 中间件
import thunk from "redux-thunk";
// 数据拆分
import list from "./reducer/list";
//  合并
const Sorte = createStore(combineReducers({
    list
}), applyMiddleware(thunk));
// 导出
export default Sorte
```

上面会有一个数据的拆分

我们在当前目录下创建一个`reducer` 文件夹 里面存放数据的相关处理

如上面的 list

```
// 定义函数方法
function list(list = {
		// 定义数据
    loading: true,
    data: []
}, action) {
    switch (action.type) {
    		// 对数据的相关操作
        case "LIST_ADD":
            return {
                loading: true,
                data: []
            }
        // Expected a default case default-case
        default:
        // do nothing
    }
    return list;
}

// 导出
export default list;
```

![目录结构](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201027143250481.png)

我们在 根目录下的 `index.js` 中进行引入的操作

```
// 导入 store
import store from './store/index'
// 引入 状态管理数据配置
import { Provider } from "react-redux";
```

然后我们用 `Provider` 包裹住根然后进行一个 store的传递

```
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

这样我们就完成了数据的拆分和中间件等的配置

### react-router-dom

对路由进行一个管理  

使用React构建的单页面应用，要想实现页面间的跳转，首先想到的就是使用路由

```
$ npm i react-router-dom
```

我们在 根目录的 `index.js` 中进行一个引入 然后我们包住 app

这里有一个先后顺序

```
import { BrowserRouter } from 'react-router-dom'
```

这里数据放在最外层 确保哪里都可以 拿到数据

```
ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
);
```

我们在 ` src`中创建一个  `routes` 文件我们来存放 路由相关内容

我们在创建一个 `component` 文件存放一些公共的组件 比如头部和底部等

### 组件编写

在`component`  里面创建一个头部组件 `Header` 



我们在组件当中会用到 样式的组件 这时候我们 就可以使用 Ant Design

### Ant Design

`antd` 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

我们进行安装

```
$ npm i antd
```

然后我们配置 按需加载css

此时我们需要对 create-react-app 的默认配置进行自定义，这里我们使用 craco（一个对 create-react-app 进行自定义配置的社区解决方案）。

```
$ npm i @craco/craco
```

修改 `package.json` 里的 `scripts` 属性。

```
/* package.json */
"scripts": {
-   "start": "react-scripts start",
-   "build": "react-scripts build",
-   "test": "react-scripts test",
+   "start": "craco start",
+   "build": "craco build",
+   "test": "craco test",
}
```

```
"scripts": {
    "start": "craco start",
    "build": "craco build",
    "test": "craco test",
    "eject": "react-scripts eject"
  },
```

然后在项目根目录创建一个 `craco.config.js` 用于修改默认配置。

```
/* craco.config.js */
module.exports = {
  // ...
};
```

我们在安装 `npm i babel-plugin-import` 

```
$ npm i babel-plugin-import
```

配置文件`craco.config.js`

```
module.exports = {
  babel: {
    plugins: [
      ['import', { libraryName: 'antd', style: 'css' }],
    ]
  },
};
```

如果要结合less配置，可参照antd官网下面给出完整配置

然后安装 `craco-less` 并修改 `craco.config.js` 文件如下。

```
$ npm i craco-less
```

将  `craco.config.js`  文件 修改成以下内容

```
const CracoLessPlugin = require('craco-less');
module.exports = {
    plugins: [
        {
            plugin: CracoLessPlugin,
            options: {
                lessLoaderOptions: {
                    lessOptions: {
                        modifyVars: { "@primary-color": "#1DA57A" },
                        javascriptEnabled: true,
                    },
                },
            },
        },
    ],
    babel: {
        plugins: [
            ['import', { libraryName: 'antd', style: true }],
        ]
    },
};
```

这里利用了 less-loader的 `modifyVars` 来进行主题配置

变量和其他配置方式可以参考 [配置主题](https://ant.design/docs/react/customize-theme-cn) 文档

修改后重启 `yarn start` 

这时候你就可以 使用 antd 的组件了

### Axios

我们需要发送请求 到服务端然后 获取到数据

安装

```
$ npm i axios
```

我们在 `store` 文件中进行引入调用 我们创建一个 `http.js` 文件 然后我们在创建个文件 进行调用

### 项目

看看我写成什么🐶样了 还不是很熟悉

![React-redux-项目](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgFB2515D32C2C41D82D78C5CC04647833.jpg)



![React-redux-项目](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img77.png)





[资源下载](https://wwa.lanzous.com/ilgDkhsx3cd)

