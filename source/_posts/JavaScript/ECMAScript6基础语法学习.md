---
title: ECMAScript6基础语法学习
categories: 技术笔记
tags:
  - ECMAScript6
excerpt: ECMAScript6基础语法学习与使用详细分析
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922205529.png'
articleLink: 67cbf70953923816
date: 2020-09-16 16:45:38
---

## ECMAScript6

### let

1. let同一作用域不可以重复声明（或者说同一作用域）

   1. 全局作用域 和 块级作用域 { }

   2. let声明后调用顺序 不对的时候会直接报错 （不会进行预解析）

      ```js
      console.log(a)
      let a = 1
      ```

      ![image-20200916165444120](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200916165444120.png)

2. var可以重复声明

   1. 作用域：全局作用域 函数作用域

   2. 在声明为var 调用顺序时候 不会报错 会显示为 undefined （会先进行预解析）

      ```js
       console.log(a)
       var a = 1
      ```

      ![image-20200916165237031](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200916165237031.png)

3. 总结就是：写在下面的东西 在上面就可以调用反而能用会造成代码混乱，而用let 则就遵循先定义后使用 会使代码更加清晰

### const 常量

必须定义就赋值 一旦声明就不可以改变了 不可以重新赋值 为块级作用域 不会被预解析

```js
const a = undefined
```

目前的兼容性还是有问题的 其他情况和 let 相同

### 块级作用域

```js
{
	//这里面都属于这个块内
}
```

```html
<!-- html -->
<ul>
   <li>第一块</li>
   <li>第二块</li>
   <li>第三块</li>
 </ul>
```

```js
let lis = document.querySelectorAll('li')
for(let i = 0;i<lis.length;i++){
  lis[i].onclick = function(e){
  	console.log(i)
  }
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgSep-16-2020%2020-40-04.gif)

### 结构赋值

##### 对象结构赋值

```js
let obj = {
  a:1,
  b:2
}
// let a = obj.a
// let b = obj.b
// console.log(a,b)//=>1,2
let {a,b} = obj 
console.log(a,b)//=>1,2
```

注意：{a,b} 的名字必须是和 obj 当中的一一对应的 没有的话值为：undefined

##### 对象结构赋值

```js
let arr = ["a","b"]
let [e,f] = arr
console.log(e,f)
```

注意：数组的结构赋值只需要对应上位置即可

**面试题**

```js
let a1 = 1;
let b1 = 2;
// 如何快速交换 a b 的值
[a1,b1] = [b1,a1]
console.log(a1,b1)
```

错误点：`Uncaught ReferenceError: Cannot access 'b' before initialization`

这个错误是因为我没有在 let 变量后面加上 ; 分号

##### 字符串结构赋值

```js
let str = "abcd"
let [q,y] = str
console.log(q,y) //=> a , b
```

### 展开运算符

```js
let arr = [1,2,4,5]
let arr2 = ["a","b","c","d"]
```

将 arr 放在 arr2 的中间位置

```js
let arr2 = ["a","b",...arr,"c","d"]
//=> ["a", "b", 1, 2, 4, 5, "c", "d"]
```

##### 剩余参数

就是将前面拿掉的所剩下的都给我

```js
let arr = [1,2,4,5]
let [q,y,...n] = arr
console.log(q,y,n)
//=> 1 2 (2) [4, 5]
// 对象同理
```

##### 问题 注意点

```
let obj1 = {
  a:1,
  b:2
}
let obj2 = obj1
obj2.a = 11
console.log(obj1) //=> 11 2
```

这样会将 obj1 中的值也给改掉 因为传递的是地址

```
let obj1 = {
  a:1,
  b:2
}
let obj2 = {...obj1}
obj2.a = 11
console.log(obj1) //=> 1 2
```

这样只是传递值了 所以obj1 没有被改掉

### Set 对象

构造函数 用来构建某一类型的对象 （对象的实例化）

```js
let arr = [1,2,35,6,78,34,1,2]
let s = new Set(arr)
console.log(s) //=> {1, 2, 35, 6, 78,34}
```

去除重复出现的数据//=>会保留第一次出现的值

#### size

数值的个数 相当于 length

```js
console.log(s.size) 
```

#### clear()

```js
s.clear()
//清空所有值
```

#### delete()

```js
s.delete(35)
//删除某一项
```

#### add()

```js
s.add('qy')
//添加某个值
```

返回的是本身所以可以 链式添加

```js
s.add('qy').add(1).add('nb')
```

#### has()

```js
s.has('qy')
//查看是否包含某个值
```

### Map 对象

```js
let arr = [
  ["a",1],
  ["b",2],
  ["c",3],
  ["d",4]
]
let m = new Map(arr)
```

返回格式

```js
Map(4) {"a" => 1, "b" => 2, "c" => 3, "d" => 4}
0: {"a" => 1}
1: {"b" => 2}
2: {"c" => 3}
3: {"d" => 4}
```

#### size

数值的个数 相当于 length

```js
console.log(m.size) 
```

#### clear()

```js
m.clear()
//清空所有值
```

#### delete()

```js
m.delete(35)
//删除某一项
```

#### get()

```js
m.get('b')
//获取指定key的值
```

#### set()

```js
m.set(key,vaule)
//添加某个值
```

返回的是本身所以可以 链式添加

```js
m.set('qy','2').set('d',5).set('nb','342')
```

#### has()

```js
m.has('b')
//指定key 是否存在
```

## 函数新增内容

### 箭头函数

```js
let fn = ()=>{
  //执行语句
		console.log("剪头函数")
  return "返回值"
}
fn()
```

```js
// 行参 => 返回值
let fn = num => num*2
console.log(fn(2)) //=>4
```

```js
//(行参，行参)=>返回值
let fn = (num1,num2) => num1*num2
console.log(fn(2,10)) //=>20
```

#### 不定参

```js
 function fn(){
 		//不定参
 		console.log(arguments)
 }
 fn(1,2,3,4)
```

![image-20200916213913407](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200916213913407.png)

剪头函数没有 不定参数 **arguments**

![image-20200916214031683](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200916214031683.png)

**解决方案**

```js
//跟剩余参数其实是一个道理
//rest 参数
let fn = (...arr)=>{
		console.log(arr)
}
fn(1,2,3,4,23)
```

![img](http://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg/imgimage-20200916214318682.png)

##### This

```js
document.onclick = function (){
    let fn = ()=>{
      	console.log(this)
    }
    function fn2() {
    		console.log(this)
    }
    fn(); //=>document
    fn2();//=>window
}
```

箭头函数本身没有this  调用箭头函数的this时候 指向是其声明时 所在的作用域 this

```js
let fn;
let fn2 = function () {
    console.log(this) //=> window
    fn = ()=>{
        console.log(this)//=> window
		}
}
fn2()
fn()
```

这个是跟随 fn2的变化进行变化的

```js
let fn;
let fn2 = function () {
    console.log(this) //=> body
    fn = ()=>{
        console.log(this)//=> body
		}
}
fn2 = fn2.bind(document.body)
fn2() 
fn()
```

#### 参数默认值

```
let fn = (a=10,b=2) =>{
		console.log(a,b)
}
console.log(fn(1)) //=> 1,2
```

## 数组新增方法

### Array.from() 

把一个类数组转换成真正的数组

类数组：有下标 有 length

```js
let lists = document.querySelectorAll('li')
lists =  Array.from(lists)
console.log(lists)
```

返回值：转换之后的新数组

```js
let lists = document.querySelectorAll('li')
lists =  Array.from(lists,(item,index)=>{
      console.log(item,index)
      return index
})
console.log(lists)
```

![image-20200916224042391](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20200916224042391.png)

```js
lists = [...lists]
//也可以转换成一个数组
//展开运算符号 展开进去
```

### Array.of

将参数转成一个数组

```js
console.log(Array.of(1,2,4,43))
```

### Array.isArray

判断某个是否是 数组

```js
let lists = document.querySelectorAll('li')
console.log(Array.isArray(lists))//=> false 
```

### arr.find

查找数组中满足要求的第一个元素的值

在数组每一项上执行的函数，接收 3 个参数：

1. element  当前遍历到的元素。
2. index[可选] 当前遍历到的索引。
3.  array[可选] 数组本身

数组中第一个满足所提供测试函数的元素的值，否则返回 undefined

```js
let arr = [1,2,3,4,5,775,32,232]
arr.find((item,index)=>{
    if(item%2 == 0){
    		console.log(item)
    }
})
```

### arr.findIndex

查找数组中满足要求的第一个元素的值的索引

参数：

1. element  当前遍历到的元素。
2. index 当前元素的索引。
3.  array 调用findIndex的数组。

```js
let arr = [1,2,3,4,5,775,32,232]
arr.findIndex((item,index)=>{
    if(item%2 == 0){
    		console.log(item)
    }
})
```

### arr.flat

扁平化多维数组

二维数组转换为一维数组

```js
let arr = [
    ["11","oo"],
    ["22","kk"],
    ["33","ddf"],
    [
        [5,"d"],
        [3,"ggs"],
        [2,35],
    		[
   			 	['1d','ff']
   			],
    ]
]
//Infinity 代表无限层 也可以填整数
console.log(arr.flat(Infinity))
```

### arr.flatMap

方法首先使用映射函数映射每个元素，然后将结果压缩成一个新数组。它与 map 和 深度值1的 flat 几乎相同，但 flatMap 通常在合并成一种方法的效率稍微高一些

```js
let newArr = arr.flatMap((item,index)=>{
  console.log(item,index);
  item = item.filter((item,index)=>{
    return index == 0
  })
  return item
})
console.log(newArr)
```

只能处理深度为1层 如果想在往下进行处理可以使用递归算法

### arr.fill

用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引

参数：用来填充数组元素的值。
可选参数：
start： 起始索引，默认值为0。
end ：终止索引，默认值为 arr.length    

```js
let arr = [0,1,2,3,4,5]
arr.fill('q',1)
console.log(arr) //=>(6) [0, "q", "q", "q", "q", "q"]
```

### arr.includes

判断数组中是否包含一个指定的值

参数：

1. 必须参数 要查什么参数
2. 可选参数 从第几位开始查找

```
let arr = [0,1,2,3,4,5]
console.log(arr.includes(3,5))
```

## 字符串新增方法

### str.includes

判断字符串是否包含一个指定的值

参数：

1. 必须参数 要查什么参数
2. 可选参数 从第几位开始查找

```
let str = 'qjhsfdsf'
console.log(str.includes('q',5))
```

### str.startsWith

判断当前字符串是否以另外一个给定的子字符串开头

参数：

1. 为要判断是否的字符串
2. 从第几位开始

```js
let str  = "qiaoyue"
console.log(str.startsWith("q")) //=>true
```

### str.endsWith

判断当前字符串是否是以另外一个给定的子字符串“结尾”

参数：

1. 为要判断是否的字符串
2. 这个字符串的长度

```js
let str  = "qiaoyue"
console.log(str.endsWith("q")) //=> false
```

### str.repeat

构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本

```js
let str  = "qiaoyue"
console.log(str.repeat(2)) //=> qiaoyueqiaoyue
```

### 模版字符串

${} 插值表达式

```js
let name = "qiaoyue"
let str = `你是谁${name}`
console.log(str) //=> 你是谁qiaoyue
```

## 对象新增方法

### 简洁表示法

```js
let a = 1;
let b = 2;
let obj = {
	a,
	b
}
```

### 属性名表达式

```js
let name = "小明"
let obj = {
		c(){
			console.log('a')
		},
		[name]:'woshi'
};
//obj[name] = 111
console.log(obj)
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916232716.png)

### Object.assign

将所有可枚举属性的值从一个或多个源对象复制到目标对象

第一个参数为目标对象 其他的都是	源对象

```js
let obj1 = {
    a:1,
    b:2
}
let obj2 = {
    c:3,
    d:4
}
obj2 = Object.assign({},obj2,obj1)
console.log(obj2) //=> {c: 3, d: 4, a: 1, b: 2}
```

可以用展开运算符进行 展开运算符还可以想放在哪就放在哪里

### Object.is

判断两个值是否是相同的值。 

规则：
    两个值都是 undefined
    两个值都是 null
    两个值都是 true 或者都是 false
    两个值是由相同个数的字符按照相同的顺序组成的字符串
    两个值指向同一个对象
    两个值都是数字并且
        都是正零 +0
        都是负零 -0
        都是 NaN

```js
 console.log(Object.is(1,'1'))
```

