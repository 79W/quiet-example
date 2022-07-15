---
title: 'React-Redux-reducer状态拆分,数据拆分'
categories: 技术笔记
tags:
  - React
  - Redux
  - reducer
  - 状态拆分
excerpt: 'React-Redux-reducer状态拆分,数据拆分'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgv2-0966c2510a8b485705b5d560287fd48f_1440w.jpg'
articleLink: 7f04f5ebfccc2426
date: 2020-10-26 10:20:24
---

### reducer 拆分

是一个纯函数 里面存着我们项目的全部数据 

```
function reducer(state={
        list: {
            tab: "all",
            page: 1,
            data: []
        },
        article: {
            detail: {},
            message:[]
        }
    },action){
        switch(action.type){

    }
     return state;
}
```

这样我们能有 list列表数据 然后我们还有article文章的数据 可能还有 评论数据 等等各种的数据

这样我们的 reducer 函数就会显得特别的乱后期维护也不是特别好 

我们可以按数据块进行拆分吗？

比如 list 是一块 article又是单独的一块 这样就可以单独处理了

那么好我们来实现 一下

list 的数据

```
// 负责处理 list 的数据
function list(list={ 
    tab: "all",
    page: 1,
    data: []
},action){
    switch(action.type){
        case "ADD":
            let {data} = list;
            data.push("a");
            return {
                ...list,
                data
            }
    }
    return list;
}
```

article 的数据

```
// 负责处理 article 的数据
function article(article={ 
    detail: {},
    message:[]
},action){
    switch(action.type){
        case "ADD":
            let {message} = article;
            message.push("b");
            return {
                ...article,
                message
            }
    }
    return article;
}
```

我们需要进行合并然后统一去传递给`store`

```
function reducer(state={},action){
    return {
        list:list(state.list,action),
        article: article(state.article,action)
    }
}
```

这样就可以了 每块都是独立的 可以进行单独的处理了

问题又来了当我们的数据特多的时候我们还是这样进行一个 赋值 会比较麻烦

我们有没有更好的 方法呢？

`combineReducers` 可以很好的解决这个问题

#### combineReducers

```
import { combineReducers } from "redux";
```

我们只需要传递给他一个对象

```
const reducer = combineReducers({
    list,
    article
});
```

里面包含着我们的 list方法和article方法当然你如果还有更多也可以写进去

这样我们看着是不是就很简洁了

### 坑

我们上面的代码中有一个坑 

我们演示一下上面的代码

当我们调用 `ADD` 的时候 

```
dispatch({
		type: "ADD"
})
```

我们初始的数据是 

![image-20201026111039834](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026111039834.png)

我们调用完

![image-20201026111107735](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026111107735.png)

我们发现两个数据的 `ADD` 都执行了 

说明了 他不知道你要执行那个 `ADD` 

而我们就需要进行一个 名字的区分如 `LIST_ADD` 和 `ARTICLE_ADD` 这时候我们在调用的时候就知道是想调用那个了



