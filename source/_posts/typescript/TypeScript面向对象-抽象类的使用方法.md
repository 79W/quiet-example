---
title: TypeScript面向对象-抽象类的使用方法
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象-抽象类的使用方法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 62a82ff2
date: 2020-11-06 17:39:50
---

## 抽象类

有的时候，一个基类（父类）的一些方法无法确定具体的行为，而是由继承的子类去实现，看下面的例子:

现在前端比较流行组件化设计，比如 `React`

```
class MyComponent extends Component {
  
  constructor(props) {
    super(props);
    this.state = {}
  }
  
  render() {
    //...
  }
  
}
```

根据上面代码，我们可以大致设计如下类结构

- 每个组件都一个 `props` 属性，可以通过构造函数进行初始化，由父级定义
- 每个组件都一个 `state` 属性，由父级定义
- 每个组件都必须有一个 `render` 的方法

```
class Component<T1, T2> {

    public state: T2;

    constructor(
        public props: T1
    ) {
       	// ...
    }
  
  	render(): string {
      	// ...不知道做点啥才好，但是为了避免子类没有 render 方法而导致组件解析错误，父类就用一个默认的 render 去处理可能会出现的错误
    }
}

interface IMyComponentProps {
    title: string;
}
interface IMyComponentState {
    val: number;
}
class MyComponent extends Component<IMyComponentProps, IMyComponentState> {

    constructor(props: IMyComponentProps) {
        super(props);

        this.state = {
            val: 1
        }
    }

    render() {
      	this.props.title;
        this.state.val;
        return `<div>组件</div>`;
    }

}
```

上面的代码虽然从功能上讲没什么太大问题，但是我们可以看到，父类的 `render` 有点尴尬，其实我们更应该从代码层面上去约束子类必须得有 `render` 方法，否则编码就不能通过

### abstract 关键字

如果一个方法没有具体的实现方法，则可以通过 abstract 关键字进行修饰

```
abstract class Component<T1, T2> {

    public state: T2;

    constructor(
        public props: T1
    ) {
    }

    public abstract render(): string;
}
```

使用抽象类有一个好处：

约定了所有继承子类的所必须实现的方法，使类的设计更加的规范

使用注意事项：

- abstract 修饰的方法不能有方法体

- 如果一个类有抽象方法，那么该类也必须为抽象的

- 如果一个类是抽象的，那么就不能使用 new 进行实例化（因为抽象类表名该类有未实现的方法，所以不允许实例化）

- 如果一个子类继承了一个抽象类，那么该子类就必须实现抽象类中的所有抽象方法，否则该类还得声明为抽象的

