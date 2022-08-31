---
title: React基础学习与JSX的使用与了解
categories: 技术笔记
tags:
  - React
  - JSX
excerpt: React基础学习与JSX的使用与了解对DOM的操作使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201021161709.png'
articleLink: 1808249993773321
date: 2020-10-21 08:44:33
---

## React

用于构建用户界面的 JavaScript 库

具有：声明式  组件化 一次学习，随处编写 的特点

**命令式编程 和 声明式编程**

告诉计算机怎么做（How） - 过程

告诉计算机我们要什么（What） - 结果

### 快速使用

**导入**

```
https://unpkg.com/react@16/umd/react.development.js
https://unpkg.com/react-dom@16/umd/react-dom.development.js
```

React.js 提供 React.js 核心功能代码，如：虚拟 dom，组件

```
React.createElement(type,props,children);
```

```
ReactDOM.render(
	// 创建虚拟dom
  React.createElement("h1", { id: "title" }, "hello React"),
  //渲染到页面的指定dom
  document.querySelector("#box"),
);
```

ReactDOM 提供了与浏览器交互的 DOM 功能，如：dom 渲染

```
ReactDOM.render(element, container[, callback])
```

```
//渲染到页面
ReactDOM.render(
	//渲染的内容
  "hello React",
  //渲染到dom元素上面
  document.querySelector("#box"),
  () => {
  //回调函数
  	console.log("渲染完成")
  }
);
```

- element：要渲染的内容
- container：要渲染的内容存放容器
- callback：渲染后的回调函数 

通过上面的形式我们比较麻烦如果多层还需一直的进行嵌套编写

```
let el = React.createElement("header",{id:"title"},
        React.createElement("h1",null,"hello React"),
        React.createElement("p",null,"欢迎来到 React 的世界")
    );
console.log(el);    
ReactDOM.render(
    el,
    document.querySelector("#box")
);
```

需要嵌套多层 然后我们有更好的解决方案

## JSX

JSX 是一个基于 JavaScript + XML 的一个扩展语法

- 它可以作为值使用
- 它并不是字符串
- 它也不是HTML
- 它可以配合JavaScript 表达式一起使用

```
/*
    jsx: js + xml 语法糖
*/     
ReactDOM.render(
    <header id="title">
        <h1>Hello React</h1>
        <p>欢迎来到 React 的世界</p>
    </header>,
    document.querySelector("#box")
);
```

 这里我们需要引入 babel 

### babel

babel-standalone.js：在浏览器中处理 JSX

```
https://cdn.bootcss.com/babel-standalone/6.26.0/babel.min.js
```

### 插值表达式

在 JXS 中可以使用 {表达式} 嵌入表达式

表达式：产生值的一组代码的集合

- 变量 算术运算 函数调用
- 分清楚 表达式 与 语句 的区别，if、for、while 这些都是语句，JSX 不支持语句

各种类型内容在插值中的使用

```
{/*注释*/}
{/*
		多行注释
*/}
```

我们定义一个dom 和一个 input

```
<div id="box"></div>
<input type="text" id="text" />
```

我们在input中输入然后动态的渲染的到页面上

```
let text = document.querySelector("#text");
text.oninput = ()=>{
  ReactDOM.render(
    <div>
    	这是新内容：{text.value}
    </div>,
    document.querySelector("#box")
  );
};
```

#### 输出数据类型

字符串、数字：原样输出

布尔值、空、未定义 会被忽略

可是输出数组但是不能输出对象

```
ReactDOM.render(
        <header id="title">
          {/*
              <div>内容</div>
          */}
          {["1","2",3]}
        </header>,
    		document.querySelector("#box")
);
```

#### 条件渲染

- 三元运算符
- 与或运算符 &&  ||

```
ReactDOM.render(
  <header id="title">
      {isShow ? <h1>正确</h1> : <h1>错误</h1>}
      {isShow && <h1>正确</h1>}
      {isShow || <h1>错误</h1>}
  </header>,
  document.querySelector("#box")
)
```

如果判断条件比较复杂的不是这种单一的判断我们可以进行函数判断条件

首先我们定义一个函数 里面写上相对性的判断

```
let fun = (i) => {
  if (i > 10) {
    return (
        <div>
        <span>1</span>
        </div>
    )
  }
}
```

然后我们使用 插值运算进行调用

```
ReactDOM.render(
  <header id="title">
     {/* 复杂的判断需要 放在函数里面进行 */}
     {fun(19)}
  </header>,
  document.querySelector("#box")
)
```

#### 列表渲染

1. jsx 不能写循环语句 For while
2. jsx 中可以接收数组

首先我们定义个数组

```
let arr = [
    "a",
    "b",
    "c",
    "d"
];
```

然后进行渲染数组

```
ReactDOM.render(
    <div>
        {arr.map(item => <p>这里是p标签数组：{item}</p>)}
    </div>,
    document.querySelector("#box")
)
```

因为不能接受对象那么对象我们怎么进行渲染呢？

首先我们定义一个对象

```
let obj = {
    a: "yi",
    b: "er",
    c: "san"
}
```

```
ReactDOM.render(
    <div>
    		{Object.keys(obj).map((item,index)=><p>第{index}{item}个p</p>)}
    </div>,
    document.querySelector("#box")
);
```

### JSX 使用注意事项

- 必须有,且只有一个顶层的包含元素
- JSX 不是html，很多属性在编写时不一样
  - className
  - style 
- 列表渲染时，必须有 key 值
- 在 jsx 所有标签必须闭合
- 组件的首字母一定大写，标签一定要小写

**XSS**
为了有效的防止 XSS 注入攻击，React DOM 会在渲染的时候把内容（字符串）进行转义，所以字符串形式的标签是不会作为 HTML 标签进行处理的

**总结：**

主要是对Jsx的理解和使用

1. 如果将dom渲染到页面上
2. 如果对数据处理然后在渲染到页面上面