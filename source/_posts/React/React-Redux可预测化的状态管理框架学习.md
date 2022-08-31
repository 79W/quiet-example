---
title: React-Redux可预测化的状态管理框架学习
categories: 技术笔记
tags:
  - React
  - Redux
  - 状态管理
excerpt: React-Redux可预测化的状态管理框架学习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201025175021.png'
articleLink: 7ed3645c2be52625
date: 2020-10-25 16:16:26
---

## Redux

Redux 是一个独立的 JavaScript 状态管理库 与非 React 内容之一

也就是我们不引入 react 也是可以使用的

### createStore

```
import { createStore } from "redux"
```

然后我们得到一个 `createStore`

```
let store = createStore(reducer, [preloadedState], enhancer)
```

创建一个 Redux store来以存放应用中所有的 state。

而 Store 对象 是为了对 state，Reducer，action 进行统一管理和维护

`reducer`接收一个纯函数并且接受两个参数分别是当前的 state 和要处理的 action返回新的 state 树

```
function reducer(state = {
	// 参数
  nub: 1,
  name: 'qiaoblog'
}, action) {
	// 要怎么处理
  console.log(action)
  switch (action.type) {
    case "DEIT":
      return {
        ...state,
        nub: state.nub + 1
      }
  }
  // 一定返回哦
  return state
}
```

这里有几个概念 什么是 `state`  `action` 

#### state 对象

通常我们会把应用中的数据存储到一个对象树（Object Tree） 中进行统一管理，我们把这个对象树称为： `state` 

**state 是只读的**

但是我们需要怎么去修改 state 中的数据呢？

可以通过 纯函数进行修改  问题又来了什么是纯函数呢？

- 相同的输入永远返回相同的输出
- 不修改函数的输入值
- 不依赖外部环境状态
- 无任何副作用

使用纯函数的好处 ：便于测试 , 有利重构 .

#### action 对象

我们对 state 的修改是通过 reducer 纯函数来进行的，同时通过传入的 action 来执行具体的操作，action 是一个对象

- type 属性 : 表示要进行操作的动作类型，增删改查……
- payload属性 : 操作 state 的同时传入的数据

但是这里需要注意的是，我们不直接去调用 Reducer 函数，而是通过 Store 对象提供的 dispatch 方法来调用

```
dispatch({
    type: 'ADD_TODO',
    name: 'qiaoblog'
  })
```

我们传递了两个参数一个是 Type 和 name 我们的type 值普遍使用全大写 

然后我们纯函数的第二个参数 `action` 可以接收到这时候我们就可以根据 type 来进行一些相关操作

我们还有定义其他的数据 比如上面的 name 参数也是一样可以接收到的

### Store 方法

#### getState()

返回应用当前的 state 树。 它与 store 的最后一个 reducer 返回值相同。

也就是获取值 `state`

```
store.getState()
```

#### dispatch(action)

分发 action。

这是触发 state 变化的惟一途径。

会使用当前 `getState()` 的结果和传入的 `action` 以同步方式的调用 store 的 reduce 函数。

返回值会被作为下一个 state。

从现在开始，这就成为了 getState()`的返回值，同时变化监听器(change listener)会被触发。

```
import { createStore } from 'redux'
// 创建 store 对象
let store = createStore(todos, [ 'Use Redux' ])
//定一个函数
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
// 传递一个对象 因为上面的函数返回是一个对象 
store.dispatch(addTodo('Read the docs'))
```

#### subscribe(listener)

添加一个变化监听器。每当 dispatch action 的时候就会执行，state 树中的一部分可能已经变化。

你可以在回调函数里调用 `getState()`来拿到当前 state。

## react-redux

Redux官方提供的 React 绑定库。 具有高效且灵活的特性。

也就是 将redux与react结合起来

```
npm i react-redux
```

### Provider

`<Provider store>` 使组件层级中的 `connect()` 方法都能够获得 Redux store。

正常情况下，你的根组件应该嵌套在 `<Provider>` 中才能使用 `connect()` 方法。

如果你**真的**不想把根组件嵌套在 `<Provider>` 中，你可以把 `store` 作为 props 传递到每一个被 `connect()` 包装的组件，但是我们只推荐您在单元测试中对 `store` 进行伪造 (stub) 或者在非完全基于 React 的代码中才这样做。

正常情况下，你应该使用 `<Provider>`。

- `store` (Redux Store): 应用程序中唯一的 Redux store 对象
- `children` (*ReactElement*) 组件层级的根组件。

```
ReactDOM.render(
  <Provider store={store}>
    <MyRootComponent />
  </Provider>,
  document.getElementById('root')
)
```

### Connect

连接 React 组件与 Redux store

连接操作不会改变原来的组件类

反而**返回**一个新的已与 Redux store 连接的组件类

```
connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
```

`connect()` 接收四个参数，它们分别是 `mapStateToProps`，`mapDispatchToProps`，`mergeProps`和`options`。

```
 connect((store.state,props))=>{
    // 允许 对 props 和 store 进行一个合并处理
    return {
        //传递给组件的数据
    };
})(Inner);
```

```
connect(function(store中的state,父组件传入的props){
        return 要传递给组件的信息
 })(要包装的组件);
```

connect 在组件中获取 store 中的 state 和 dispatch 方法

创建一个组件 `Inner`

```
function Inner(props) {
    // console.log(props);
    let { nub, dispatch } = props;
    return (
        <div>
            <p>nub:  <strong>{nub}</strong></p>
            <button
                onClick={() => {
                    dispatch({
                        type: "ADD"
                    })
                }}
            >加1</button>
        </div>
    );
}
```

导入`connect`包

```
import { connect } from "react-redux";
```

将 `Inner` 组件传进去然后会返回一个新的组件 

```
connect((state, props) => {
    //console.log(state,props);
    return {
        ...props,
        nub: state.nub
    };
})(Inner);
```

传入一个组件 返回一个组件

## redux 原则

- 单一数据源:

  整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中

- State 是只读的

  唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象

- 使用纯函数来执行修改