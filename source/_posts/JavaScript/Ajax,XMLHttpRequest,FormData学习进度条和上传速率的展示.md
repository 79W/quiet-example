---
title: 'Ajax,XMLHttpRequest,FormData学习进度条和上传速率的展示'
categories: 技术笔记
tags:
  - Ajax
  - XMLHttpRequest
  - FormData
excerpt: 学习XMLHttpRequest的基础用法利用FormData完成文件的上传和使用upload事件完成上传进度和速率的展示
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imghttp-requests-xhr.png'
articleLink: e4db1a331e8b3305
date: 2020-10-05 14:35:33
---

## XMLHttpRequest

对象用于与服务器交互通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。

Ajax 即“`Asynchronous Javascript And XML`”（异步 JavaScript 和 XML）

### Ajax的基本使用

- 新建XMLHttpRequest对象

  ```
  let xhr = new XMLHttpRequest();
  ```

- 配置请求参数

  ```
  xhr.open("get","/checkUser",true); //true是异步，false是同步
  ```

- 接收返还值

  ```
  xhr.onload = function(){
      let res = JSON.parse(xhr.responseText);
   }
  ```

- 发送服务器

  ```
  xhr.send();
  ```

### GET请求

有两种传递参数的方法

**第一种**

在请求地址后拼接上参数如：

```
xhr.open("get", "/checkUserName?username='JoeBlog'", true);
```

上面是ES6的写法 用拼接变量的方法

```
xhr.open("get", `/checkUserName?username=${this.value}`, true);
```

**第二种**

在请求地址后端用斜杠进行分割 这种需要配合后端进行接收参数

```
xhr.open("get","/get/4",true);
```

Node 后端路由

```
router.get("/get/:id", (ctx, next) => {
    console.log(ctx.params);
    ctx.body = {
        status: 1,
        info: "请求成功"
    }
})
```

如果没有这个路由进行配置会进行报错

而且在前端请求的时候必须加上参数 如果没有也会报错而第一种不拼接上参数也是可以请求的

### POST请求

```
xhr.open("post", "/post", true);
```

但是POST传递参数的方式就有点特别了

发送数据时候需要设置http正文头格式

```
xhr.setRequestHeader()
```

里面写上头信息 

```
xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
```

那么我们传递数据就应该是下面这个样子然后我们在send过去

```
let data = "username=王五&age=20";
```

发送到服务器

```
xhr.send(data);
```

如果我们的头信息写的是 Json 格式

```
xhr.setRequestHeader("content-type","application/json");
```

那么我们就可以写json数据

```
let data = JSON.stringify({
    username:"王五",
    age:20
})
//发送数据
xhr.send(data);
```

### setRequestHeader()

是设置HTTP请求头部的方法。此方法必须在  `open()` 方法和 `send()`之间调用。

如果多次对同一个请求头赋值，只会生成一个合并了多个值的请求头。

```
xhr.setRequestHeader(header, value);
```

### getAllResponseHeaders() 

返回所有的响应头，以 CRLF分割的字符串，或者 `null` 如果没有收到任何响应。

```
var headers = xhr.getAllResponseHeaders();
```

返回结果就是 相应头里面的内容

```
date: Fri, 08 Dec 2017 21:04:30 GMT\r\n
content-encoding: gzip\r\n
x-content-type-options: nosniff\r\n
server: meinheld/0.6.1\r\n
x-frame-options: DENY\r\n
content-type: text/html; charset=utf-8\r\n
connection: keep-alive\r\n
strict-transport-security: max-age=63072000\r\n
vary: Cookie, Accept-Encoding\r\n
content-length: 6502\r\n
x-xss-protection: 1; mode=block\r\n
```

编码格式 实例

```
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");  //默认编码
xhr.setRequestHeader("Content-type","multipart/form-data");  //二进制编码
xhr.setRequestHeader("Content-type","application/json");  //json编码
```

### getResponseHeader()

返回包含指定响应头文本的字符串

```
var myHeader = xhr.getResponseHeader(name);
```

实例

```
var myHeader = xhr.getResponseHeader("content-type")
```

### onreadystatechange

只要 `readyState` 属性发生变化，onreadystatechange 函数就会被执行。

readyState：存有服务器响应的状态信息

- 0: 请求未初始化（代理被创建，但尚未调用 open() 方法）
- 1: 服务器连接已建立（`open`方法已经被调用）
- 2: 请求已接收（`send`方法已经被调用，并且头部和状态已经可获得）
- 3: 请求处理中（下载中，`responseText` 属性已经包含部分数据）
- 4: 请求已完成，且响应已就绪（下载操作已完成）

status常用状态码

| HTTP状态码 | 描述                                                   |
| ---------- | ------------------------------------------------------ |
| 100        | 继续。继续响应剩余部分，进行提交请求                   |
| 200        | 成功                                                   |
| 301        | 永久移动。请求资源永久移动到新位置                     |
| 302        | 临时移动。请求资源零时移动到新位置                     |
| 304        | 未修改。请求资源对比上次未被修改，响应中不包含资源内容 |
| 401        | 未授权，需要身份验证                                   |
| 403        | 禁止。请求被拒绝                                       |
| 404        | 未找到，服务器未找到需要资源                           |
| 500        | 服务器内部错误。服务器遇到错误，无法完成请求           |
| 503        | 服务器不可用。临时服务过载，无法处理请求               |

### 返回数据

可以返回 json 和xml数据

- 服务器返还json数据

  ```js
  xhr.responseText  //来获取
  ```

- 服务器返还xml数据

  ```js
  xhr.responseXML //获取值
  ```


而当我们返回的是 XML格式的 就需要在 服务端进行定义

Node 服务端

```
router.get("/xml", (ctx, next) => {
    ctx.set("content-type","text/xml");
    ctx.body = `<?xml version='1.0' encoding='utf-8' ?>
                    <books>
                        <nodejs>
                            <name>nodejs实战</name>
                            <price>56元</price>
                        </nodejs>
                        <react>
                            <name>react入门</name>
                            <price>50元</price>
                        </react>
                    </books>
                `
})
```

我们需要设置 `content-type` 为 `text/xml`

如果后端没有进行设置 我们前端又知道传递的是 xml 那么我们前端也是可以重写response里的content-type内容

```
// 重写content-type；
xhr.overrideMimeType("text/xml");
```

## 同步及异步Ajax

我们将 `open` 设置为 `false` 这时候就为同步的Ajax了

我们将网速设置为 3G 在我们的浏览器控制台中找到 `Online`  然后选择 `slow 3G` 这时候的网速就被限制了 

![Online](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201005153541157.png)

我们运行下面的代码 然后在网速慢的时候 进行快速点击 按钮一个和二 观看 控制台打印的内容 会有很明显的差异

如果是同步的 我们不管点多少次 按钮二 都需要等待按钮一 执行完成才能

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>
        <button>按钮一</button>
        <button>按钮二</button>
    </body>
    <script>
        let btns = document.querySelectorAll("button");
        btns[0].onclick = function () {
            let xhr = new XMLHttpRequest();
            xhr.open("get", "/get/4", false);
            xhr.onload = function () {
                console.log(xhr.responseText);
            }
            xhr.send();
        }
        btns[1].onclick = function () {
            console.log("我是按钮二");
        }
    </script>
</html>
```

## FormData

提供了一种表示表单数据的键值对 `key/value` 的构造方式

可以轻松的将数据通过`XMLHttpRequest.send()`方法发送出去

#### 创建对象

```
let form = new FormData();
```

#### append()

添加一个新值到 `FormData` 对象内的一个已存在的键中，如果键不存在则会添加该键。

```
form.append("name","张三");
```

#### delete()

会从 `FormData` 对象中删除指定键，即 key，和它对应的值，即 value。

```
form.delete(name);
// name 要删除的键（Key）的名字。
```

#### get()

用于返回FormData对象中和指定的键关联的第一个值

```
form.get(name)
// name 要获取的键（Key）的名字。
```

示例

```
//创建对象
var formData = new FormData();
//添加数据
formData.append('username', 'Chris');
formData.append('username', 'Bob');
//获取一个
formData.get('username'); // Returns "Chris"
```

#### getAll()

会返回该 `FormData`对象指定 key 的所有值。

```
form.getAll(name);
// name表示要检索的 key 名称。
```

示例

```
//下列代码会先创建一个空的 FormData 对象：
var formData = new FormData();
//使用 FormData.append 添加两个 username 的值：
formData.append('username', 'Chris');
formData.append('username', 'Bob');
//下列 getAll() 方法会返回一个数组，包含了所有 username 的值：
formData.getAll('username'); // Returns ["Chris", "Bob"]
```

#### has()

查询 `FormData`  里面是否包含某个key 

```
form.has(name);
// name 要查询的 key 名称。
```

示例

```
var formData = new FormData();

formData.has('username'); // Returns false
formData.append('username', 'Chris');
formData.has('username'); // Returns true
```

#### keys()

返回一个迭代器（`iterator`），遍历了该 formData  包含的所有key

示例

```
// 先创建一个 FormData 对象
var formData = new FormData();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

// 输出所有的 key
for (var key of formData.keys()) {
   console.log(key); 
}
```

结果

```
key1
key2
```

#### values()

返回一个允许遍历该对象中所有值的 迭代器 

```
form.values();
```

示例

```
//创建一个FormData测试对象
var formData = new FormData();
formData.append('key1', 'value1');
formData.append('key2', 'value2');

//显示值
for (var value of formData.values()) {
   console.log(value); 
}
```

结果

```
value1
value2
```

## upload事件

**返回一个** `XMLHttpRequestUpload`对象，用来表示上传的进度。

可以被绑定在upload对象上的事件监听器如下：

| 事件        | 相应属性的信息类型               |
| ----------- | -------------------------------- |
| onloadstart | 获取开始                         |
| onprogress  | 数据传输进行中                   |
| onabort     | 获取操作终止                     |
| onerror     | 获取失败                         |
| onload      | 获取成功                         |
| ontimeout   | 获取操作在用户规定的时间内未完成 |
| onloadend   | 获取完成（不论成功与否）         |

```
xhr.upload.onloadstart = function(){
		console.log("开始上传");
}
```

```
xhr.upload.onprogress = function(evt){
		console.log("正在上传");
}
```

```
xhr.upload.onload = function(){
		console.log("上传成功");
}
```

```
xhr.upload.onloadend = function(){
		console.log("上传完成");
}
```

```
xhr.upload.onabort = function(){
		console.log("取消上传");
}
```

而在我们的 `onprogress` 数据传输中 有两个数据可以获取

- evt.total ：需要传输的总大小
- evt.loaded :当前上传的文件大小

这样我们可以计算出来当前上传的百分比

```
let percent =  (evt.loaded/evt.total*100).toFixed(0);
```

我们也可以计算出来当时上传的速率

我们需要计算出来时间差和在这段时间内上传文件的大小然后进行计算