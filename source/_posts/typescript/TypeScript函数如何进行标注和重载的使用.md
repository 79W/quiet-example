---
title: TypeScript函数如何进行标注和重载的使用
categories: 技术笔记
tags:
  - TypeScript
excerpt: TypeScript函数如何进行标注和重载的使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201106130554.png'
articleLink: f37bc979
date: 2020-11-06 12:43:44
---

## 函数详解

- 函数类型标注
  - 函数声明、函数表达式的标注
  - 函数参数、默认参数、剩余参数、返回值
- 普通函数与箭头函数中的 <u>this</u>
- 函数重载的使用

### 函数的标注

一个函数的标注包含传递的参数和返回值的类型

我们可以直接在函数直接指定

```
function fn(a: string): string {};
```

我们还可以使用接口还有`type`关键字进行定义

```
type callback = (a: string)=> string;
// 接口定义
interface ICallBack {
  (a: string): string;
}
```

调用：

```
let fn: callback = function(a) {};
let fn: ICallBack = function(a) {};
```

### 可选参数

通过参数名后面添加 `?` 来标注该参数是可选的

```
let div = document.querySelector('div');
function css(el: HTMLElement, attr: string, val?: any) {

}
// 设置 ok
div && css( div, 'width', '100px' );
// 获取 ok
div && css( div, 'width' );
```

我们可以根据实际的需要进行规定那些参数是可传或可不传的

### 默认参数

我们知道在`js`当中存在默认 参数而在`ts`当中也是存在默认参数的

- 有默认值的参数也是可选的
- 设置了默认值的参数可以根据值自动推导类型

```
function sort(items: Array<number>, order = 'desc') {}
sort([1,2,3]);
```

### 剩余参数

顾名思义：就是我获取到我需要用的参数后剩下的我统一存在一个变量里面

```
interface IObj {
    [key:string]: any;
}
function merge(target: IObj, ...others: Array<IObj>) {
    return others.reduce( (prev, currnet) => {
        prev = Object.assign(prev, currnet);
        return prev;
    }, target );
}
let newObj = merge({x: 1}, {y: 2}, {z: 3});
```

### 函数中的 this

```
interface T {
    a: number;
    fn: (x: number) => void;
}

let obj1:T = {
    a: 1,
    fn(x: number) {
        //any类型
        console.log(this);
    }
}
```

在我们的`ts`当中`this`的类型为`any` 也就是任意类型 我们应该如何进行设置类型呢

![tpyeScript](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201106125243110.png)

#### 普通函数

我们可以在函数的第一个参数位（它不占据实际参数位置）上显式的标注 `this` 的类型

```
interface T {
    a: number;
    fn: (x: number) => void;
}

let obj2:T = {
    a: 1,
    fn(this: T, x: number) {
        //通过第一个参数位标注 this 的类型，它对实际参数不会有影响
        console.log(this);
    }
}
obj2.fn(1);
```

#### 箭头函数

箭头函数的 `this` 不能像普通函数那样进行标注，它的 `this` 标注类型取决于它所在的作用域 `this` 的标注类型

```
interface T {
    a: number;
    fn: (x: number) => void;
}

let obj2: T = {
    a: 2,
    fn(this: T) {
        return () => {
            // T
            console.log(this);
        }
    }
}
```

### 函数重载

有的时候，同一个函数会接收不同类型的参数返回不同类型的返回值，我们可以使用函数重载来实现，通过下面的例子来体会一下函数重载

我们定义个函数然后接收三个参数：dom对象，string ，'block'|'none'|number

```
function showOrHide(ele: HTMLElement, attr: string, value: 'block'|'none'|number) {
	//
}
```

我们进行调用测试

```
let div = document.querySelector('div');
if (div) {
  showOrHide( div, 'display', 'none' );
  showOrHide( div, 'opacity', 1 );
	// error，
	// 这里是有问题的
	// 虽然通过联合类型能够处理同时接收不同类型的参数
	// 但是多个参数之间是一种组合的模式，我们需要的应该是一种对应的关系
  showOrHide( div, 'display', 1 );
}
```

我们用函数重载的方式将上面重新定义一下

```
// 处理 display 的规则
function showOrHide(ele: HTMLElement, attr: 'display', value: 'block'|'none');
// 处理 opacity 的规则
function showOrHide(ele: HTMLElement, attr: 'opacity', value: number);
// 处理逻辑
function showOrHide(ele: HTMLElement, attr: string, value: any) {
  ele.style[attr] = value;
}
// 获取dom
let div = document.querySelector('div');
if (div) {
  showOrHide( div, 'display', 'none' );
  showOrHide( div, 'opacity', 1 );
  // 通过函数重载可以设置不同的参数对应关系
  showOrHide( div, 'display', 1 );
}
```

