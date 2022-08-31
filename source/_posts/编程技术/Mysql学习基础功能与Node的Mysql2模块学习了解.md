---
title: Mysql学习基础功能与Node的Mysql2模块学习了解
categories: 技术笔记
tags:
  - Mysql
  - Node
excerpt: 对Mysql进行简单学习与了解用node中的Mysql2模块进行连接数据库学习用node操作mysql基础操作
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200929182403.png'
articleLink: e101ec34d58c3329
date: 2020-09-29 15:35:33
---

## Mysql

是一种关系型数据库管理系统 是按照数据结构来组织、存储和管理数据的仓库。

可以对数据进行增删改查 在我们开发当中用的最多的也就这四句了不知道说的对不对哈 ！ 

剩下对创建表等什么的操作都用可视化了的软件了 但是你得知道有这些语句

- 创建数据库 CREATE DATABASE 数据库名;  
- 显示数据库 SHOW DATABASES;
- 查看数据库信息 SHOW CREATE DATABASE 数据库名;
- 修改数据库编码格式 ALTER  DATABASE 数据库名 CHARACTER SET = utf8;
- 删除数据库  DROP DATABASE 数据库名;

接下来我们简单了解Mysql的增删改查操作

### 增

首先你要知道你往哪个表增加那些东西 

```
INSER INTO 表名 (字段一,字段二,字段三) VALUES ("值一","值二","值三");
```

### 删

最好加上条件不然你就会跑路。。。

```
DELETE FROM 表名 WHERE 条件;
```

### 改

设置条件确定一个东西然后将这个东西改为新的内容

```
UPDATE 表名  SET 设置的内容  WHERE 条件语句；
```

### 查

查就比较难了 以后你会正着查倒着查 从中间查 反正是各种查 各式各样的查 都是泪

```
SELECT 字段  FROM 表名  WHERE  条件语句；
```

### 条件语句

#### ADN

这是个条件链接符号

顾名思义 就是两个条件都要 什么和什么

如：我想找到这个班 姓李的同学的成绩大于60分的

那么我们分析这是两个 条件 你得姓李 你的成绩得大于60 

```
SELECT * FROM Class WHERE name LIKE "李%" AND achievement > 60;
```

#### OR

这是个条件链接符号

顾名思义 就是那个满足我就要那个 

如： 我想找到这个班 李明 或者 成绩大于60的

```
SELECT * FROM Class WHERE name = "李明" OR achievement > 60;
```

#### ORDER BY(DESC/ASC)

对查询的结果进行排序  ASC 或 DESC 关键字来设置查询结果是按升序或降序排列

典型案例如：班级的成绩 查询出来后我想按成绩从高到低进行显示（升序）

```
SELECT * from Class ORDER BY achievement ASC;
```

那我的需求改了 我就想看看 成绩最差的是谁（降序）

```
SELECT * from Class ORDER BY achievement DESC;
```

#### LIMIT

用于限制查询结果返回的数量，常用于分页查询;

```
# 查询10条数据，索引从0到9，第1条记录到第10条记录
select * from Class limit 10;
select * from Class limit 0,10;

# 查询8条数据，索引从5到12，第6条记录到第13条记录
select * from Class limit 5,8;
```

#### LIKE

可以说成是一个模糊查询

LIKE 子句中使用百分号 %字符来表示任意字符，类似于UNIX或正则表达式中的星号 *。

如果没有使用百分号 %, LIKE 子句与等号 = 的效果是一样的。

看看咱们班姓李的都是谁 

这里我用了% 那么就是我不管你李后面是什么我都要

```
SELECT * FROM Class WHERE name LIKE "李%";
```

## Mysql2模块

为什么Node.js已经有mysql模块,还出了mysql2呢?

为啥不用Mysql模块而是用Mysql2模块 简单的理解是mysql2对promise进行了封装 

### 安装

```
npm i mysql2
```

### 使用

我们要进行链接数据库只有链接上数据库才能进行更多的操作

**链接**

```
const connection = mysql.createConnection({
		//数据库地址
    host: '44.166.135.12',
    //数据库用户名
    user: 'roor',
    //数据库密码
    password: 'root',
    //数据库表
    database: 'root'
});
```

当我们链接后测试下

`query` 第一个写的是SQL语句然后跟着写上处理方法

```
connection.query('SELECT * FROM `user`', function (err, results, fields) {
		// 打印处理结果
    console.log(results);
});
```

如果我们打印出来东西那么就说明我们的代码没有问题

我们这里还可以使用占位符号

#### 占位符

也就是说我们的sql语句可能存在变量的问题 那么我们就可以使用 `?` 就代表占位下然后用 `[]` 代表变量

```
connection.query(
	//sql语句
	'SELECT * FROM `table` WHERE `name` = ? AND `age` > ?',
	//变量
  ['Page', 45],
  //回调
  function(err, results) {
    console.log(results);
  }
);
```

## Promise

MySQL2还支持Promise API 

 这个也新特性 相比 mysql 更好的地方 大家可以更加深度的去学习 这里只做了解 

```
async function main() {
  // get the client
  const mysql = require('mysql2/promise');
  // create the connection
  const connection = await mysql.createConnection({host:'localhost', user: 'root', database: 'test'});
  // query database
  const [rows, fields] = await connection.execute('SELECT * FROM `table` WHERE `name` = ? AND `age` > ?', ['Morty', 14]);
}
```

MySQL2使用`Promise`范围内可用的默认对象。但是您可以选择`Promise`要使用的实现

```
// get the client
const mysql = require('mysql2/promise');
 
// get the promise implementation, we will use bluebird
const bluebird = require('bluebird');
 
// create the connection, specify bluebird as Promise
const connection = await mysql.createConnection({host:'localhost', user: 'root', database: 'test', Promise: bluebird});
 
// query database
const [rows, fields] = await connection.execute('SELECT * FROM `table` WHERE `name` = ? AND `age` > ?', ['Morty', 14]);
```

MySQL2还在Pools上公开了一个.promise（）函数，因此您可以从同一池中创建一个Promise / non-Promise连接。

```
async function main() {
  // get the client
  const mysql = require('mysql2');
  // create the pool
  const pool = mysql.createPool({host:'localhost', user: 'root', database: 'test'});
  // now get a Promise wrapped instance of that pool
  const promisePool = pool.promise();
  // query database using promises
  const [rows,fields] = await promisePool.query("SELECT 1");
```

MySQL2在Connections上公开了一个.promise（）函数，以“升级”现有的非承诺连接以使用promise

```
// get the client
const mysql = require('mysql2');
// create the connection
const con = mysql.createConnection(
  {host:'localhost', user: 'root', database: 'test'}
);
con.promise().query("SELECT 1")
  .then( ([rows,fields]) => {
    console.log(rows);
  })
  .catch(console.log)
  .then( () => con.end());
```

