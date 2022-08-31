---
title: TypeScript面向对象-类与接口的使用方法
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象-类与接口的使用方法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 77e3806c
date: 2020-11-06 17:42:55
---

## 类与接口

在前面我们已经学习了接口的使用，通过接口，我们可以为对象定义一种结构和契约。

我们还可以把接口与类进行结合，通过接口，让类去强制符合某种契约，从某个方面来说，当一个抽象类中只有抽象的时候，它就与接口没有太大区别了，这个时候，我们更推荐通过接口的方式来定义契约

- 抽象类编译后还是会产生实体代码，而接口不会
- `TypeScript` 只支持单继承，即一个子类只能有一个父类，但是一个类可以实现过个接口
- 接口不能有实现，抽象类可以

### implements

在一个类中使用接口并不是使用 `extends` 关键字，而是 `implements`

- 与接口类似，如果一个类 `implements` 了一个接口，那么就必须实现该接口中定义的契约
- 多个接口使用 `,` 分隔
- `implements` 与 `extends` 可同时存在

```typescript
interface ILog {
  getInfo(): string;
}

class MyComponent extends Component<IMyComponentProps, IMyComponentState> implements ILog {

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
  
  	getInfo() {
      	return `组件：MyComponent，props：${this.props}，state：${this.state}`;
    }

}
```

实现多个接口

```typescript
interface ILog {
    getInfo(): string;
}
interface IStorage {
    save(data: string): void;
}

class MyComponent extends Component<IMyComponentProps, IMyComponentState> implements ILog, IStorage {

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
  
  	getInfo(): string {
      	return `组件：MyComponent，props：${this.props}，state：${this.state}`;
    }
  
  	save(data: string) {
      	// ... 存储
    }

}
```

#### 继承接口

和类一样，接口也可以相互继承。

这让我们能够从一个接口里复制成员到另一个接口里，可以更灵活地将接口分割到可重用的模块里。

```typescript
interface ILog {
    getInfo(): string;
}
interface IStorage extends ILog {
    save(data: string): void;
}
```

一个接口可以继承多个接口，创建出多个接口的合成接口。