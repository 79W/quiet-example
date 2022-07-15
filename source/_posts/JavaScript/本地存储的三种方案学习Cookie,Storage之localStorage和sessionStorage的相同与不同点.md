---
title: '本地存储的三种方案学习Cookie,Storage之localStorage和sessionStorage的相同与不同点'
categories: 技术笔记
tags:
  - Cookie
  - Storage
  - localStorage
  - sessionStorage
  - 本地存储
  - 本地缓存
excerpt: 学习本地存储的三种方式对Koa-Cookie的学习和Storage-Api的学习和使用方面的了解和各种小细节的学习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgcookies2.png'
articleLink: c5cf45d9ebfe3304
date: 2020-10-04 15:49:33
---

## Cookie

cookie是http协议下，服务端或者脚本可以维护客户端信息的一种方式。

Cookie是一段不超过4KB的小型文本数据，由一个名称（Name）、一个值（Value）和其它几个用于控制Cookie有效期、安全性、使用范围的可选属性组成。

### koa中cookie的使用

**储存cookie的值**

```
ctx.cookies.set(name, value, [options])
```

**获取cookie的值**

```
ctx.cookies.get(name, [options])
```

**options常用设置**

- `maxAge` 一个数字表示从 Date.now() 得到的毫秒数
- `expires` cookie 过期的 `Date`
- `path` cookie 路径, 默认是`'/'`
- `domain` cookie 域名
- `secure` 安全 cookie  设置后只能通过https来传递cookie
- `httpOnly` 服务器可访问 cookie, 默认是 **true**
- `overwrite` 一个布尔值，表示是否覆盖以前设置的同名的 cookie (默认是 **false**). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie

### 客户端Cookie

**设置Cookie**

```
document.cookie="key=value"
```

- key和value是包含在一个字符串中

  - key包含字段
    - [name] 这个name为自己取的cookie名称，同名的值会覆盖
    - domain 所属域名
    - path 所属路径
    - Expires/Max-Age 到期时间/持续时间 (单位是秒)
    - http-only 是否只作为http时使用，如果为true，那么客户端能够在http请求和响应中进行传输，但时客户端浏览器不能使用js去读取或修改
- 多个key=value使用 ; （分号）分隔

**获取Cookie**

```
document.cookie
```

返回值是当前域名下的所有cookie，并按照某种格式组织的字符串 ：

key=value; key1=value1; ......keyn=valuen

#### 封装Cookie

只是在原方法的基础上进行一些简单功能性的封装 将cookie更加的简单化 

**设置Cookie**

可以直接进行调用函数来设置 cookie

```
//设置cookie方法
function setCookie(name,value,options={}){
    let cookieData = `${name}=${value};`;
    for(let key in options){
        let str = `${key}=${options[key]};`;
        cookieData += str;
    }
    document.cookie = cookieData;
}
```

**获取Cookie**

我们有时候需要获取指定名称的cookie值就可以用这个直接进行获取 会更加的方便

```
//获取cookie方法；
function getCookie(name){
    let arr = document.cookie.split("; ");
    for(let i =0;i<arr.length;i++){
        let arr2 = arr[i].split("=");
        if(arr2[0]==name){
            return arr2[1];
        }
    }
    return "";
}
```

## 本地缓存Storage

本地缓存有两种 分别为 `localStorage` 和 `sessionStorage` 但是两种的api是完全相同的 而且用起来也很简单 下面分别进行演示

### localStorage

#### 设置

这里需要传递两个参数 

- 第一个是要创建或更新的键名 
- 第二个是 要创建或更新的键名对应的值

```
 localStorage.setItem("test","测试文字");
```

#### 读取

参数需要传递一个键名

返回 键名对应的值。

如果键名不存在于存储中，则返回 `null`。

```
let value = localStorage.getItem("test")
```

#### 清除

传递你想要移除的键名

```
 localStorage.removeItem("test");
```

#### 清除所有

调用它可以清空存储对象里所有的键值。

```
localStorage.clear();
```

#### 获取指定索引

接受一个数值 n 作为参数，返回存储对象第 n 个数据项的键名

键的存储顺序是由用户代理定义的

**所以尽可能不要依赖这个方法**

```
var aKeyName = storage.key(key);
```

**实例**

```
function populateStorage() {
  localStorage.setItem('bgcolor', 'yellow');
  localStorage.setItem('font', 'Helvetica');
  localStorage.setItem('image', 'cats.png');

  localStorage.key(2); // 应该返回 'image'
}
```

### sessionStorage

api方法与 `localStorage` 完全一样 只需要将 `localStorage`换成 `sessionStorage`

如`localStorage`的设置api

```
localStorage.setItem("test","测试文字");
```

那么`sessionStorage` 的设置api 就是

```
sessionStorage.setItem("test","测试文字");
```

## 本地存储异同

### 共同点

#### localStorage和sessionStorage和cookie共同点

- 同域（同源策略）限制：同源策略：请求与响应的 协议、域名、端口都相同 则时同源，否则为 跨源/跨域
- 存储的内容都会转为字符串格式
- 都有存储大小限制

#### localStorage和sessionStorage共同点

- API相同
- 存储大小限制一样基本类似
- 无个数限制

### 不同点

**localStorage**

- 没有有效期，除非删除，否则一直存在
- 同域下页面共享
- 支持 storage 事件（可以监听这个事件看是否发生了改变）

**sessionStorage**

- 浏览器关闭，自动销毁
- 页面私有
- 不支持 storage 事件

**cookie**

- 浏览器也会在每次请求的时候主动组织所有域下的cookie到请求头 cookie 中，发送给服务器端
- 浏览器会主动存储接收到的 set-cookie 头信息的值
- 可以设置 http-only 属性为 true 来禁止客户端代码（js）修改该值
- 可以设置有效期 (默认浏览器关闭自动销毁)(不同浏览器有所不同)
- 同域下个数有限制，最好不要超过50个(不同浏览器有所不同)
- 单个cookie内容大小有限制，最好不要超过4000字节(不同浏览器有所不同)





[资源下载](https://wwa.lanzous.com/ioS8uh5z4hi)

