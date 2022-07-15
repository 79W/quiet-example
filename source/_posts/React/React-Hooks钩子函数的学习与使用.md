---
title: React-Hooks钩子函数的学习与使用
categories: 技术笔记
tags:
  - React
  - Hook
  - 钩子函数
excerpt: React-Hooks钩子函数的学习与使用
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimgionic-react-hooks-20201024102.png
articleLink: b7877892d3643324
date: 2020-10-24 08:14:33
---

## 函数式组件

函数组件：本质就是一个函数

接收一个参数，参数是父级传递过来的 props

必须有一个返回值,返回值是该组件要输入的内容

- （16.8-）无状态组件: 函数组件

-  没有状态及生命周期

-  也没有 this

示例

```
import React,{createRef} from 'react';
/*
  函数组件：本质就是一个函数
  接收一个参数，参数是父级传递过来的 props
  必须有一个返回值,返回值是该组件要输入的内容

  （16.8-）无状态组件: 函数组件
      - 没有状态及生命周期
      - 也没有 this
*/
function Child(props){
  // console.log(props);
  const {name} = props;
  let ref = createRef();
  // console.log(this); 函数组件没有this
  return <h1 ref={ref} onClick={()=>{
  }}>Hello {name}</h1>
}
function App() {
  return (
    <div className="App">
        <Child name="小明" />
    </div>
  );
}
export default App;
```

我们用函数组件没有生命周期那些api 你可以使用 hook来实现

## Hook

*Hook* 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

Hook 为已知的 React 概念提供了更直接的 API：props， state，context，refs 以及生命周期

Hook 还提供了一种更强大的方式来组合他们

### useState

通过在函数组件里调用它来给组件添加一些内部 state

React 会在重复渲染时保留这个 state

`useState` 会返回一对值：**当前**状态和一个让你更新它的函数

你可以在事件处理函数中或其他一些地方调用这个函数

它类似 class 组件的 `this.setState`，但是它不会把新的 state 和旧的 state 进行合并

我们在 Class 中是怎么定义变量的？

```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

是不是 在 `state` 里面进行定义

那么我们学习了 `useState` 就可以 用一下的方法进行定义 

`useState` 是一种新方法，它与 class 里面的 `this.state` 提供的功能完全相同。

```
function App() {
    // 定义变量 （默认值）
    let [age, setAge] = useState(12)
    return (
        <div className="App">
            <h1>{age}</h1>
            <button onClick={() => {
                // 改变
                setAge(
                    age += 1
                )
            }}>加一加</button>
        </div >
    );
}
```

`useState` 返回值可以看出来是两个 一个是 当前 state(值) 以及更新 state(改变) 的函数

```
var fruitStateVariable = useState('banana'); // 返回一个有两个元素的数组
var fruit = fruitStateVariable[0]; // 数组里的第一个值
var setFruit = fruitStateVariable[1]; // 数组里的第二个值
```

上面我们可以写成

```
var [fruit,setFruit] = useState('banana'); // 返回一个有两个元素的数组
```

所有我们在 `button` 里面进行调用是可以 改变值的

![React Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaddage00223.gif)

有人会说 我直接定义一个 变量不不就行了怎么这么麻烦

```
function App() {
    // 定义变量
    let age = 10;
    return (
        <div className="App">
            <h1>{age}</h1>
            <button onClick={() => {
                age += 1
            }}>加一加</button>
        </div >
    );
}
```

![React Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaddupras32.gif)



我们发现 根本都没有更新这个数据 

你可以在一个组件中多次使用 State Hook:

```
function ExampleWithManyStates() {
  // 声明多个 state 变量！
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

### useEffect

`useEffect` 就是一个 Effect Hook，给函数组件增加了操作副作用的能力

它跟 class 组件中的 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 具有相同的用途，只不过被合并成了一个 API

在上面的案例里面我们加上它看看效果是怎么样的

```
import React, { useState, useEffect } from 'react';

function App() {
    // 定义变量 （默认值）
    let [age, setAge] = useState(12)

		// 定义		
    useEffect(() => {
        console.log("更新...")
    })

    return (
        < div className="App" >
            <h1>{age}</h1>
            <button onClick={() => {
                // 改变
                setAge(
                    age += 1
                )
            }}>加一加</button>
        </div >
    );
}

export default App;

```

![React Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgupdata003.gif)

我们发现它可以监听到页面的更新

那么我们可以 监听某一个数据的更新吗？

我们在定义一个数据 

```
import React, { useState, useEffect } from 'react';

function App() {
    // 定义变量 （默认值）
    let [age, setAge] = useState(12)
    let [num, setNum] = useState(22)

    useEffect(() => {
        console.log("age更新引起的更新...")
    }, [age])

    return (
        < div className="App" >
            <h1>{age}</h1>
            <button onClick={() => {
                // 改变
                setAge(
                    age += 1
                )
            }}>age加一加</button>
            <h1>{num}</h1>
            <button onClick={() => {
                // 改变
                setNum(
                    num += 1
                )
            }}>num加一加</button>
        </div >
    );
}

export default App;

```

![React Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imghookupdat34.gif)

我们监听了 age 数据更新 然而 num 的更新并没有 触发

然后我们也可以监听一些卸载后的事情

```
useEffect(() => {
    console.log("组件挂载完成之后");
    return () => {
    		console.log("组件即将卸载时执行");
    }
}, []);
```

我们只需要 return 里面进行编写 就是在 组件即将卸载时执行

也就是在页面 卸载的时候我们需要清除掉一些东西 都可以写在这个 `return` 里面

### useRef

如果你将 ref 对象以 `<div ref={myRef} />` 形式传入组件 则无论该节点如何改变

React 都会将 ref 对象的 `.current` 属性设置为相应的 DOM 节点

```
import React, { useRef } from 'react';

function App() {
    const inputEl = useRef(null);
    return (
        <div>
            <input ref={inputEl} type="text" />
            <button onClick={() => {
                // 看看挂在哪里了
                console.log(inputEl)
            }
            }>挂在在哪里了？</button>
        </div >
    )
}

export default App
```

![React Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201024100342625.png)

### useMemo

我们想在组件挂载之前就做一些事情可以使用 `useMemo`

```
const val = useMemo(()=>{
      console.log("组件即将挂载及即将更新");
      return `我在挂载之前`;
});
```

每次渲染都会执行的

## Hook 规则

Hook 本质就是 JavaScript 函数

但是在使用它时需要遵循两条规则

### 只在最顶层使用 Hook

**不要在循环，条件或嵌套函数中调用 Hook**

确保总是在你的 React 函数的最顶层调用他们

遵守这条规则，你就能确保 Hook 在每一次渲染中都按照同样的顺序被调用

这让 React 能够在多次的 `useState` 和 `useEffect` 调用之间保持 hook 状态的正确

### 只在 React 函数中调用 Hook

**不要在普通的 JavaScript 函数中调用 Hook**

- ✅ 在 React 的函数组件中调用 Hook
- ✅ 在自定义 Hook 中调用其他 Hook 



## 自定义 Hook

通过自定义 Hook，可以将组件逻辑提取到可重用的函数中

规定：所有以`use` 开头的都是 Hook

**自定义 Hook 是一个函数，其名称以 “`use`” 开头，函数内部可以调用其他的 Hook**

```
// 自定义hook
function useToggle(init){
    const [off,setOff] = useState(init);
    return [off,()=>{
      setOff(!off);
    }];
}
```

调用自定义的 `useToggle`  Hook

```
const [show,changeShow] = useToggle(true);
```

与 React 组件不同的是 自定义 Hook 不需要具有特殊的标识

我们可以自由的决定它的参数是什么 以及它应该返回什么（如果需要的话）

换句话说 它就像一个正常的函数

但是它的名字应该始终以 `use` 开头 这样可以一眼看出其符合 Hook 的规则

**自定义 Hook 是一种自然遵循 Hook 设计的约定，而并不是 React 的特性。**

