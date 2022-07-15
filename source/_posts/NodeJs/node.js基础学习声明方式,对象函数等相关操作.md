---
title: 'node.js基础学习声明方式,对象函数等相关操作'
categories: 技术笔记
tags:
  - node
  - 学习笔记
excerpt: 学习node的基础相关变量的声明对象的声明和数组怎么使用的相关的操作方法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921221118.png'
articleLink: 0a53cbb7d4581901
date: 2020-07-01 19:34:19
---

## Nodejs浏览器运行和服务器的区别

1. js都是运行在浏览器端的
   1. ECMAScript：js语法（变量,表达式,数据类型）
   2. Bom：浏览器对象模型，用js来操作浏览器窗口
   3. Dom：文档对象模型，用js去操作页面上的Dom树
2. node.js是运行在服务器端的
   1. 没有Bom和Dom只有ECMAScript

```js
//正常的js node运行
var name = "乔越";
console.log(name);

//错误运行
console.log(document);
///------ 依然报错
alert("node 运行不了 这是浏览器弹窗")

```

## 声明方式

### var声明变量

	1. 有变量提升
 	2. 没有块级作用 是函数作用域
 	3. 可以重复声明
 	4. 可以重新赋值

### let声明变量

 1. 没有变量提升

    ```js
    console.log(age);//报错
    let age = 38
    ```

 2. 有块级作用

    ```js
    for(let i = 0; i<10;i++){}
    console.log(i)//报错
    ```

 3. 不可以重复声明

    ```js
    let num = 10
    let num = 20
    console.log(unm)//报错
    ```

 4. 可以重新赋值

    ```js
    let name = "乔越"
    name = "node"
    console.log(name)//正常
    ```

### const声明变量

一旦声明就无法改变

1. 没有变量提升

2. 块级作用域

   ```js
   {
   	const num = 20;
   }
   console.log(num) //报错
   ```

3. 不能重复声明

4. 不能重复赋值（声明就必须初始化）

   ```js
   const name = "乔越"
   name = "node"
   console.log(name)//报错
   ```

## 对象

### 对象结构赋值

```js
//声明一个对象
let obj = {
	name : "乔越",
	age : 22,
};
//es5
let name1 = obj.name;
console.log(name1)//正常
//es6
let {
  	name:name1,
  	age:age1
} = obj;
console.log(name1,age1)
```

```js
//思考
let {
	name:name1,
  	age:age1,
  	fen:fen1,
}
console.log(name1,age1,fen1)
//fen1打印是 undefined 没有赋值
```

### 对象成员简写

```js
let name = "乔越"
let age = 22
let fff = 100
//声明了一个对象
//对象里面有上面的属性
//希望这些属的值对应上面的值
//es5
obj = {
	name:name,
	age:age,
	sayHi:function(){
		console.log("乔越")
	}
}
obj.sayHi();

//es6 对象成员的简写
let obj2 = {
  name,//相当于 name:name
  age,
  fenshu,//这里报错
  fenshu2:fff,//这就不会报错了 
  sayHi(){
    console.log("乔越")
  }
}
obj2.sayHi();
```

### 对象展开

展开运算符 ...

```js
let qiao = {
	name:"乔越",
	age:22,
	sayHi(){
		console.log("乔越")
	}
	
}
//展开
let linge = {
	...qiao,
	xingbie:"男"
}
console.log(linge);
//结果是和上面那个qiao的对象内容一模一样
```



## 数组

### 数组的结构赋值

就是把数组中的每一个元素的值依次赋值给变量

```js
//声明一个数组
let arr = [10,20,40,52]
//es5
let num1 = arr[0]
//es6
let [num1,num2] = arr
console.log(num1)
```

### 数组展开

展开运算符 ...

1. 数组拼接

```js
let arr1 = [10,34,5]
let arr2 = [...arr1,234,64]
console.log(arr2) //在arr1上加上 234 64
```

```js
//最大值
let arr1 = [10,34,5]
//以前的做法
let max1 = Math.max.apply(Math,arr1)
//展开 做法
let max2 = Math.max(...arr1)
```

### forEach()

遍历数组用 把每一项交给回掉函数 进行处理

没有返回值

```js
let arr = [10,20,30,40]
arr.forEach(function(item,index){
	//item 每一项
	//index 每一项的索引
})
```

### map()

有返回值

```js
let arr = [10,33,13,20,30,40]
let newmap = arr.map(function(item,index){
			//item 每一项
			//index 每一项的索引
			
  		//这个会存在一个新的数组
			return item * index
})
console.log(newmap)//新的数组
```

### filter()

过滤器

会创建一个新的数组 新的数组中的元素是通过检查后符合条件的元素

```js
let arr = [10,33,13,20,30,40]
arr.filter((item,index)=>{
			//item 每一项
			//index 每一项的索引
			
			return item % 2 = 0;
})
```



## 函数声明

```js
//es5
function test1(name,age){
	console.log(name,age)
}
test1("乔越",22);
//行参如果多就写成一个对象
function test1(obj){
	console.log(obj.name,obj.age)
}
let obj = {
	name:"乔越",
	age:22
}
test1(obj);

//es6
function test2(name,age){
	console.log(name,age)
}
//可以直接对应上
test2({
  	name:'乔越',
  	age:22
})
```

### 箭头函数

​	说白了就是匿名函数的简写

	1. fun 改成 =>   =>可以读成 goesto
 	2. 如果只有一个行参那么可以省略行参小括号
 	3. 如果不是一个行参 0个或者多个就不能省略小括号
 	4. 如果函数体只有一句话 那就可以省略函数体的大括号
 	5. 如果函数体只有一句话并且是 return 返回值 那么return也省略
 	6. 如果函数体不是一句话 那就不能省略大括号

```js
//普通匿名函数
let fu = function(name){
	console.log("我的名字"+name)
}
fu2("乔越")
//箭头函数
let fu2 = name=>console.log("我的名字"+name)
fu2("乔越")
```

## 数据类型 Set

Set ：数组去重

作用和数组类似但是不同 他不能放重复的元素

```js
let ste1 = new Set([10,2030,21,22,10])
console.log(ste1)//10,2030,21,22  没有重复的 
```

## 模版字符串

会保留原样的字符格式 以及占位

```js
let str = `
		乔越
  这是一个歌曲
  你说你想怎么
  听一首小歌曲
  真好真好真好
`
console.log(str)
```

```js
let name = "乔越"
console.log(`name: ${name}`)
```

```js
function text(){
	return 'hhhhhh'
}
console.log(`我在笑 ${text()}`)
```

