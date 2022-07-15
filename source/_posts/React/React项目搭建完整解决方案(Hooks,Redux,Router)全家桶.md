---
title: React项目搭建完整解决方案(Hooks,Redux,Router)全家桶
categories: 项目案例
tags:
  - React
  - Redux
  - Hooks
excerpt: React项目完整搭建的一套完整的方案也就是React全家桶来构建React项目
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201117105305.png'
articleLink: 18f8d54e
date: 2020-11-17 08:36:29
---

写一个项目所需要的基本架构组成

我们写项目完全可以按照这个模式去使用自己有拓展的地方在去增加

说明： 本解决方案为`Hooks`加`Redux`加`Router`来构建项目的整体架构

#### 基础包

我们使用脚手架生成React项目后 内置了React的基础包我们还需要按照一些

**自带的包**

```
"react": "^17.0.1",
"react-dom": "^17.0.1",
"react-scripts": "4.0.0",
"web-vitals": "^0.2.4"
```

**需安装的包**

- react-router-dom 路由
- redux 状态管理
- react-redux  React 专用状态管理的库
- redux-thunk 使用 `thunk` 等中间件可以帮助在 Redux 应用中实现异步性

后面我们需要什么再进行安装 基础搭建这些就够了

## 开始

我们使用脚手架 `create-react-app` 生成项目后我们将没有用的东西进行删除

留下来的目录和文件

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
    ├── App.js
    └── index.js
```

我们首先配置 `react-redux` 状态管理也就是数据管理

### react-redux

通过官方文档我们知道react-redux是react官方用来绑定redux的。

我们将Provider放在最上层让redux的store在任何组件里都是可用的。

然后使用connect()函数来链接react的组件和redux的store。

在没有Provider的情况下不能单独使用connect();

我们在 `index.js`文件中进行引入 后进行配置

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
// 数据管理
import { Provider } from 'react-redux'
import store from './store/index' // 这里请关注
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

从上段代码可以看出我们还没创建 `store` 文件 这个文件是干什么用的呢？

`store` ：里面存着数据状态的和 中间件 `redux-thunk` 的配置

我们来创建这个文件夹 在`src`中我们创建一个文件夹名字为`store` 里面在分别创建两个文件夹 分别为`action`,`reducers`

`reducers`: 来存放我们的数据 （我们需要有一个根的文件然后将其他的数据合并为一个state）

`action`: 来进行数据的操作

我们在`store` 文件夹下创建一个 `index.js` 来进行配置

我们最终创建的 `store` 文件夹为

```
store
├── index.js
├── reducers
│   ├── 数据一.js
│   ├── 数据二.js
│   └── index.js // 归总使用
└── action
    ├── 处理逻辑一.js
    └── 处理逻辑二.js
```

`reducers`归总的文件是将 数据一和数据二或者更多 统一成一个 然后导出一个对象给`store`下的`index.js`

`action`里面是对数据进行处理的也就是逻辑调用 数据定义的方法

然后就是我们 `store`下的`index.js`

```
import { createStore, applyMiddleware, combineReducers } from 'redux'
import Thunk from 'redux-thunk'
// 这里导入 reducers
import reducers from './reducers/index'
const store = createStore(
		// 合并
    combineReducers(reducers),
    // 中间件
    applyMiddleware(Thunk)
)
// 这里不做什么东西只是合成一个store
export default store
```

`redux-thunk`中间件在这里进行一个异步的处理 

中间就是在action与reducer之间又加了一层，没有中间件的Redux的过程是：`action -> reducer`，而有了中间件的过程就是`action -> middleware -> reducer`，使用中间件我们可以对action也就是对dispatch方法进行装饰，我们可以用它来实现异步action、打印日志、错误报告等功能

`combineReducers`用于 Reducer 的拆分。

你只要定义各个子 Reducer 函数，(也就是我们上面 `reducers`文件夹里面的东西) 然后用这个方法，将它们合成一个大的 Reducer。

#### reducers文件夹

我们如何进行编写呢？为一个纯函数并且接收两个参数 `state`自定义的数据,`action`传递的参数

```
// lecturers.js
// 这里是直接倒出了
export default function lecturers(state = {
		// 数据
    data: [],
}, action) {
		// 我们 传递的参数会在 action 中
    switch (action.type) {
        case "LOAD_LECTURERS":
            return {
                ...state,
                data: action.data
            };
    }
    return state
}
```

我们 需要在 函数体中进行处理 我们通过传递 `Type`来判断是什么操作进行数据的改变

#### action文件夹

我们可以在这个文件中配置 `axios` 进行一个ajax请求配置

我们创建一个`http`文件

```
import axios from 'axios'
const http = axios.create({
    baseURL: '/api',
    withCredentials: true,
})
// 这里为 http 请求配置
export default http
```

然后我们导入进行 别的文件就可以使用 `ajax`了

我们创建一个文件来对数据的操作 创建一个 `getLecturer.js` 来呼应上我们上面在`reducers`文件夹中创建的 `lecturers.js` 文件

```
// getLecturer.js
// 导入 axios
import http from './http'
// 导出
export default function getLectorer() {
		// 返回一个函数 
    return function (dispatch) {
    		// 第一个这里就跟 axios 文档一样了
        return http.post('/lecturer/lists?page=1&rows=100', {
            order: "desc",
            sort: "sort",
            category_id: 2
        }).then((res) => {
        		// 我们调用的时候需要将 dispatch 传递过来然后进行操作 lecturers.js 的数据
            dispatch({
                type: "LOAD_LECTURERS",
                data: res.data
            })
        })
    }
}
```

我们将数据管理配置完成了下一步我们来配置路由 

### react-router-dom

我们在`App.js`文件中进行引入 路由模块

```
import React from 'react'
import { BrowserRouter, Route, Switch  } from 'react-router-dom'
function App() {
  return (
    <BrowserRouter >
      <Switch>
          <Route path={路经} exact={是否精确匹配} render={回调返回模版} />
       </Switch>
    </BrowserRouter>
  );
}
export default App;
```

#### Route 组件

组件可以需要接收 `path`路经 `component`和 `render`二选一 都是渲染组件使用

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

### 创建组件

我们创建一个 组件 （这里你可以进行分文件夹）看你怎么方便这里只做演示组件内的数据管理怎么操作

```
import React from 'react'
import { connect } from "react-redux"
function Index(props) {
    let { dispatch } = props
    return (
				<div>
					组件内容
				</div>
    )
}
// 重点
export default connect(props => ({ ...props.index }))(Index)
```

我们在组件中导入 `react-redux` 然后我们进行配置 `connect`

我们导出的时候是将我们编写的 函数组件传递给 `connect` 

```
connect(接收的参数 => (返回的参数))(传递函数组件)
```

上面我们为什么 返回了 `props.index` 因为 props 中有很多数据 我们只要需要的数据（还记得我们上面reducers 文件夹吗） 这里返回的是一个新对象

>此解决方案为React 16.8 以后的版本 存在了Hooks 后的解决方案 不知道对不对 很多老项目还都是class 所以class 还是要学的！

