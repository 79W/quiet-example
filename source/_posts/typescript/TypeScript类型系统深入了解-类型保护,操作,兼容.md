---
title: TypeScript类型系统深入了解-类型保护,操作,兼容
categories: 技术笔记
tags:
  - TypeScript
  - 面向对象
excerpt: TypeScript类型系统深入了解-类型保护,操作,兼容
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtypescriptmianxiangduix.png'
articleLink: e5d79585
date: 2020-11-06 18:07:23
---

## 类型保护

我们通常在` JavaScript `中通过判断来处理一些逻辑

在 `TypeScript` 中这种条件语句块还有另外一 个特性:

根据判断逻辑的结果，缩小类型范围(有点类似断言)，这种特性称为 类型保护 ，触发条件:

- 逻辑条件语句块:if、else、elseif 

- 特定的一些关键字:typeof、instanceof、in

### typeof

我们知道 `typeof` 可以返回某个数据的类型，在 `TypeScript` 在` if  else `代码块中能够把 `typeof` 识别为类型保护，推断出适合的类型。

```
function fn(a: string|number) { 
	// error，不能保证 a 就是字符串 
	a.substring(1);
	//我们需要进行类型的判断然后做出响应的反应
  if (typeof a === 'string') {
    // ok
    a.substring(1); 
  } else {
    // ok
    a.toFixed(1); 
  }
}
```

### instanceof

与 `typeof` 类似的， `instanceof` 也可以被 `TypeScript` 识别为类型保护

```
function fn(a: Date|Array<any>) {
	//  判断 a 是不是 Array 类型
  if (a instanceof Array) {
 		 a.push(1); 
 	} else {
  	 a.getFullYear(); 
  }
}
 
```

### in

```
// 定义接口 A
interface IA {
    x: string;
    y: string;
}
// 定义接口 B
interface IB {
    a: string;
    b: string; 
}
然后函数参数的类型为A或者B
function fn(arg: IA | IB) { 
	// 我们需要判断 x 是否存在 arg 中 
	if ('x' in arg) {
      // ok
      arg.x; 
      // error 
      arg.a;
  } else { 
      // ok
      arg.a; 
      // error
      arg.x;
  }
}
```

### 字面量类型保护

如果类型为字面量类型，那么还可以通过该字面量类型的字面值进行推断

```
interface IA { 
    type: 'IA';
    x: string;
    y: string; 
}
interface IB { 
    type: 'IB';
    a: string;
    b: string; 
}

function fn(arg: IA | IB) { 
  if (arg.type === 'IA') {
  		// ok
  		arg.x; 
  		// error 
  		arg.a;
  } else { 
      // ok
      arg.a; 
      // error 
      arg.x;
   } 
}
```

### 自定义类型保护

有的时候，以上的一些方式并不能满足一些特殊情况，则可以自定义类型保护规则

```
// 自定义保护规则
function canEach(data: any): data is Element[]|NodeList { 
		// 返回 true 或则 假
		return data.forEach !== undefined;
}

function fn2(elements: Element[]|NodeList|Element) {
		// 进行函数的传递然后返回真假值
    if ( canEach(elements) ) { 
        elements.forEach((el: Element)=>{
        	el.classList.add('box'); 
        });
    } else { 
    		elements.classList.add('box');
    } 
} 
```

`data is Element[]|NodeList` 是一种类型谓词，格式为: `xx is XX` ，返回这种类型的函数就可以 被 `TypeScript` 识别为类型保护

## 类型操作

`TypeScript` 提供了一些方式来操作类型这种数据，但是需要注意的是，类型数据只能作为类型来使用，而不能作为程序中的数据，这是两种不同的数据，一个用在编译检测阶段，一个用于程序执行阶段

### typeof

在 `TypeScript` 中， `typeof` 有两种作用

- 获取数据的类型
- 捕获数据的类型

```
let str1 = 'qiaoblog';
// 如果是 let ，把 'string' 作为值
let t = typeof str1;
// 如果是 type，把 'string' 作为类型
type myType = typeof str1;
// 使用类型
let str2: myType = '乔越博客';
let str3: typeof str1 = '乔越博客';
```

我们设置的`let` 接收`typeof`是把 `string` 作为了值 而不是当作了类型 所有我们会看到 可能为很多的类型

![typescript](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201106182127267.png)

如果使用的是 `type` 去接收的时候 是将 `string`作为了类型

![typescript](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201106182328801.png)

> 注意：let 会编译到运行中 type 不会编辑只用做类型监测

### keyof

获取类型的所有 key 的集合

```
// 定义一个 接口
interface Person {
    name: string;
		age: number;
};
```

我们使用`keyof`来获取接口中的所有的`key`

```
type personKeys = keyof Person;
// 等同:type personKeys = "name" | "age"
```

我们使用 获取到的key 来获取变量

```
// 定义一个对象
let p1 = {
  name: 'zMouse',
  age: 35 
}
// 进行调用 我们将所有的key 进行传递
function getPersonVal(k: personKeys) {
   	return p1[k];
}
/**
等同:
    function getPersonVal(k: 'name'|'age') { return p1[k];}
*/
getPersonVal('name'); //正确 getPersonVal('gender'); //错误
```

### in

针对类型进行操作的话，内部使用的 for...in 对类型进行遍历

```
interface Person {
    name: string;
    age: number;
}
type personKeys = keyof Person; type newPerson = {
[k in personKeys]: number;
/**
    等同 [k in 'name'|'age']: number; 也可以写成
    [k in keyof Person]: number;
*/
}

/**
type newPerson = {
    name: number;
    age: number;
}
*/
 
```

> 注意: in 后面的类型值必须是 `string` 或者 `number` 或者 `symbol`



## 类型兼容

`TypeScript` 的类型系统是基于结构子类型的

它与名义类型(如:java)不同(名义类型的数据类型 兼容性或等价性是通过明确的声明或类型的名称来决定的)。

这种基于结构子类型的类型系统是基于组 成结构的，只要具有相同类型的成员，则两种类型即为兼容的。

```
 
class Person {
    name: string;
    age: number;
}
class Cat {
    name: string;
    age: number;
}
function fn(p: Person) { 
		p.name;
}
let xiaohua = new Cat();
// ok，因为 Cat 类型的结构与 Person 类型的结构相似，所以它们是兼容的 
fn(xiaohua);
```

