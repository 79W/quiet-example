---
title: TypeScript面向对象-类的基本使用方法和一些特性
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象类的基本使用方法和一些特性
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 551d1a62
date: 2020-11-06 17:00:05
---

## 类

面向对象编程中一个重要的核心就是：`类`

当我们使用面向对象的方式进行编程的时候 通常会首先去分析具体要实现的功能 把特性相似的抽象成一个一个的类 然后通过这些类实例化出来的具体对象来完成具体业务需求。

 `TypeScript` 与 `EMCAScript2015+` 在类方面共有的一些特性

- `class` 关键字
- 构造函数：`constructor`
- 成员属性定义
- 成员方法
- this关键字

在 `TypeScript` 中还有许多 `ECMAScript` 没有的

#### 创建类

通过 `class` 就可以描述和组织一个类的结构，语法：

```
// 通常类的名称我们会使用 大坨峰命名 规则，也就是 （单词）首字母大写
class User {
  // 类的特征都定义在 {} 内部
}
```

#### 构造函数

我们可以通过 `new` 关键字来调用该类从而得到该类型的一个具体对象

为什么类可以像函数一样去调用呢，其实我们执行的并不是这个类

而是类中包含的一个特殊函数：构造函数 - `constructor`

```
// 创建一个类
class User {
	// 构造函数
	constructor() {
    console.log('实例化...')
  } 
}
// 实例化一个类
let user1 = new User;
```

- 默认情况下，构造函数是一个空函数

- 构造函数会在类被实例化的时候调用
- 我们定义的构造函数会覆盖默认构造函数
- 如果在实例化（new）一个类的时候无需传入参数，则可以省略 `()`
- 构造函数 `constructor` 不允许有`return` 和返回值类型标注的（因为要返回实例对象）

通常情况下，我们会把一个类实例化的时候的初始化相关代码写在构造函数中，比如对类成员属性的初始化赋值

#### 成员属性与方法定义

我们如何在类里面进行定义属性和方法呢？

我们声明一个类

```
class User {
	// 在构造器中接收传递的参数
  constructor(id: number, username: string) {
  }
}
let user1 = new User(1, 'qiaoblog');
```

我们定义 属性来接收构造器传递过来的参数

```
class User {
		// 定义变量 设置标注
    id: number;
    username: string;
    // 构造函数接收参数
    constructor(id: number, username: string) {
    		// 将接收到的参数进行赋值
        this.id = id;
        this.username = username
    }
}
// 实例化并传递参数
let user1 = new User(1, 'qiaoblog')
```

我们可以在类中定义方法吗？

```
class User {
    id: number;
    username: string;
    constructor(id: number, username: string) {
        this.id = id;
        this.username = username
    }
    // 定义方法
    get(): string {
    		// 打印已有属性
    		// 在类的内部可以通过 `this` 来访问成员属性和方法
        console.log(this.username)
        // 我因为设置了返回值为 string
        return this.username
    }
}
let user1 = new User(1, 'qiaoblog')
// 调用方法
user1.get() // 打印 ‘qiaoblog’
```

上面一段代码中 我们使用了this的关键词 可以看出来 我们可以通过 `this`来访问类的成员属性和方法

### 修饰符

有的时候，我们希望对类成员（属性、方法）进行一定的访问控制，来保证数据的安全

通过 `类修饰符` 可以做到这一点，目前 TypeScript 提供了四种修饰符：

- public：公有，默认
- protected：受保护
- private：私有
- readonly：只读

#### public 修饰符

这个是类成员的默认修饰符，它的访问级别为：

- 自身
- 子类
- 类外

#### protected 修饰符

它的访问级别为：

- 自身
- 子类

#### private 修饰符

它的访问级别为：

- 自身

#### readonly 修饰符

只读修饰符只能针对成员属性使用，且必须在声明时或构造函数里被初始化，它的访问级别为：

- 自身
- 子类
- 类外

```
class User {
  constructor(
  	// 可以访问，但是一旦确定不能修改
  	readonly id: number,
    // 可以访问，但是不能外部修改
    protected username: string,
    // 外部包括子类不能访问，也不可修改
    private password: string
  ) {
    // ...
  }
	// ...
}

let user1 = new User(1, 'zMouse', '123456');
```

