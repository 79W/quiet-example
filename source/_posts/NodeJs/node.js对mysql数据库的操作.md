---
title: node.js对mysql数据库的操作
categories: 技术笔记
tags:
  - node
  - 学习笔记
  - mysql
excerpt: node.js对mysql数据库的操作
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921221118.png'
articleLink: 4708ccd0e1352905
date: 2020-07-05 15:12:29
---

## sql语句

### 插入

```sql
insert into 表名(字段名1，字段名2) values(值1，值2);
```

### 删除

```sql
delete from 表名 where 条件;
```

### 修改

```sql
update 表名 set 字段名1=新值1,字段名2=新值2 where 条件;
```

### 查询

```sql
select * from 表名 [where 条件];
```

## nodejs对数据库的操作

### 模块 mysql

#### 安装模块

`npm i mysql`

#### 导入模块

`var mysql      = require('mysql');`

#### 创建链接

`var connection = mysql.createConnection({ host user password database});`

1. host 数据库服务器地址
2. user 数据库用户名
3. password 数据库密码
4. database 数据库名

#### 打开数据库

`connection.connect();`

#### 执行sql语句

#### 关闭数据库

`connection.end();`

### 查操作

你想操作的数据 只需要更改 `connection.query()` 里面的sql语句即可

```js
//导包
var mysql      = require('mysql');
//创建一个链接服务器
var connection = mysql.createConnection({
	host     : '47.93.184.100',//数据库服务器地址
	user     : 'joe127',//数据库用户名
	password : 'joe127',//数据库密码
	database : 'joe127'//数据库名
});
//打开数据库链接
connection.connect();
//执行sql语句
connection.query('SELECT*from user', (error, results, fields)=> {
	//错误对象 error 如果没有错误为 null
	console.log(error) 
	
	//执行sql后得到的结果集
	console.log(results)
	console.log(results[1].username)
	
	//拿到的是字段信息 不常使用
	console.log(fields)
});
//关闭链接
connection.end();
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgHX5yBAWiM6Pk3cj-20200921225458837.png)

### 增操作

1. results 返回的是一个对象
2. results.affectedRows 受影响的行数 大于0则成功
3. results.insertId 插入这条数据的id

```js
//导包
var mysql      = require('mysql');
//创建一个链接服务器
var connection = mysql.createConnection({
	host     : '47.93.184.100',//数据库服务器地址
	user     : 'joe127',//数据库用户名
	password : 'joe127',//数据库密码
	database : 'joe127'//数据库名
});
//打开数据库链接
connection.connect();

let id = 4
let name1 = "马化腾"
let miao = "真正的大佬"

//执行sql语句
connection.query(`insert into user(ID,username,des) values('${id}','${name1}','${miao}')`, (error, results, fields)=> {
	console.log(results) //返回的是一个对象
	console.log(results.affectedRows) //受影响的行数 大于0则成功
	console.log(results.insertId) //插入这条数据的id
	
});
//关闭链接
connection.end();
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgVt5woXTCr7mAeDq-20200921225540961.png)

### 该操作

```js
//导包
var mysql      = require('mysql');
//创建一个链接服务器
var connection = mysql.createConnection({
	host     : '47.93.184.100',//数据库服务器地址
	user     : 'joe127',//数据库用户名
	password : 'joe127',//数据库密码
	database : 'joe127'//数据库名
});
//打开数据库链接
connection.connect();
//执行sql语句
connection.query(`update user set username='马云' where ID = 2;`, (error, results, fields)=> {
		console.log(results) //返回的是一个对象
//		console.log(results.affectedRows) //受影响的行数 大于0则成功
//		console.log(results.insertId) //插入这条数据的id
});
//关闭链接
connection.end();
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgMPaZKiX3deB8mFs-20200921225623176.png)

### 删操作

```js
//导包
var mysql      = require('mysql');
//创建一个链接服务器
var connection = mysql.createConnection({
	host     : '47.93.184.100',//数据库服务器地址
	user     : 'joe127',//数据库用户名
	password : 'joe127',//数据库密码
	database : 'joe127'//数据库名
});
//打开数据库链接
connection.connect();
//执行sql语句
connection.query(`delete from user where ID = 0;`, (error, results, fields)=> {
		console.log(results) //返回的是一个对象
//		console.log(results.affectedRows) //受影响的行数 大于0则成功
//		console.log(results.insertId) //插入这条数据的id
});
//关闭链接
connection.end();
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgKtb5YxsUX4Evcpk-20200921225642012.png)