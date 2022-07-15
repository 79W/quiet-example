---
title: JavaScritp面向对象之包装对象和对象类型判断
categories: 技术笔记
tags:
  - JavaScript
  - 面向对象
  - 类型判断
excerpt: JavaScritp面向对象之包装对象和对象类型判断
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922205953.png'
articleLink: 6ab3e4acbb124020
date: 2020-09-20 19:02:40
---

## 包装对象

把我们基本类型 string number boolean 自动判别自动包装一层对象 （临时对象）

```
//没有 string对象 然而可以使用 string的对象内的方法
let str = "a b c"
str = str.split(' ')
```

### 自己进行包装对象

```
function mysplit(str,method,arg){
	let temp = new String(str)
	return temp[method](arg)
	//销毁包装对象
}
```

```
let arr = mysplit('a b c','split',' ')
```

## 类型判断

### hasOwnProperty()

方法会返回一个布尔值，指示对象自身属性中是否具有指定的属性（也就是，是否有指定的键）。

方法判断属性是否存在

```
o = new Object();
o.hasOwnProperty('prop'); // 返回 false
o.prop = 'exists';
o.hasOwnProperty('prop'); // 返回 true
delete o.prop;
o.hasOwnProperty('prop'); // 返回 false
```

### constructor

**`constructor `**是一种用于创建和初始化`class`创建的对象的特殊方法。

如果不指定一个构造函数(constructor)方法, 则使用一个默认的构造函数(constructor)。

```
class Polygon {
  constructor() {
    this.name = 'Polygon';
  }
}

const poly1 = new Polygon();

console.log(poly1.name);
// expected output: "Polygon"

```

### instanceof()

**`instanceof`** **运算符**用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

```
let arr = []
console.log(arr instanceof Array)//==>true
console.log(arr instanceof Object)//==>true
```

### 精准判断类型

```
let arr = []
let res = Object.prototype.toString.call(arr)
console.log(res)//=>[Object Array]
```