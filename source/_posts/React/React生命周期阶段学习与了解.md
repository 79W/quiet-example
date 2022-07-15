---
title: React生命周期阶段学习与了解
categories: 技术笔记
tags:
  - React
  - 生命周期
excerpt: React生命周期阶段学习与了解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgog-learning-path-react.jpg'
articleLink: 053be72ca94c2222
date: 2020-10-22 11:03:22
---

## 生命周期

所谓的生命周期就是指某个事物从开始到结束的各个阶段

当然在 React.js 中指的是组件从创建到销毁的过程

React.js 在这个过程中的不同阶段调用的函数

通过这些函数，我们可以更加精确的对组件进行控制

前面我们一直在使用的 render 函数其实就是组件生命周期渲染阶段执行的函数

### 演变

每个版本可能对生命周期的api进行挑战 总共可以分为三部分 之前 现在 未来

> #### 之前（React 16.3 之前）

#### 挂载阶段 

`constructor`

类的构造函数，也是组件初始化函数，一般情况下，我们会在这个阶段做一些初始化的工作

- 初始化 state
- 处理事件绑定函数的 this

```
constructor(props){
  super(props);
  console.log(1,"初始化");
}
```

`componentWillMount`

```
componentWillMount(){
	console.log(2,"组件即将被渲染到DOM");
}
```

`render`

render 方法是 Class 组件必须实现的方法

```
render(){
	console.log(3,"向DOM中渲染");
}
```

`componentDidMount`

在组件挂载后（render 的内容插入 DOM 树中）调用。通常在这个阶段，我们可以：

- 操作 DOM 节点
- 发送请求

```
componentDidMount(){
	console.log(4,"组件被渲染到DOM了");
}
```

#### 更新阶段

##### 父组件更新引起组件更新

`componentWillReceiveProps(nextProps)`

```
componentWillReceiveProps(nextProps){
	console.log(0,"父组件更新引起子组件更新");
}
```

`shouldComponentUpdate(nextProps, nextState)`

发生在更新阶段，getDerivedStateFromProps 之后，render 之前，该函数会返回一个布尔值，决定了后续是否执行 render，首次渲染不会调用该函数

```
// nextProps 新的props nextState 新的state
shouldComponentUpdate(nextProps, nextState){
  console.log(1,"组件更新",nextProps, nextState,this.props,this.state);
  return true;// true 接着向下执行生命周期,并更新视图，false 打断执行,不在更新视图
}
```

`componentWillUpdate(nextProps, nextState)`

shouldComponentUpdate返回true以后，组件进入重新渲染的流程，进入componentWillUpdate,这里同样可以拿到nextProps和nextState。

```
componentWillUpdate(nextProps, nextState){
		console.log(2,"组件即将更新");
}
```

`render`

```
render(){
	console.log(3,"正在更新");
}
```

`componentDidUpdate(prevProps, prevState)`

该函数会在 DOM 更新后立即调用，首次渲染不会调用该方法。我们可以在这个函数中对渲染后的 DOM 进行操作

```
componentDidUpdate(prevProps, prevState){
 	console.log(4,"组件更新完成",prevProps, prevState,this.props,this.state);
 }
```

##### 组件自身更新

`shouldComponentUpdate`

`componentWillUpdate`

`render`

`componentDidUpdate`

#### 卸载阶段

`componentWillUnmount`

```
componentWillUnmount(){
	console.log("组件即将被卸载");
}
```

卸载阶段案例：

```
import React,{Component} from 'react';
class Child extends Component {
    componentDidMount(){
        window.onresize=()=>{
            let box = document.querySelector("#box");
            let newChild = document.createElement("div");
            newChild.innerHTML = window.innerWidth;
            box.appendChild(newChild);
        };
    }
    componentWillUnmount(){
        console.log("组件即将被卸载");
        window.onresize = null;
    }
    render(){
      return <div id="box"></div>;
    }
}
export default Child;
```

![image-20201022113111460](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201022113111460.png)

> #### 现在

#### 挂载阶段

`constructor`

初始化

`static getDerivedStateFromProps(props, state) `

这个是新方法api 请注意 this问题  因为是静态方法

```
// 利用getDerivedStateFromProps替代了componentWillMount 和componentWillReceiveProps
static getDerivedStateFromProps(props,state){
  console.log(props,state,"组件即将挂载");
  return false;
}
```

`render`

必须要有的

`componentDidMount`

```
componentDidMount(){
	console.log(4,"组件被渲染到DOM了");
}
```



#### 更新阶段

##### 父组件更新引起组件更新

`static getDerivedStateFromProps(props, state)`

`shouldComponentUpdate()`

`componentWillUpdate()`

`render()`

`getSnapshotBeforeUpdate()`

组件更新之后，即将重新渲染DOM

```
 getSnapshotBeforeUpdate(){
 	console.log(组件更新之后);
 }
```

`componentDidUpdate()`

##### 组件自身更新

`shouldComponentUpdate()`

`componentWillUpdate()`

`render()`

`getSnapshotBeforeUpdate() `

`componentDidUpdate()`

#### 卸载阶段

`componentWillUnmount`

#### 错误处理

`static getDerivedStateFromError() `

`componentDidCatch(error, info)  `

错误捕获只能捕获到生命周期当中的错误

![image-20201022113043530](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201022113043530.png)

> #### 未来

`useEffect() `

可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合

它在第一次渲染之后和每次更新之后都会执行

你可能会更容易接受 effect 发生在“渲染之后”这种概念

不用再去考虑“挂载”还是“更新”

React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。

将 document 的 title 设置为点击次数

```
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>次数：{count}</p>
      <button onClick={() => setCount(count + 1)}>
        点击
      </button>
    </div>
  );
}
```

与 `componentDidMount` 或 `componentDidUpdate` 不同，使用 `useEffect` 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。大多数情况下，effect 不需要同步地执行。

如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它。

参考：[React生命周期](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

