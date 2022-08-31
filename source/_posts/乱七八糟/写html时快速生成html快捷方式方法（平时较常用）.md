---
title: 写html时快速生成html快捷方式方法（平时较常用）
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 快捷方式
excerpt: tab键很强大，输入要输入的信息后，点击tab键，效果即现。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916140001.png'
articleLink: 7cf43c250aea1703
date: 2018-11-03 21:17:17
---

# 头部部分：（html5）

1、输入“！”，按tab键；  
2、输入“html:5”，按tab键；  
得到：

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

注意：编写中文网页，记得把头部语言<html lang="en">改为<html lang="zh-cn">。  
css样式引用：  
输入“link”，按tab键；  
得到：

```
<link rel="stylesheet" href="">
```

# 代码补全：

要输入一段代码段，用拼接形式。  
1、子集合 用“>”拼接，几个子集用 _星号 拼接，标签内文本用“{value}”拼接，有序数字用“$”拼接  
如：_ul>(li>a{vass$})\*4

得到：

```
<ul>
    <li><a href="">value1</a></li>
    <li><a href="">value2</a></li>
    <li><a href="">value3</a></li>
    <li><a href="">value4</a></li>
</ul>
```

2、同级元素用 “^” 拼接，可以用()把要优先级区别开  
如：div>(header>ul>li\*2>a)+footer>p  
得到：

```
<div>
    <header>
        <ul>
            <li><a href=""></a></li>
            <li><a href=""></a></li>
        </ul>
    </header>
    <footer>
        <p></p>
    </footer>
</div>
```

3、设置元素id，class，类似于css中id设置用#，class用. 。  
如：#header{头信息}  
得到：

```
<div id="header">头信息</div>
```

如：.title{标题}  
得到：

```
<div class="title">标题</div>
```

4、同时还可以给标签添加属性  
同时：ul>(li>a\[#\])\*3  
然后按Tap键，就可以快速撰写

```
<ul>
    <li><a href="#"></a></li>
    <li><a href="#"></a></li>
    <li><a href="#"></a></li>
</ul>
```

不同的属性： ul>(li>a\[click=”#”\])\*3  
然后按Tap键，就可以快速撰写

```
<ul>
    <li><a href="" click="#"></a></li>
    <li><a href="" click="#"></a></li>
    <li><a href="" click="#"></a></li>
</ul>
```

5、快速输入标签，输入标签名称，按下tab键  
如：div input label video audio 等  
得到：

```
<div></div>
<input type="text"> 
<label for=""></label>  
<video src=""></video> 
<audio src=""></audio>
```
