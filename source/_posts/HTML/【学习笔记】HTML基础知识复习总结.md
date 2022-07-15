---
title: 【学习笔记】HTML基础知识复习总结
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 基础知识
  - 学习笔记
excerpt: >-
  HTML概述 什么是html 超文本标记语言\_超文本标记语言 html为标记性语言
  HTML不是一门编程语言，而是一门标记语言，因为HTML是由多种的元素组成，这些元素可以包含文本，超链接等不同的内容。 html发展历史 自1993年之后
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgKC1fk8HFeD5tgiZ.png'
articleLink: b834b121c6c43102
date: 2020-01-02 19:33:31
---

## HTML概述

### 什么是html

**超文本标记语言** **超文本标记语言**

**html为标记性语言**

HTML不是一门编程语言，而是一门标记语言，因为HTML是由多种的**元素**组成，这些元素可以包含文本，超链接等不同的内容。

### html发展历史

自1993年之后HTML出现真正意义上的第一版，发展到至今，经历了5个大版本的更新和迭代。具体每个版本发布的时间如下：

*   超文本标记语言（第一版）-在1993年6月作为互联网工程工作小组（IETF）工作摘要发布（并非标准）
*   HTML 2.0-1995年11月作为RFC 1866发布，在RFC 2854于2000年6月发布之后被宣布已经过时
*   HTML 3.2-1997年1月14日，W3C推荐标准
*   HTML 4.0-1997年12月18日，W3C推荐标准
*   HTML 4.01（微小改进）-1999年12月24日，W3C推荐标准
*   HTML 5-第一份正式正式已于2008年1月22日公布，正式版本发布于2014年10月29日

### html基本结构

首先创造的是后缀法定`.html` `.htm`

```html
<!-- 声明为html 告诉浏览器这个html-->
<!DOCTYPE html>
<!-- 
html 根元素
head 头部
body 主体内容
-->
<html lang="en">

<head>
  <!-- 编码 -->
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <!-- 网站标题 -->
  <title>Document</title>
</head>
<!-- 内容 -->
<body>
  <!-- 开始标签 内容 结束标签 -->
  <h1>内容区</h1>
</body>

</html>
```

![【学习笔记】HTML基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgKC1fk8HFeD5tgiZ.png "【学习笔记】HTML基础知识复习总结")

#### html两种形态

##### 封闭状态

```html
<div>文本内容</div>
```

##### 空元素

```html
<input type="text">
```

#### html标签

![【学习笔记】HTML基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgWboFCDgXTm5lfPH.png "【学习笔记】HTML基础知识复习总结")

![【学习笔记】HTML基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgFxhXpO4zmGNAYoL.png "【学习笔记】HTML基础知识复习总结")

### html元素属性

![【学习笔记】HTML基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgemCnoGtwqBLJZVp.png "【学习笔记】HTML基础知识复习总结")

*   属性名（属性名称）：其数量和作用都是HTML给定的。
*   属性值（attribute value）：属性对应的值，建议使用一对双引号进行包裹。

HTML元素的属性可以划分为以下4种：

1.  标准（通用）属性：HTML元素几乎都具有的属性，例如id，name，style和class属性等。
2.  专有（私有）属性：HTML中某些元素特有的属性，例如“元素的动作属性等。
3.  事件属性：用来为HTML元素注册DOM事件的属性，例如onclick属性等。
4.  自定义属性：第三方框架中为了完成某个特定功能而定义的属性，例如Vue框架的`v-if`属性等。

### html头部

#### 头元素

##### 标题元素

```html
<!--网站标题-->
<title>Document</title>
```

##### 基本元素

```html
<base target="_blank" href="http://www.example.com/">
```

##### 链接元素

```html
<!--引入外部元素-->
<link href="link-element-example.css" rel="stylesheet">
```

##### 风格元素

```html
<!--内嵌样式表-->
<style type="text/css">
    body {
      color:red;
    }
</style>
```

##### 元元素

```html
<!--编码-->
<meta charset="UTF-8">
```

##### 脚本元素

```html
<!--js-->
<script type="text/javascript">
    console.log('打印一个测试信息.');
</script>
```

##### noscript元素

```html
<!--浏览器关闭的时候执行的-->
<noscript>
    <a href="http://www.example.com/">这是一个链接</a>
</noscript>
```

#### 肉元素

meat元素的常用用法如下所示：

为搜索引擎定义关键词：

```html
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
```

为网页定义描述内容：

```html
<meta name="description" content="Free Web tutorials on HTML and CSS">
```

定义网页作者：

```html
<meta name="author" content="KingJ">
```

每30秒中刷新当前页：

```html
<meta http-equiv="refresh" content="30">
```

HTML5版本定义编码格式：

```html
<meta charset="UTF-8">
```

定义HTML页面的视口：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

### html注释

![【学习笔记】HTML基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgBl4IdLEOpGT1XRj.png "【学习笔记】HTML基础知识复习总结")