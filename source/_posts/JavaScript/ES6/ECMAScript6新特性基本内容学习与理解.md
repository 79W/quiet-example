---
title: ECMAScript6新特性基本内容学习与理解
categories: 技术笔记
tags:
  - ECMAScript6
excerpt: ECMAScript6新特性基本内容学习与理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201220111059.png'
articleLink: 40d369e8
date: 2020-12-20 08:24:25
---

### let作用域

let 语句声明一个块级作用域的本地变量

```
let x = 1;
if (x === 1) {
  let x = 2;
  console.log(x);
  // expected output: 2
}
console.log(x);
// expected output: 1
```

应用场景：循环绑定事件，事件处理函数中获取正确索引

我们使用 Var 声明 一个循环遍历进行绑定事件 从而打印相对应的变量

```
var elements = [{}, {}, {}]
for (var i = 0; i < elements.length; i++) {
  elements[i].onclick = function () {
    console.log(i)
  }
}
elements[0].onclick() // 3
```

上面的不管怎么打印 值总是最后一个数值3 我们需要采用闭包的形式解决

```
var elements = [{}, {}, {}]
for (var i = 0; i < elements.length; i++) {
    elements[i].onclick = (function (i) {
        return function () {
            console.log(i)
        }
    })(i)
}
elements[0].onclick()
```

虽然解决了问题但是 很复杂麻烦不易理解 

我们使用 let 来进行定义 

```
var elements = [{}, {}, {}]
for (let i = 0; i < elements.length; i++) {
  elements[i].onclick = function () {
    console.log(i)
  }
}
elements[0].onclick()
```

这样也是正常 因为let 是块级作用域 互不影响

#### 声明提升

我们声明Var 变量的时候可以先调用后声明

```
console.log(foo)
var foo = 'zce' //undefined
```

并且不会报错 但是这样 可以理解为一个 bug

```
// console.log(foo)
// let foo = 'zce' // 报错
```

然而使用 let 必须先声明在调用 否则报错

### const

常量是块级范围的，非常类似用 let语句定义的变量。

但常量的值是无法（通过重新赋值）改变的，也不能被重新声明。

```
// const name = 'zce'
// 恒量声明过后不允许重新赋值
// name = 'jack'
```

```
// 恒量要求声明同时赋值
// const name
// name = 'zce'
```

### 数组解构

ES5中，为了从数组中获取特定数据并赋值给变量

```
var arr = ["A", "B", "C"];
var a = arr[0],
    b = arr[1],
    c = arr[2];
console.log(a); //输出：A
```

而在ES6中我们可以解构出来

```
let [a, b, c] = ["A", "B", "C"];
console.log(a);//输出：A
```

还可以拿某个位置的单个值

```
let [, , c] = ["A", "B", "C"];
console.log(c); //输出：C
```

我们也可以在数组解构赋值表达式中为任意变量添加默认值。只有当该位置在右侧数组中的值不存在时或值为undefined时，默认值才会生效。

```
const arr = ["A", "B"];
let [a, b, c = "C"] = arr;
console.log(c); //输出：C
```

#### 展开运算符

在数组解构的最后一个例子中，我们在变量名前添加三个点（…）将这个变量变成了不定元素。

这三个点叫作展开运算符。

展开运算符用于展开数组。

```
let arr = [2, 3, 4];
console.log(1, ...arr, 5); //输出：1 2 3 4 5
```

我们还可以在函数参数的地方直接使用如

```
function foo (first, ...args) {
  console.log(args)
}
foo(1, 2, 3, 4)
```

这样我们 `first` 接受到 1 剩下的全部到`args` 中形成一个数组 也称为剩余参数

### 对象解构

其实和数组解构大同小异

```
const obj = { name: 'zce', age: 18 }
```

我们想拿到 `name` 值可以

```
const { name } = obj
console.log(name)
```

有时候和我们自己定义的变量有冲突的时候我们还可以使用别名

```
const name = 'tom'
const { name: objName } = obj
console.log(objName)
```

后面加上等于号也是可以赋值默认值

### 模版字符

```
const str = `hello es2015, this is a string`
```

我们可以直接换行

```
const str = `hello es2015,
							this is a string`
```

我么还可以在里面插入变量

```
const name = 'tom'
// 可以通过 ${} 插入表达式，表达式的执行结果将会输出到对应位置
const msg = `hey, ${name} --- ${1 + 2} ---- ${Math.random()}`
console.log(msg)
```

### 字符串拓展

```
const message = 'Error: foo is not defined.'
```

#### startsWith

方法用来判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回 `true` 或 `false`。

```
message.startsWith('Error')
```

#### endsWith

方法用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 `true` 或 `false`。

```
message.endsWith('.')
```

#### includes

方法用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回false。

```
message.includes('foo')
```

### 箭头函数

特别注意`this`的指向问题

我们在对象中定一个变量和一个方法 方法中进行调用自身的变量是调用不到的

```
const person = {
  name: 'tom',
  sayHi: () => {
    console.log(`hi, my name is ${this.name}`)
  },
}
person.sayHiAsync() //拿不到 name
```

我们如果使用 普通函数就是可以的

```
const person = {
  name: 'tom',
  sayHi: function () {
  	 console.log(`hi, my name is ${this.name}`)
	}
}
person.sayHiAsync() // 可以拿到
```

### 对象

在ES6中我们可以直接在对象中声明变量的键

```
const obj = {
  foo: 123,
  // Math.random(): 123 // 不允许
  // 通过 [] 让表达式的结果作为属性名
  [Math.random()]: 123
}
```

我们在内部声明函数可以省去 ` function`

```
const obj = {
  foo: 123,
  // method1: function () {
  //   console.log('method111')
  // }
  // 方法可以省略 : function
  method1 () {
    console.log('method111')
    // 这种方法就是普通的函数，同样影响 this 指向。
    console.log(this)
  },
}
```

#### assign

用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```
const source1 = {
  a: 123,
  b: 123
}

const source2 = {
  b: 789,
  d: 789
}

const target = {
  a: 456,
  c: 456
}
// 后面的覆盖前面的
const result = Object.assign(target, source1, source2)

console.log(target) // { a: 123, c: 456, b: 789, d: 789 }
console.log(result === target) // true
```

#### is

方法判断两个值是否为同一个值

```
console.log(
  // 0 == false              // => true
  // 0 === false             // => false
  // +0 === -0               // => true
  // NaN === NaN             // => false
  // Object.is(+0, -0)       // => false
  // Object.is(NaN, NaN)     // => true
)
```

### Set 数据结构

允许你存储任何类型的唯一值

```
const s = new Set()
// 我们可以连续添加 但是结果会和我们添加的不一样
s.add(1).add(2).add(3).add(4).add(2)
// 结果是去掉重复的数据 
// 1,2,3,4
```

#### 应用场景：数组去重

```
const arr = [1, 2, 1, 3, 4, 1]

// const result = Array.from(new Set(arr))
// 解构赋值
const result = [...new Set(arr)]

console.log(result)
```

### Map 数据结构

Map数据结构类似于对象，也是键值对的集合，但是*“键”的范围不限于字符串*，各种类型的值（包括对象）都可以当作键

**Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。**

```
const map = new Map();

const k1 = ['a'];
const k2 = ['a'];

map
.set(k1, 111)
.set(k2, 222);

map.get(k1) // 111
map.get(k2) // 222
```

#### Map实例的操作方法

1. keys()：返回键名的遍历器。
2. values()：返回键值的遍历器。
3. entries()：返回所有成员的遍历器。
4. forEach()：遍历 Map 的所有成员。

```
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()] // [1, 2, 3]

[...map.values()] // ['one', 'two', 'three']

[...map.entries()] // [[1,'one'], [2, 'two'], [3, 'three']]

[...map] // [[1,'one'], [2, 'two'], [3, 'three']]
```

### Symbol

是一种基本数据类型 每个从`Symbol()`返回的symbol值都是唯一的。一个symbol值能作为对象属性的标识符；这是该数据类型仅有的目的

```
const symbol1 = Symbol();
const symbol2 = Symbol(42);
const symbol3 = Symbol('foo');

console.log(typeof symbol1);
// expected output: "symbol"

console.log(symbol2 === 42);
// expected output: false

console.log(symbol3.toString());
// expected output: "Symbol(foo)"

console.log(Symbol('foo') === Symbol('foo'));
// expected output: false

```

### for...of 循环

```
const arr = [100, 200, 300, 400]

for (const item of arr) {
  console.log(item)
}
```

普通对象不能被直接 for...of 遍历

```
const obj = { foo: 123, bar: 456 }
for (const item of obj) {
  console.log(item)
}
```

