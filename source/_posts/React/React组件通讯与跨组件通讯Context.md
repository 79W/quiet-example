---
title: React组件通讯与跨组件通讯Context
categories: 技术笔记
tags:
  - React
  - 组件通讯
  - Context
excerpt: React组件通讯与跨组件通讯Context
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201022140406.png'
articleLink: 23319a1e40f71222
date: 2020-10-22 09:32:12
---

## 组件通讯

在 React.js 中，数据是从上自下流动（传递）的

也就是一个父组件可以把它的 state / props 通过 props 传递给它的子组件

但是子组件不能修改 props - React.js 是单向数据流

如果子组件需要修改父组件状态（数据）是通过回调函数方式来完成的

#### 父级向子级通信

我有一个数据给自己的子级那么就可以通过 props 进行传递

我们定义一个父级

```
// 父级
class App extends Component {
    state = {
    		//定义数据
        data: "这是数据进行传递的数据"
    }
    render() {
        return (
            <div>
                <h1>这是父级</h1>
                //调用子级 并传递数据w
                <P data={this.state.data} />
            </div>
        )
    }
}
```

我们在子级中进行接收

```
// 子级
class P extends Component {
    render() {
        //接收父级传递过来的数据
        console.log(this.props);
        return (
            <p>我是子级</p>
        )
    }
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201022100621.png)

父传子就是把数据添加子组件的属性中，然后子组件中从props属性中，获取父级传递过来的数据。

#### 子级向父级通信

在react 中是不可以逆向传递的是从上向下进行传递那么我们子如何向父进行传递呢？

我们可以通过一个回调函数

创建父级

```
// 父级
class App extends Component {
    state = {
        mydata: "这是数据进行传递的数据"
    }
    // 定义一个修改数据的方法
    setmyData(mydata) {
        this.setState({
            mydata
        })
    }
    render() {
        return (
            <div>
                <h1>这是父级</h1>
                {/* 将数据和 修改数据的方法进行传递 */}
                <P mydata={this.state.mydata} setmyData={this.setmyData} />
            </div>
        )
    }
}
```

这里重要的是定义了 `setmyData` 方法 和在下面向子级进行传递了这个方法

子级接收这个方法

```
// 子级
class P extends Component {
    render() {
        //接收父级传递过来的数据
        console.log(this.props)
        return (
            <p>我是子级</p >
        )
    }
}
```

我们打印看一看

![image-20201022102812863](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201022102812863.png)

我们可以看到数据和方法全部传递了过来

那么我们去调用就可以了呀

子级进行调用

```
// 子级
class P extends Component {
    render() {
        //接收父级传递过来的数据
        let { mydata, setmyData } = this.props
        return (
            <p>我是子级
            		// 定义个按钮
                <button onClick={
                		// 点击事件进行执行函数
                    () => {
                        setmyData("子级改了数据")
                    }
                }>修改数据</button >
            </p >

        )
    }
}
```

这里你会发现报错

`Cannot read property 'setState' of undefined`

`无法读取未定义的属性“setState”`

![image-20201022103126023](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201022103126023.png)

这里的问题是 `this` 的问题

我们可以在父级里面的  `setmyData`  打印下 `this`  看看

![image-20201022103527515](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201022103527515.png)

解决办法换成 尖头函数

```
setmyData = (mydata) => {
    this.setState({
    		mydata
    })
}
```

![updatafujixiu](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgupdatafujixiu.gif)

解决了子向父传递数据的问题

### 跨组件通信 - context

我们说了子向父亲传递数据可以放在属性中 就可以

那么我们父向孙级别的传递数据怎么办呢？

很多有会说` 父->子->孙`  这样是可以实现但是 孙子的孙子呢？而且中间层级都不使用这个数据

我们这个时候就可以使用  `context`

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法

#### 创建一个

```
const MyContext = React.createContext(defaultValue);
```

`defaultValue` 可以理解为默认值

#### 开始传递

```
<MyContext.Provider value={/* 某个值 */}>
```

Provider 接收一个 `value` 属性传递给消费组件

当 Provider 的 `value` 值发生变化时，它内部的所有消费组件都会重新渲染

#### 接收数据

`Class.contextType`

```
孙.contextType = MyContext;
//获取值
let value = this.context;
```

以下也是可以的

```
class MyClass extends React.Component {
  static contextType = MyContext;
  render() {
    let value = this.context;
    /* 基于这个值进行渲染工作 */
  }
}
```

#### 演示：

```
import React, { Component } from 'react';
// 创建
const MyContext = React.createContext();
// 接收 与 发送
const { Provider, Consumer } = MyContext;
// 子级
class P extends Component {
    render() {
        return (
            <p>子<br />
                {/* 引入孙 */}
                <Sp />
            </p >
        )
    }
}
// 孙级
class Sp extends Component {
    static contextType = MyContext;
    render() {
        // 接收到了数据
        let value = this.context;
        // 打印数据
        console.log(value)
        return (
            <span> 我是孙</span >
        )
    }
}
// 父级
class App extends Component {
    state = {
        mydata: "这是数据进行传递的数据"
    }
    render() {
        return (
            <div>
                {/* 传递数据了 */}
                <Provider value={this.state.mydata}>
                    <h1>这是父级</h1>
                    {/* 将数据和 修改数据的方法进行传递 */}
                    <P />
                </Provider>
            </div>
        )
    }
}
export default App;
```

**注意在使用不熟练时，最好不要再项目中使用 context，context一般给第三方库使用**

