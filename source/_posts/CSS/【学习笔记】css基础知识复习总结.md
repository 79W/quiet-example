---
title: 【学习笔记】css基础知识复习总结
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - css
  - 基础知识
  - 学习笔记
excerpt: css概述 什么是css 级联样式表\_初始样式表 CSS是用来定义HTML元素显示的样式和布局方式的，例如设置显示字体的颜色，大小等效果
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgBfC34qrJlANzQhn.png'
articleLink: b59ea22f372d4702
date: 2020-01-02 19:36:47
---

## css概述

### 什么是css

**级联样式表** **初始**样式表

CSS是用来定义HTML元素显示的样式和布局方式的，例如设置显示字体的颜色，大小等效果。

```css
p{
  color:red;
  font-size:24px;
}
```

### css发展历程

从1994年开始，直到1996年才出版了第一版CSS要求。CSS发展至今，也仅经历了3个大版本的更新和互换。具体每个版本发布的时间如下：

*   CSS1-作为一项W3C推荐，CSS1发布于1996年12月17日。1999年1月11日，此推荐被重新修订。
*   CSS2-作为一项W3C推荐，CSS2发布于1999年1月11日。CSS2添加了对介质（打印机和听觉设备）和可下载字体的支持。
*   CSS3-CSS3计划将CSS划分为更小的模块。于1999年开始制定，2001年5月23日W3C完成了CSS3的工作计划。

### 内联样式

```html
<div style="color: lightcoral;">这是测试内容.</div>
```

![【学习笔记】css基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdmN6LfC5ueTJgV7.png "【学习笔记】css基础知识复习总结")

### 内嵌样式表

```css
<style type="text/css">
    p {
        color: lightcoral;
        font-size: 24px;
    }
</style>
```

两个问题：

*   HTML内容与CSS样式的强补充问题，合并网页的内容和样式有效地分离。
*   如果为不同元素设置相同的CSS样式的话，只需要定义一次CSS样式代码。

### 外联样式表

```css
<link rel="stylesheet" href="style/demo.css">
```

加载过程

1.  浏览器会加载HTML页面并进行解析。
2.  解析到“元素约会的CSS文件，并通过href属性读取到CSS文件的路径。
3.  根据读取的路径，开始加载CSS文件并进行解析。

![【学习笔记】css基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgBfC34qrJlANzQhn.png "【学习笔记】css基础知识复习总结")

### css语法结构

![【学习笔记】css基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgN3c1GmEsbLfaZTj.png "【学习笔记】css基础知识复习总结")

*   选择器（Selector）：使用定位当前HTML页面中的元素，可以是一个元素也可以是多个元素（元素集）。
*   声明块（Declaration block）：包含一个或多个CSS声明，其语法结构是一对花括号。

### css注释

![【学习笔记】css基础知识复习总结](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imglLcmyeM5AjkFgNp.png "【学习笔记】css基础知识复习总结")