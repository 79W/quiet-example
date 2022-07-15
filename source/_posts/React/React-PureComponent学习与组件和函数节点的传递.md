---
title: React-PureComponent学习与组件和函数节点的传递
categories: 技术笔记
tags:
  - React
  - 组件传递
excerpt: React-PureComponent学习与组件和函数节点的传递
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201023152017.png'
articleLink: 74ba6be09c5a3323
date: 2020-10-23 12:55:33
---

## PureComponent

还记得我们创建类组件时候必须要继承什么吗？

```
//创建个类组件
class App extends Component {
  render() {
    return (
      <h1>类组件需要继承 Component </h1>
    )
  }
}
```

那么我们的 `PureComponent` 是什么呢？

`React.PureComponent` 与 `React.Component`很相似。

两者的区别在于 `React.Component`]并未实现 `shouldComponentUpdate()`

而 `React.PureComponent` 中以浅层对比 prop 和 state 的方式来实现了该函数

**用法**

```
//创建 类组件
class App extends PureComponent {
  render() {
    return (
      <h1>类组件继承 PureComponent </h1>
    )
  }
}
```

这样看来区别不大哈！！！！

我们写一段代码 

```
class App extends Component {
  state = {
    data: "qiaoblog"
  }
  render() {
    console.log('更新了。。。。')
    return (
      <div>
        <h1>类组件继承 Component </h1>
        <button onClick={() => {
          let { data } = this.state;
          this.setState({
            data
          })
        }}> 改值</button >
      </div >

    )
  }
}
```

我们这段代码是 继承 `Component` 然后进行修改 `state` 里面的 `data` 

但是我们修改的值没有发生改变 

但每次 更新 都会打印下 

![wzz](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgConmmmeup01.gif)

给我们的感觉就是 我们点击 改值的时候 是重新刷新了一下

那么我们在用 `PureComponent` 来试一试

```
class App extends PureComponent {
  state = {
    data: "qiaoblog"
  }
  render() {
    console.log('更新了。。。。')
    return (
      <div>
        <h1>类组件继承 PureComponent </h1>
        <button onClick={() => {
          let { data } = this.state;
          this.setState({
            data
          })
        }}> 改值</button >
      </div >

    )
  }
}
```

对比 `Component` 是同一个功能代码

![pure02](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgpure02.gif)

但是给我们的效果是不一样的 我们不管点击了多少次都没 打印出来第二个 更新了的字样 

也就是说 没有进行第二次刷新 

那么在某些情况下使用 `React.PureComponent` 可提高性能。

但是有一种情况就是  

如果使用PureComponent ，state 的中的谋个值是引用类型，一定记得在更新时，更新引用地址(返回一个新对象)

如果你返回的是以下的方式是不会更新页面的 

```

class App extends PureComponent {
  state = {
    data: {
      name: "qiaoblog"
    }
  }
  render() {
    console.log('更新了。。。。')
    return (
      <div>
        <h1>类组件继承 PureComponent </h1>
        <button onClick={() => {
             let { data } = this.state;
             data.name = "qiaoblog2";
             this.setState({
            	 data: data
          	 })
        }}> 改值</button >
      </div >

    )
  }
}
```

正确方式

```
class App extends PureComponent {
  state = {
    data: {
      name: "qiaoblog"
    }
  }
  render() {
    console.log('更新了。。。。')
    return (
      <div>
        <h1>类组件继承 PureComponent </h1>
        <button onClick={() => {
          let { data } = this.state;
          data.name = "qiaoblog2";
          this.setState({
          	//这里返回一个新对象
            data: { ...data }
          })
        }}> 改值</button >
      </div >

    )
  }
}
```

如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。

仅在你的 props 和 state 较为简单时，才使用 `React.PureComponent`

## dangerouslySetInnerHTML

我们有时候需要返回一串html

```
let inner = `<h1>这是html的h1标签</h1>`
```

而在页面并不是我们想要的 h1标签 而是将字符串直接输出了

![image-20201023133532815](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023133532815.png)

这时候我们就需要使用 `dangerouslySetInnerHTML` 进行转译

```
let inner = `<h1>这是html的h1标签</h1>`
class App extends PureComponent {
    render() {
        return (
            <div
                dangerouslySetInnerHTML={{
                    __html: inner
                }}
            ></div >
        )
    }
}
```

这里是 写在标签里面属于标签的属性 

![image-20201023134230094](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023134230094.png)

## children

组件标签对之间的内容会被当做一个特殊的属性 props.children 传入组件内容

比如我们父组件向子组件进行传递一些 标签的时候可以这样

```
<Mytest>
    <p>这是文本第一行</p>
    <p>这是文本第二行</p>
    <p>这是文本第三行</p>
</Mytest>
```

`Mytest` 是子组件 传递了三个`p`标签

我们在子组件中进行接收

```
import React, { PureComponent } from "react";
class Mytest extends PureComponent {
    render() {
    	  // 这里打印 父传递过来的
        console.log(this.props)
        return (
            <div>

            </div>
        )
    }
}
export default Mytest
```

![image-20201023140623089](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023140623089.png)

这里接收到了三个p标签并且存在了 `children` 字段里面 

可以自定义结构的组件的常用形式

- children
- 传递函数
- 传递子组件

### 传递组件

父级

```
class App extends PureComponent {
    render() {
        return <div>
            <Mytest />
        </div>
    }
}
```

父级里面有个子组件 Btn

```
class Btn extends PureComponent {
    render() {
        return <button>关闭</button>
    }
}
```

我们用父级把btn这个组件传递给 Mytest 组件

可以这样弄

```
// 自定义一个名字 然后将 btn 组件插进去 首字母要大写
<Mytest Myclass={Btn} />
```

然后我们 就可以在 `Mytest` 子组件中 打印出来 

![image-20201023141839060](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023141839060.png)

这时候我们 可以直接进行调用

```
import React, { PureComponent } from "react";
class Mytest extends PureComponent {
    render() {
        let { Myclass } = this.props
        return (
            <div>
                <Myclass />
            </div>
        )
    }
}
export default Mytest
```

![image-20201023142310903](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023142310903.png)

然后我们还可以传递函数

### 函数传递

我们将 btn 组件传递过来后我们还可以给他传递函数

`Mytest`组件定义一个函数方法

```
 mycon = () => {
        console.log("我是被传递过来的函数")
    }
```

进行传递

```
 <Myclass mycon={this.mycon} />
```

然后 `Btn` 进行接收 

```
class Btn extends PureComponent {
    render() {
        console.log(this.props)
        // 接收
        let { mycon } = this.props
        return <button
            onClick={() => {
                //调用
                mycon()
            }}
        >关闭</button>
    }
}
```

然后我们可以看出来 有一个 mycon 的方法 

然后我们点击一下 按钮会发现可以打印成功 说明函数传递成功

![image-20201023142844170](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023142844170.png)



## Refs

React 支持一种非常特殊的属性 Ref ，你可以用来绑定到 render() 输出的任何组件上。

绑定一个 ref 属性到 render 的返回值上：

```
<input ref="myInput" />
```

在其它代码中，通过 this.refs 获取支撑实例:

```
var input = this.refs.myInput;
var inputValue = input.value;
```

![image-20201023150507606](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023150507606.png)

但是这个版本可能要废弃了

```
Warning: A string ref, "myInput", has been found within a strict mode tree. String refs are a source of potential bugs and should be avoided. We recommend using useRef() or createRef() instead. 
```

![image-20201023151118738](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201023151118738.png)

**新版 createRef**

我们需要先调用 `createRef` 

```
import React, { createRef } from 'react';
const myRef = createRef()
```

在将自己定义的名字放在 `ref` 中

```
<input type="text" ref={myRef} />
```

然后我们可以调用这个

```
console.log(myRef)
```

完整代码

```
import React, { createRef } from 'react';
const myRef = createRef()
class MyComponent extends React.Component {
    render() {
        return (
            <div>
                <input type="text" ref={myRef} />
                <input
                    type="button"
                    value="点我输入框获取焦点"
                    onClick={() => {
                        myRef.current.focus();
                    }}
                />
            </div>
        );
    }
}
export default MyComponent
```

