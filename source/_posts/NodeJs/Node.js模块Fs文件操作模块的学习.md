---
title: Node.js模块Fs文件操作模块的学习
categories: 技术笔记
tags:
  - 模块
  - fs
  - node
  - buffer
excerpt: Node.js模块Fs文件操作模块的学习buffer学习了解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img594b7b7d1e884.png'
articleLink: f70215f753720924
date: 2020-09-24 10:04:09
---

## 模块

在 Node.js 模块系统中，每个文件都被视为独立的模块。

### 导入模块

引入一个文件 形式模块

```js
//通过require来引入
require('路径')//注意一定要有"./"，文件后缀可加可不加。
```

### 导出模块

#### module.exports

```js
let a = 10;
class Person{
    constructor(){
        this.name = "张三";
    }
    hobby(){
        console.log("喜欢篮球");
    }
}
module.exports = {
     a,
   Person
}
```

### exports

```js
let a = 10;
class Person{
    constructor(){
        this.name = "张三";
    }
    hobby(){
        console.log("喜欢篮球");
    }
}
exports.a = a;
exports.Person = Person;
// exports 是 module.exports 的引用；
// module.exports = exports;

```

### node_modules

里面有`package.json`配置文件  然后可以直接倒入这个文件下面 模块包

如下面有个 mymo的文件 里面有`package.json` 并且 main 字段为 `index.js` 那么我导入这个文件夹的时候就会去查找 `index.js` 这个文件然后倒入

```
name-包名
version--包的版本号。
description-包的描述。
homepage-包的官网RL
author- https: //npmjs.org,"<包的作者,它的值是你在https:npmjs.org网站的有效账户名,遵循账户名<邮件>的规则,例如: zhangsan
contributors-包的其他贡献者。
dependencies/ devDependencies-生产开发环境依赖包列表。它们将会被安装在
node_module目录下。
repository-包代码的Repo信息,包括type和URl,type可以是git或svn,L则是包的Repo地址。
main - main字段指定了程序的主入口文件, require(moduleName'')就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js
keywords-关键字
```

## fs文件操作

### 文件操作

所有的文件操作 没有加Sync 都是异步操作 否则就是同步操作

#### 文件读取

**异步读取**

```js
let fs = require("fs");
fs.readFile("1.txt",(err,data)=>{
    if(err){
        return console.log(err);
    }
    console.log(data.toString());
})
```

**同步读取**

```js
let fs = require("fs");
let res = fs.readFileSync("1.txt");
console.log(res.toString());
```

#### 文件写入

```js
let fs = require("fs");
//flag配置  "a":追加写入，"w":写入，"r":读取
fs.writeFile("2.txt","我是要写入的内容",{flag:"w"},err=>{
    if(err){
        return console.log(err);
    }
    console.log("写入成功");   
})
```

#### 文件删除

```js
fs.unlink("2.txt",err=>{
    if(err){
        return console.log(err);
    }
    console.log("删除成功");
})
```

#### 复制文件

先读取文件再写入文件

```js
fs.copyFile("index1.html","index2.html",err=>{
	if(err){
		return console.log("失败")
	}
	console.log("成功")
})
```

自己也可以写出来个这个复制

```js
function mycopy(src,dest){
   fs.writeFileSync(dest,fs.readFileSync(src));
}
mycopy("1.txt","4.txt");
```



#### 修改文件名

```js
fs.rename("1.txt","5.txt",function (err) {
    if(err){
        console.log(err);
    }else{
        console.log("修改成功");
    }
});
```

### 目录操作

#### 创建目录

```js
fs.mkdir("new",err=>{
	  if(err){
        console.log(err);
    }else{
        console.log("修改成功");
    }
})
```

#### 修改目录名称

```js
fs.rename("new","new2",err=>{
	  if(err){
        console.log(err);
    }else{
        console.log("修改成功");
    }
})
```

#### 读取目录

目录下面得有东西哦！

```js
fs.readdir("new",(err,data)=>{
	  if(err){
        console.log(err);
    }else{
        console.log(data);
    }
})
```

目录下文件是有后缀的 目录下的目录是没有后缀的

#### 删除目录

条件：必须是空的目录（文件）

```js
fs.rmdir("new",(err)=>{
	  if(err){
        console.log(err);
    }else{
        console.log(data);
    }
})
```

#### 删除非空文件夹

思路先把目录里面的文件删除在删除目录

- 获取目录下面的所有文件 文件夹

- ["33","1.txt","2.html"] 上面说过 文件会有后缀 文件夹没有后缀

- 我们不用后缀这个方法 又个更方便的 获取详细信息 里面可以直接判断是文件还是文件夹

- 是文件就直接删除 文件夹就递归在进去查找

```js
function removeDir(path){
   let data = fs.readdirSync(path);
    // ["33","1.txt","2.html"];
   for(let i=0;i<data.length;i++){
        // 是文件或者是目录； --->?文件 直接删除？目录继续查找；  
        let url = path + "/" + data[i];
        let stat =  fs.statSync(url);
      	//判断是目录还是文件
        if(stat.isDirectory()){
            //目录 继续查找；
            removeDir(url);
        }else{
            // 文件 删除
            fs.unlinkSync(url);
        }
   }
    //  删除空目录
   fs.rmdirSync(path);
}
removeDir("22");
```

### 文件目录公共方法

#### 判断目录或文件是否存在

```js
fs.exists("new",function (exists) {
    console.log(exists);
})
```

#### 获取目录或者文件的详细信息

获取到详细信息了后就可以进行判断是否是一个 文件或者文件夹等操作

```js
fs.stat("index.html",(err,data)=>{
		if(err){
        console.log(err);
    }else{
        console.log(data);
        //判断文件是否是文件
        let res = data.isFile()
        console.log(res)
        // 判断是否是一个文件夹
        let res2 = data.isDirectory()
        console.log(res2)
    }
})
```

## buffer

用于表示固定长度的字节序列。

`Buffer` 类在全局作用域中，因此无需使用 `require('buffer').Buffer`。

`Buffer` 类用于直接处理二进制数据。 它可以使用多种方式构建。

#### Buffer.alloc(size[, fill[, encoding]])

分配一个大小为 `size` 字节的新 `Buffer`。 如果 `fill` 为 `undefined`，则用零填充 `Buffer`。

```js
let buffer = Buffer.alloc(10)
console.log(buffer)
//=> <Buffer 00 00 00 00 00 00 00 00 00 00>
```

#### Buffer.from(array)

使用 `0` – `255` 范围内的字节数组 `array` 来分配一个新的 `Buffer`。 超出该范围的数组条目会被截断以适合它。

```js
// 创建一个包含字符串 'buffer' 的 UTF-8 字节的新 Buffer。
const buf = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);
```

如果 `array` 不是一个 `Array` 或适用于 `Buffer.from()` 变量的其他类型，则抛出 `TypeError`。

#### Buffer.from(string[, encoding])

创建一个包含 `string` 的新 `Buffer`。

 `encoding` 参数指定用于将 `string` 转换为字节的字符编码。

```js
const buf1 = Buffer.from('this is a tést');
const buf2 = Buffer.from('7468697320697320612074c3a97374', 'hex');

console.log(buf1.toString());
// 打印: this is a tést
console.log(buf2.toString());
// 打印: this is a tést
console.log(buf1.toString('latin1'));
// 打印: this is a tÃ©st
```

#### Buffer.isBuffer(obj)

如果 `obj` 是一个 `Buffer`，则返回 `true`，否则返回 `false`。

