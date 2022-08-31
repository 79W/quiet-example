---
title: TypeScript面向对象-继承的使用方法和相关特性
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象-继承的使用方法和相关特性
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 5fa3c3e0
date: 2020-11-06 17:15:05
---

## 继承

在`ts`中也是通过`extends`关键字来实现类的继承

```
class Vip extends User{
	// 此段代码的意思为 Vip类继承了User类
	// 也就说明了 User 是 Vip 的父类
}
```

### super关键字

在子类中，我们可以通过 `super` 来引用父类

- 如果子类没有重写构造函数，则会在默认的 `constructor` 中调用 `super()`
- 如果子类有自己的构造函数，则需要在子类构造函数中显示的调用父类构造函数 : `super(//参数)`，否则会报错
- 在子类构造函数中只有在 `super(//参数)` 之后才能访问 `this`
- 在子类中，可以通过 `super` 来访问父类的成员属性和方法
- 通过 `super` 访问父类的的同时，会自动绑定上下文对象为当前子类 `this`

我们定义一个父类`User`然后用`Vip`类去继承

```
// 父类
class User {
		// 定义属性
    id: number;
    username: string;
    // 构造器
    constructor(id: number, username: string) {
        this.id = id;
        this.username = username
    }
    // 父类方法
    get(): string {
        console.log(this.username)
        return this.username
    }
}
```

我们定义子类`Vip`并且有自己的构造器

```
// 子类
class Vip extends User {
		// 子类的构造器
    constructor(id: number, username: string, score: number) {
    		// 如果子类有自己的构造器就需要 使用 super 进行参数传递
        super(id, username)
    }
}
```

我们实例化子类然后进行调用

```
// 实例化并传递参数
let vip1 = new Vip(1, 'qiao', 12)
// 子类可以直接调用父类的方法
vip1.get()
```

#### 方法的重写

上面例子我们可以知道 子类可以直接调用父类的方法

但是子类也可以对父类的方法进行重写

```
class Vip extends User {
    score: number
    constructor(id: number, username: string, score: number) {
        super(id, username)
        this.score = score
    }
    // 这里覆盖掉了父类的方法
    get(): string {
        console.log(`我是Vip：${this.username}我的积分：${this.score}`)
        return this.username
    }
}
```

#### 方法的重载

参数个数和参数类型的不同进行相应的处理

```
class VIP extends User {
  
    constructor(
  		id: number,
      username: string,
      public score = 0
    ) {
        super(id, username);
    }
    
    // 参数个数，参数类型不同：重载
  	postArticle(title: string, content: string): void;
    postArticle(title: string, content: string, file: string): void;
    postArticle(title: string, content: string, file?: string) {
        super.postArticle(title, content);
        if (file) {
            this.postAttachment(file);
        }
    }
}
// 具体使用场景
let vip1 = new VIP(1, 'Leo');
vip1.postArticle('标题', '内容');
vip1.postArticle('标题', '内容', '1.png');
```

