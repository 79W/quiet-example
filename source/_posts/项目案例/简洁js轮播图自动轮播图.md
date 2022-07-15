---
title: 简洁js轮播图自动轮播图
categories: 项目案例
tags:
  - HTML
  - 个人笔记
  - 资源分享
  - 简洁js
  - 自动轮播图
  - 轮播图
excerpt: 简易轮播图
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimg5c3888e377108.gif'
articleLink: 46b15bbde5b42411
date: 2019-01-11 20:17:24
---

这是一个悲痛的日子  
![简洁js轮播图自动轮播图](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimg5c3888e377108.gif "简洁js轮播图自动轮播图")

**直接上代码：**

* * *

```
<div id="container">
        <div onclick="tutu1()" id="list" style="left: 0px;">
            <img onclick="tu1()" src="img/1 (1).jpg" alt="1" />
            <img onclick="tu()" src="img/1 (2).jpg" alt="2" />
            <img onclick="tu()" src="img/1 (3).jpg" alt="3" />
            <img onclick="tu()" src="img/1 (4).jpg" alt="4" />
            <img onclick="tu()" src="img/1 (5).jpg" alt="5" />
            
        </div>
        
    </div>
* {
    margin: 0;
    padding: 0;
    text-decoration: none;
}

body {
    padding-top: 200px;
    padding-left: 600px;
    
}

#container {
    position: relative;
    width: 600px;
    height: 400px;
    border: 1px solid #333;
    overflow: hidden;
}

#list {
    position: absolute;
    z-index: 1;
    width: 4200px;
    height: 400px;
}

img {
    float: left;
    
}

#container:hover .arrow {
    display: block;
}
#dadiv{
    width: 1200px;
    height: 800px;
    display:"none";
}
#dadiv img{
    display:"none";
    z-index: 9999;
    width: 1200px;
    height: 800px;
}
//自动轮播 每1秒轮播一次
var kaishi = document.getElementById("list");

    function lun(){ 
        if(list.style.left == "-2400px"){
            yuan = 0;
        }else{
            yuan = parseInt(list.style.left)-600;
        }
        list.style.left = yuan + "px";
        } 
        var t2 = window.setInterval("lun()",3000); 
```

 