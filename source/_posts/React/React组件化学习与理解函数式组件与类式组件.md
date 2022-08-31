---
title: React组件化学习与理解函数式组件与类式组件
categories: 技术笔记
tags:
  - React
excerpt: React组件化学习与理解函数式组件与类式组件
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img8d61885czujian.png'
articleLink: 0483188fad8f1221
date: 2020-10-21 14:24:12
---

## React组件化

对具有一定独立功能的数据与方法的封装对外暴露接口

有利于代码功能的复用且不用担心冲突问题

有两种组件的方法

- 函数式组件的方法
  - 函数的名称就是组件的名称
  - 函数的返回值就是组件要渲染的内容

- 类式组件的方法
  - 组件类必须继承 `React.Component`

  - 组件类必须有 `render` 方法

### 函数式组件

1. 组件名称必须以大写字母开头
2. 组件的返回值只能有一个根元素

```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

我们打印一下 `props` 是什么

```
function Welcome(props) {
  console.log(props)
  return <h1>Hello, {props.name}</h1>;
}
```

我们首先渲染到页面上

```
ReactDOM.render(
  <Welcome name="QiaoBlog" />,
  document.getElementById('root')
);
```

这里我们在控制台上面就可以看到 打印的结果

![image-20201021144810587](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201021144810587.png)

我们将两部分 分开也就是 我渲染在一个文件里面 我函数在另外一个文件

我们创建个文件 `fun01.js` 然后在里面写上

```
import React from 'react';

function Welcome(props) {
    console.log(props)
    return <h1>Hello, {props.name}</h1>;
}
//导入函数组件
export default Welcome;
```

然后我们在 `app.js` 页面进行导入

```
import React from 'react';
import ReactDOM from 'react-dom';
//导入编写的函数组件
import Welcome from './zujianfun'

ReactDOM.render(
  <Welcome name="QiaoBlog" />,
  document.getElementById('root')
);
```

### 类式组件

也就是ES6形式的class

但是这里需要继承父类 `React.Component`

在 `React.Component` 的子类中有个必须定义的 `render()`函数。

```
class ClassFun extends React.Component {
    constructor(props) {
        super(props)
        console.log(props)
    }
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

#### State

我们可以自己定义变量并且进行修改

```
class ClassFun extends React.Component {
    constructor(props) {
        super(props)	
        this.state = {name: "qiaoBlog"};
    }
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

#### this.setState()

用来时刻更新组件 state

**不要直接修改 State**

此代码不会重新渲染组件：

```
// Wrong
this.state.comment = 'Hello';
```

而是应该使用 `setState()`:

```
// Correct
this.setState({comment: 'Hello'});
```

构造函数是唯一可以给 `this.state` 赋值的地方

**State 的更新可能是异步的**

出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用。

因为 `this.props` 和 `this.state` 可能会异步更新，所以你不要依赖他们的值来更新下一个状态

此代码可能会无法更新

```
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

#### 小案例

**组件** `classfun.js`

```
import React from 'react';

class ClassFun extends React.Component {
    constructor(props) {
        super(props)
        console.log(props)
    }
    state = {
        name: 'qiao',
        age: 19
    }
    render() {
        let { name, age } = this.state;
        return (<div>
            <h1>我叫：{name}</h1><br />
            <h1>我{age}岁</h1>
            <button
                onClick={(e) => {
                    this.setState(function () {
                        return {
                            age: ++age
                        }
                    });
                }}
            >长一岁</button>
        </div >);
    }
}

export default ClassFun;
```

`app.js` 主入口

```
import React from 'react';
import ReactDOM from 'react-dom';
import ClassFun from './classfun'

ReactDOM.render(
	// 这里调用
  <ClassFun />,
  document.getElementById('root')
);
```

效果

![anlit-21-202015-57-29](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imganlit-21-202015-57-29.gif)

这个案例使用了 三点

1. state 定义变量
2. setState 更新变量
3. onClick 点击事件

### props 和 state

- props 父组件传递过来的参数
- state 组件自身状态
  - setState 更新
  - 多个 setState 合并

#### 区别

**state**

主要作用是用于组件保存、控制、修改*自己*的可变状态

在组件内部进行初始化

也可以在组件内部进行修改

但是组件外部不能修改组件的 state

**props**

主要作用是让使用该组件的父组件可以传入参数来配置该组件

它是外部传进来的配置参数

组件内部无法控制也无法修改

**总：**

state 和 props 都可以决定组件的外观和显示状态

通常props 做为不变数据或者初始化数据传递给组件

可变状态使用 state



