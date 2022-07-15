---
title: TypeScript面向对象-类与对象的类型
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript面向对象-类与对象的类型
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: 660d2357
date: 2020-11-06 17:47:30
---

## 类与对象类型

当我们在 TypeScript 定义一个类的时候，其实同时定义了两个不同的类型

- 类类型（构造函数类型）
- 对象类型

首先，对象类型好理解，就是我们的 new 出来的实例类型

那类类型是什么，我们知道 JavaScript 中的类，或者说是 TypeScript 中的类其实本质上还是一个函数，当然我们也称为构造函数，那么这个类或者构造函数本身也是有类型的，那么这个类型就是类的类型

```typescript
class Person {
	// 属于类的
  static type = '人';

  // 属于实例的
  name: string;
  age: number;
  gender: string;

  // 类的构造函数也是属于类的
  constructor( name: string, age: number, gender: '男'|'女' = '男' ) {
    this.name = name;
    this.age = age;
    this.gender = gender;
  }
  
  public eat(): void {
    // ...
  }

}

let p1 = new Person('zMouse', 35, '男');
p1.eat();
Person.type;
```

上面例子中，有两个不同的数据

- `Person` 类（构造函数）
- 通过 `Person` 实例化出来的对象 `p1`

对应的也有两种不同的类型

- 实例的类型（`Person`）
- 构造函数的类型（`typeof Person`）

用接口的方式描述如下

```typescript
interface Person {
    name: string;
    age: number;
    gender: string;
    eat(): void;
}

interface PersonConstructor {
  	// new 表示它是一个构造函数
    new (name: string, age: number, gender: '男'|'女'): PersonInstance;
    type: string;
}
```

在使用的时候要格外注意

```typescript
function fn1(arg: Person /*如果希望这里传入的Person 的实例对象*/) {
  	arg.eat();
}
fn1( new Person('', 1, '男') );

function fn2(arg: typeof Person /*如果希望传入的Person构造函数*/) {
  	new arg('', 1, '男');
}
fn2(Person);
```

