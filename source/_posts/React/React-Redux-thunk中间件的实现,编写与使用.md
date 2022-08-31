---
title: 'React-Redux-thunk中间件的实现,编写与使用'
categories: 技术笔记
tags:
  - React
  - Redux
  - thunk
  - 中间件
excerpt: 'React-Redux-thunk中间件的实现,编写与使用'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgmaxresdefault.png'
articleLink: 6e5b9d8e54f82426
date: 2020-10-26 13:54:24
---

## redux 中间件

简单的说就是我在两端的中间做点事情 

那两端呢 第一端就是我在获取数据 第二端是我修改数据 在这两个中间进行一些活动

在真正去修改 reducer 之前，做一些处理，比如我们的副作用

原理就是改写 `dispatch`

首先我们引入进来

```
import React,{useEffect} from 'react';
import {useSelector, useDispatch, useStore} from "react-redux";
```

```
//获取 dispatch
let dispatch = useDispatch();
//我们在存一份
let prevDispatch = dispatch;
```

我们对 `dispatch` 进行改写操作

```
dispatch = (action)=>{
      // 第一种情况直接去修改 reducer
      if((typeof action) == "object"){
          prevDispatch(action);
      } else {
          // 第二种情况，经过一些副作用之后，再去修改 reducer
          action(dispatch);
      }
  }
```

第一种情况这是传入的一个对象然后我们直接进行修改操作

```
dispatch({
		type:"ADD"
});
```

第二种情况就是 传入的是一个函数  我们先执行函数然后在 通知更新修改

```
dispatch((dispatch)=>{
    setTimeout(()=>{
   		 dispatch({type:"ADD"});
    },2000);
});
```

核心的思路就是对原有 `dispatch` 做一个改写 

## redux- thunk

使用 `thunk` 等中间件可以帮助在 Redux 应用中实现异步性

可以将 `thunk` 看做 store 的 `dispatch()` 方法的封装器

没有 `thunk` 的话，默认地是同步派遣

```
npm i redux-thunk
```

在能够编写 `thunk` action creator 之前，确保我们的 store 已准备好接收中间件：

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from '../reducers/root_reducer';
// 这里的第二个参数将 中间件传入进去
const store = () => createStore(rootReducer, applyMiddleware(thunk));

export default store;
```

现在一切设置完毕， `thunk` 中间件已经能被应用到该 store ：`thunk` 中间件导入自 `redux-thunk`，并且 `thunk` 的实例被传递给 Redux 的 `applyMiddleware()`函数。

### 用法

这里用法其实咱们上面也就进行编写过了

第一种用法就是传递一个对象 可以直接进行修改数据

```
dispatch({
		type:"ADD"
});
```

第二种就是传递一些函数 这个函数会接收两个参数 `dispatch` 和 `getState`

```
dispatch((dispatch,getState)=>{
		//发送ajax请求 then回调后再去执行 dispatch
    Axios.get(``).then((res)=>{
        dispatch({
            type: "ADD",
            data: res.data.data
        });
    });
});
```

**总：如果应用需要与服务器交互，则应用 `thunk` 等中间件可以解决异步数据流问题**

