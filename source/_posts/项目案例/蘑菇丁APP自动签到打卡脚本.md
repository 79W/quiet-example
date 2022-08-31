---
title: 蘑菇丁APP自动签到打卡脚本
categories: 项目案例
tags:
  - 蘑菇丁
  - 自动签到
  - node
excerpt: 蘑菇丁APP自动签到打卡脚本
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201030154048.png'
articleLink: c62b0402125f3630
date: 2020-10-30 14:32:36
---

## 蘑菇丁

我们每天要在 蘑菇丁APP上进行打卡这关系到我们是否能正常毕业

但是我们会忘记打卡 并且感觉这很麻烦 所以就自己写了写这个程序

## 基本

因本人的需要所以 在自己的服务器上面也进行了运行

可以给大家提供使用 因服务器不是很好 量大的话会 不稳定所以 要多看是否打卡

还有就是我随时会下架 （如服务到期获取运行负载等因素我会停止提供）

如果你想先试试 那么请来：http://moguding.79bk.cn/

## 服务API

#### 登陆本服务

本程序运行好了提供了一个API 就是添加用户的API

```
POST /addUser
```

发送数据

```
{
  username:"手机号",
  password:"密码"
}
```

#### 获取密码错误的用户

```
GET /getwrong
```

返回数据

```
[
  {
    "username":"乔越博客",
    "studentNumber":883888232
  }
]
```

## 蘑菇丁API

此接口全部是对接 蘑菇丁APP的 所以下面我就不演示 蘑菇丁丁网址了 （源代码里面进行）

#### 登陆接口 

```
POST /session/user/v1/login
```

我们需要发送 账号和密码然后就会给我们返回相关数据

#### 签到

```
POST /attendence/clock/v1/save
```

我们需要传递的信息 `planId` 这个很重要

```
{
    "device":"iOS",
    "planId":"",
    "country":"中国",
    "state":"NORMAL",
    "attendanceType":"",
    "address":"地址",
    "type":"",
    "longitude":"经纬度",
    "city":"城市",
    "province":"省份",
    "latitude":"经纬度"
}
```

type 参数是类型 值为`START` 表示打上班卡 `END` 表示打下班卡

我们还需要在 请求头中添加上 我们登陆获取 `token` 

```
Authorization: 通过登陆获取
```

#### 获取事件id

`planId` 这是我们进行签到时候进行传递的 如果没有这个 签到也会成功但是 APP 上会不显示

```
POST /practice/plan/v1/getPlanByStu
```

传递的参数 为学生 （这里可能会变）

```
{
  "paramsType": "student"
}
```

我们还需要在 请求头中添加上 我们登陆获取 `token` 

```
Authorization: 通过登陆获取
```

## 数据库

你只需把本项目中`user.sql  ` 用mysql导入即可

然后在 app.js 中连接上你的数据即可

## 前端页面

前端页面我也进行了简单的注册 凑合看吧

![moguding](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201030145521906.png)

## 资源下载

会去github当中去下载 点击下面链接即可前往

[前往下载](https://github.com/QiaoBug/AutoMoGuDing)

## 版权信息

如果你要使用的话请给我个版权信息 

```
name : 乔越博客
link : https://www.79bk.cn/
```

[乔越博客](https://www.79bk.cn/)

