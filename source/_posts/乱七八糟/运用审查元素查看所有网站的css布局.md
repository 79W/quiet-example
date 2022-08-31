---
title: 运用审查元素查看所有网站的css布局
categories: 乱七八糟
tags:
  - HTML
  - 个人笔记
  - css
  - css布局
  - 审查元素
excerpt: 我们首先打开一个你想查看的网站
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5beab0e0aed0f.png'
articleLink: 318caccf649c5313
date: 2018-11-13 19:19:53
---

1.我们首先打开一个你想查看的网站(这里拿”乔越博客”为例)

* * *

![运用审查元素查看所有网站的css布局](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5beab0e0aed0f.png "运用审查元素查看所有网站的css布局")

2.打开后 按 F12   并找打 Console  这个按钮  并 找到 闪动的  |  后复制代码 并回车（看图）

* * *



代码：

* * *

```
[]. forEach. call($$("*"), function(a){
a. style. boxShadow ="0 0 5px #"+(~~(Math. random()*(1<<24))). toString(16)
a. style . borderRadius = "5px" ;

   }
)
```

3.回车后的效果是这个样子的：

* * *

![运用审查元素查看所有网站的css布局](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5beab2e92458e.png "运用审查元素查看所有网站的css布局")