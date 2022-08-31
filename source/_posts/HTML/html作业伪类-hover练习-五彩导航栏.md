---
title: 'html作业伪类:hover练习五彩导航栏'
categories: 技术笔记
tags:
  - HTML
  - 伪类
excerpt: 利用伪类hover实现以下效果把鼠标放在按钮a标签上实现替换图片和字体改色的
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918124151.png'
articleLink: 840d7ea169ee0913
date: 2019-04-13 13:29:09
---

题目：

利用伪类：hover 实现以下效果  把鼠标放在按钮a标签上实现替换图片和字体改色的操作

演示图

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918123646.png)

要先利用左浮动 把4张图片进行横排 排好 

然后再利用css hover伪类进行更换

看代码：

```
<divid="zong">
<divid="an1">
<p><aclass="an11"href="">五彩导航1</a></p>
</div>
<divid="an2">
<p><aclass="an12"href="">五彩导航2</a></p>
</div>
<divid="an3">
<p><aclass="an13"href="">五彩导航3</a></p>
</div>
<divid="an4">
<p><aclass="an14"href="">五彩导航4</a></p>
</div>
</div>
```

```
<style type="text/css">
#zong{
/*设置总外大div 长和宽*/
width: 600px;
height: 58px;
}
.an11,.an12,.an13,.an14{
/*同类代码 长宽 左浮动 边距10*/
width: 120px;
height: 58px;
float: left;
margin: 10px;
/*字居中*/
text-align: center;
color: rgb(255, 255, 255);
/*去除a标签的下划线*/
text-decoration:none;
/*设置行高*/
line-height:50px;
}
/*设置每个按钮的图片！*/
.an11{background:url('img/bg1.png') no-repeat;}
.an12{background:url('img/bg2.png') no-repeat;}
.an13{background:url('img/bg3.png') no-repeat;}
.an14{background:url('img/bg4.png') no-repeat;}
/*设置每个按钮的鼠标停放替换的图片 // 设置变化字的颜色*/
.an11:hover{background: url(img/bg5.png) no-repeat;color: rgb(231, 235, 0);}
.an12:hover{background: url(img/bg7.png) no-repeat;color: rgb(0, 162, 255);}
.an13:hover{background: url(img/bg8.png) no-repeat;color: rgb(76, 0, 255);}
.an14:hover{background: url(img/bg6.png) no-repeat;color: rgb(255, 0, 170);}
</style>
```

