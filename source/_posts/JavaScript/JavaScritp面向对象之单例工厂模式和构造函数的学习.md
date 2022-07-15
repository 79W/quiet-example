---
title: JavaScritp面向对象之单例工厂模式和构造函数的学习
categories: 技术笔记
tags:
  - JavaScript
  - 面向对象
  - 单例模式
  - 工厂模式
  - 构造函数
excerpt: 'JavaScritp面向对象思想学习,单例模式,工厂模式,构造函数'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922205953.png'
articleLink: a2de14c9ca0b4017
date: 2020-09-17 10:25:40
---

## 单例模式

```
var name1 = 'wangwu';
var age1 = 25;

var name2 = 'zhangsan';
var age2 = 26;
```

以上的写法是描述两个人，每个人都有姓名和年龄，但是每个人的姓名和年龄并没有放在一起，也就是说每个人的年龄和姓名并没有对应起来

#### 对象的概念：

**把描述同一个事物（同一个对象）的属性和方法放在同一个内存空间下面，起到了分组的作用，这样不同事物之间的属性即使属性名相同相互也不会发生冲突**。

```
var person1 = {
    name:'wangwu',
    age:25
};

var person2 = {
    name:'zhangsan',
    age:26
};
```

分组之后，每一个人的姓名和年龄都在同一块内存空间下，也就是每个人的姓名和年龄都对应起来了。

我们也把这种分组编写代码的模式称之为**单例模式**

而 person1，person2 叫做命名空间。

#### 模块化开发

我们可以使用单例模式来进行**模块化开发**

一般情况下会根据当前项目的需求，划分几个功能模块，每个人负责一部分，同时开发，最后把每个人的代码进行合并。

```
var myOn = {
	change:function(){
		this.clickEvent(); // 在自己的命名空间下调用自己命名空间的方法
	},
	clickEvent:function(){
		console.log('clickEvent...')
	}
}
```

```
var youOn = {
	change:function(){
		
	}
}
```

myOn 和 youOn 都有change方法，如果没有分模块来编写的话会造成命名的冲突，按照JavaScript语言的解析规则，后声明的方法会覆盖前面声明的方法。

而通过模块划分之后，这两个模块下的change方法归属于各自的模块，调用的时候都是调用自己模块下的方法不会有冲突。

#### 注意

单例模式解决了分组的问题，让每个对象有了自己独立的命名空间，但是不能批量生产，每一个新的对象都要重新写一份一模一样的代码。

## 工厂模式

把实现同一事情的相同代码，放到一个函数中，以后如果再想实现这个功能，就不需要重新编写这些代码了，只要执行当前的函数即可。

这就是**函数的封装**，体现了**高内聚、低耦合**的思想：减少页面的中的冗余代码，提高代码的重复利用率：

```
function Tab(){
	//添加原料
	let obj = {}
	//加工原料
	obj.tablength = 1
	obj.psFor = function(){
		console.log("psFor...")
	}
	//出厂
	return obj
}
```

#### 问题：

对象识别问题：也就是不能识别出来是来自那个工厂生产出来的 最终指向 Object

性能问题：

```
let str = new String() //==>可以判断类型
```

如果用工厂模式就不可以知道是什么

公共空间：原型 prototype ===》 new 运算符：

## 构造函数

### new 运算符

1. 执行函数
2. 自动创建一个空对象
3. 把创建的对象指向另一个对象
4. 把控对象和函数里面的this 衔接起来
5. 隐式返回this

```
function test(){
	console.log("text...")
}
new test;
//加不加小括号都可以
```

可以用来简化工厂模式 就简化成构造函数了

```
function Tab(){
	//let obj = {} //===> this
	this.name = "张三"
	this.hobby = function(){
		console.log('篮球')
	}
	// return obj
}
let tab1 = new Tab()

```

```
//直接调用函数
var res = myfun('xx' , 7);
```

没有用`new`而直接调用函数的方式，不是构造函数而是普通函数执行

**构造函数模式的目的就是为了创建一个自定义类，并且创建这个类的实例**。

**执行的时候**

1. 普通函数执行：myfun()
2. 构造函数执行：new myfun()，通过new执行后，myfun 就是一个类了，而函数的返回值就是myfun 这个类的一个实例。

### 约定

1. 首字母大写
2. 属性放在构造函数里面
3. 方法放在原型里面

![image-20200917135250967](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200917135250967.png)



```
Tab.prototype.psFor= function(){
	console.log('psFor...')
}
```

每个原型上都有一个预定义的属性： constructor ===》构造函数 会覆盖原本的 constructor属性

下面的写法是不对的

```
Tab.prototype = {
	psFor(){
		console.log('psFor...')
	}
}
```

但是可以在里面写上 constructor

```
Tab.prototype = {
	constructor:Tab,
	psFor(){
		console.log('psFor...')
	}
} 
```

### constructor 用处

判断类型（如果被覆盖了 在用下面的方案 就会出问题）

```
let str = "javascript"
if(str.constructor === String){
		console.log("是字符串")
}else{
		console.log("不是字符串")
}
```

### 仿写 new 运算符

```
function myNew(constructor,...arg){
		//1.创建对象
		let obj = {}
		//2.改变this指向
		constructor.call(obj,...arg)
		//3.原型覆盖
		obj.__proto__ = constructor.prototype
		return obj
}
```

### 私有变量

```
function Fn() {
    var num = 10;
    this.x = 100; // f1.x = 100
    this.getX = function () { // f1.getX=function
        console.log(this.x);
    };
}
var f1 = new Fn;
console.log(f1.num); // -> undefined
console.log(f1.x); //-> 100
```

类有普通函数的一面，当函数执行的时候，`var num`其实只是当前形成的私有作用域中的私有变量而已，它和f1这个实例没有任何的关系，只有`this.xxx=xxx`才相当于给f1这个实例增加私有的属性和方法，才和f1有关系。

### 继承

- 继承：子类继承父类所有属性和行为，父类不受影响。
- 目的：找到类之间的共性精简代码

```
function Person(name){
    this.name = name;
    this.eyes = "两只";
    this.legs = "两条";
}
function Student(name){
    Person.call(this,name)
    this.className = "二班";
}
let newPerson = new Student("张三");
console.log(newPerson.className);
```

简单原型继承,出现影响父类的情况；

```
function Person(name){
    this.name = name;
    this.eyes = "两只";
    this.legs = "两条";
}
function Student(name){
		//继承
    Person.call(this,name)
    this.className = "二班";
}
Student.prototype = Person.prototype  //直接赋值
```

#### 组合继承

```
function Dad(){
    this.name = "张三";
}
Dad.prototype.hobby = function(){
    console.log("喜欢篮球");
}
function Son(){
    Dad.call(this);
}
let F = function(){}
F.prototype = Dad.prototype;
Son.prototype = new F();
Son.prototype.constructor = Son;

let newSon = new Son();
newSon.hobby();
```



## 传值和传地址

传值和传址问题

- 基本数据类型：Number、String、Boolean、Null、Undefined
- 复杂数据类型/引用数据类型:Array、Date、Math、RegExp、Object、Function等

```
let obj = {
		name:"张三",
		age:20
}
let obj2 = obj
obj2.age = 30
console.log(obj) //=> {name:"张三",age:30}
```

如果拷贝对象包含函数，或者undefined等值，此方法就会出现问题

```
let obj = {
		name:"张三",
		age:20,
		test:undefined,
		fn:function(){
			console.log('fn...s')
		}
}
//序列化然后在转化回来
//如果拷贝对象包含函数，或者undefined等值，此方法就会出现问题
let obj2 = JSON.parse(JSON.stringify(obj))
obj2.age = 30
console.log(obj,obj2) 
//=> obj:{name: "张三", age: 20, test: undefined, fn: ƒ} 
//=> obj2:{name: "张三", age: 30}
```

### 深拷贝

```
//递归深拷贝
function deepCopy(obj){
    let newObj = Array.isArray(obj)?[]:{};
    for(let key in obj){
    		if(obj.hasOwnProperty(key)){
            if(obj.hasOwnProperty(key)){
              if(typeof obj[key] == "object"){
                if(obj[key] === null){
                  newObj[key] = null
                }else{
                  newObj[key] = deepCopy(obj[key]);
                }
              }else{
                  newObj[key] = obj[key];
              }
            }
    		}
    }
    return newObj;
}
```

## 原型链

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918115646.png)

## es6类和继承

### 类

```
//类型为 function
class Drag{
		//静态属性
		static height = "188cm"
		//静态方法
		static text(){
			console.log('text...')
		}
		constructor(){
				this.name = "张三"
		}
		hobby(){
				console.log("篮球")
		}
}
console.log(Drag.height)
let drag = new Drag()
console.log(drag)
```

### 继承

#### extends

extends 继承关键字

关键字用于类声明或者类表达式中，以创建一个类，该类是另一个类的子类。

继承的`.prototype`必须是一个`Object`或者 `null`。

```
class ChildClass extends ParentClass { ... }
```

```
class LimitDrag extends Drag{
	constructor(){
		super()
	}
}
```

#### 使用 `extends`与内置对象

这个示例继承了内置的`Date`对象。

```js
class myDate extends Date {
  constructor() {
    super();
  }

  getFormattedDate() {
    var months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
    return this.getDate() + "-" + months[this.getMonth()] + "-" + this.getFullYear();
  }
}
```

#### 扩展 null

可以像扩展普通类一样扩展`null`，但是新对象的原型将不会继承 `Object.prototype`。

```js
class nullExtends extends null {
  constructor() {}
}

Object.getPrototypeOf(nullExtends); // Function.prototype
Object.getPrototypeOf(nullExtends.prototype) // null

new nullExtends(); //ReferenceError: this is not defined

```

## 案例

用鼠标进行拖拽

```
<div class="mydiv1" style="width: 100px;height: 100px;background: red;position: absolute;"></div>
```

#### 面向过程实现

```
 let mydiv1 = document.querySelector('.mydiv1')
 mydiv1.onmousedown = (e)=>{
     let ev = e || window.event
     let x = ev.clientX - mydiv1.offsetLeft
     let y = ev.clientY - mydiv1.offsetTop
     mydiv1.onmousemove = (e) =>{
         let ev = e || window.event
         let xx = ev.clientX
         let yy = ev.clientY
         mydiv1.style.left = xx - x + 'px'
         mydiv1.style.top = yy - y + 'px'
     }
     mydiv1.onmouseup = () =>{
     		mydiv1.onmousemove = ""
     }

}
```

#### 面向对象实现

```
 				function Drag(ele){
            this.ele = ele
            this.downFn()
        }

        Drag.prototype.downFn = function(){
            this.ele.onmousedown = (e)=>{
                let ev = e || window.event
                let x = ev.clientX - this.ele.offsetLeft
                let y = ev.clientY - this.ele.offsetTop
                this.moveFn(x,y)
                this.upFn()
            }
        }
        Drag.prototype.moveFn = function(x,y){
            this.ele.onmousemove = (e) =>{
                let ev = e || window.event
                let xx = ev.clientX
                let yy = ev.clientY
                this.ele.style.left = xx - x + 'px'
                this.ele.style.top = yy - y + 'px'
            }
        }
        Drag.prototype.upFn = function(){
            this.ele.onmouseup = () =>{
                this.ele.onmousemove = ""
            }
        }
```

调用

```
let mydiv1 = document.querySelector('.mydiv1')
let drag1 = new Drag(mydiv1)
```

![Sep-17-2020 15-04-40](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgSep-17-2020%2015-04-40.gif)

**通过继承给个别的添加 功能**

```
			function LimitDrag(ele){
            Drag.call(this,ele)

        }
        let Link = function(){}
        Link.prototype = Drag.prototype
        LimitDrag.prototype = new Link()
        LimitDrag.prototype.constructor = LimitDrag
        LimitDrag.prototype.setStyle = function(left,top){
            left = left<0?0:left
            top = top<0?0:top
            this.ele.style.left = left + 'px'
            this.ele.style.top = top + 'px'
        }

        let mydiv1 = document.querySelector('.mydiv1')
        let drag1 = new LimitDrag(mydiv1)
```


