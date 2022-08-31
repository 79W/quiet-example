---
title: HTTP基础之缓存理解优缺点（三）
categories: 技术笔记
tags:
  - HTTP
  - 缓存
excerpt: HTTP基础之缓存理解优缺点
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922193901.png'
articleLink: 99463318f72c2917
date: 2019-08-17 15:36:29
---

## 私有缓存

私有缓存只能单独用户 

```
cache-control:private
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgIJtCWfrDhVsbLM4.png)

## 共享缓存

可以被多个用户使用

```
cache-control:public
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imghV6wklTSyYRKNgt.png)

## 缓存

将资源存在本地一份就不需要去服务器里面访问了 直接去本地访问就可以 那么就可以减少服务器的压力了 缓存也不是永久存在的可能会被清除或者过期

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgObcAJEqoV5hweaY.png)

### 缓存的优点

1. 缓解服务器端的资源消耗和运行压力，提升服务器端的整体性能。

2. 减少服务器端资源加载的延迟，进而减少显示某个资腺所用的时间。 
3. 减少对带宽造成的压力，避免网络阻塞问题的出现
4. Web站点变得更具有响应性

### 缓存应用

常见的HTTP缓存只能存储GET响应，对于其他类型的响应则无能为力。 普遍的缓存案例:

1. 检索请求的成功响应:响应状态码为200,则表示为成功。包含例如HTML文档，图片，或者文件 的响应。
2. 不变的重定向:响应状态码为301。
3.  错误响应:响应状态码为404的一个页面。
4. 不完全的响应:响应状态码为206,只返回局部的信息。
5. 除了 GET请求外，如果匹配到作为一个已被定义的cache键名的响应。

## cache-control 头

### 禁止缓存

```
Cache-Control:no-store
Cache-Control:no-store,no-cache,must-revalidate
```

### 强制确认缓存

```
Cache-Control:no-cache
```

### 缓存过期机制

```
Cache-Control:max-age = 3153600
```

### 缓存验证确认

```
Cache-Control:must-revalidate
```

### Pragema头

跟 `cache-control：nocache` 相同 兼用 http/1.0以后的客户端

```
Pragema:no-cache
```

### Expires 头

无效日期 为0 表示失效

如果设置了 cache-control “max-age”或者“s-max-age”那么expires 失效

```
Expires：wed 21 oct 2019 gmt
```

