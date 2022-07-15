---
title: 'html选择器,css选择器,复杂,聚合,选择器,属性,派生,伪类,交集选择器'
categories: 技术笔记
tags:
  - HTML
  - 交集选择器
excerpt: 'html选择器,css选择器,复杂,聚合,选择器,属性,派生,伪类,交集选择器'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918123417.png'
articleLink: b3698d116d2b5812
date: 2019-04-12 17:19:58
---

## 1.聚合选择器：

同时对几个选择器进行相同的操作，好几个地方需要相同的设定，可以合并来写

```
p,h1,#psw,.p1{
属性：属性值；
}
列举：
<style>
      h1,p,.red{
          color:red;
      }
    </style>
</head>
<body>
        <h1 >聚合选择器</h1>    
        <p class="red">这是p1</p>
        <p class="red">这是p2</p>
        <p>这是p3</p>
</body>
```

## 2.属性选择器：

```
举例：
<a class="blank" href="#" target="_blank">这个链接的target是blank</a></br>
<a class="no blank" href="#" target="_self">这个链接的target是self</a></br>
<a href="#" >这个链接没写target</a>
①标签名[属性名]{}
a[href]{
color:red;
}
②标签名[属性名][属性名]{}
a[target][href]{
color:red;
}
③标签名[属性名=属性值]{}属性值必须完全符合才可以（绝对匹配）
a[target="_self"]{
color:red;
}
④标签名[属性名~=属性值]{}只要有相同的元素即可（模糊匹配）
a[class~="blank"]{
color:red;
}
```

## 3.派生选择器：

包括：后代选择器、子元素选择器、相邻兄弟选择器

①后代选择器（包含选择器）：

主要是用于对容器对象中的子对象进行控制，使其他容器对象的同名子对象不受影响
容器对象在前，子对象在后，中间为空格

```
<style>

p strong{
color:red;
}
</style>

<body>
<strong>我是加粗的字</strong>
<p>这是段落标签中的内容,<strong>我是红色的字</strong></p>
<p>这是段落标签中的内容,<span><strong>我也是红色的字</strong></span></p>
</body>
```

样式会应用到p元素的所有strong元素中，不管中间是否还有其他元素隔开

②子元素选择器：
如果不希望应用到所有该后代的相同元素上，那么需要用子元素选择器
如果是>,则只会应用到他的直系后代

```
<style>

p>strong{
color:red;
}
</style>
</head>
<body>
<strong>我是加粗的字</strong>
<p>这是段落标签中的内容,<strong>我是红色的字</strong></p>
<p>这是段落标签中的内容,<span><strong>我不再是红色的字</strong></span></p>

</body>
```

③相邻兄弟选择器
兄弟关系，和strong元素相邻的p元素发生样式变化

```
<style>

strong + p{
color:red;
}
</style>
```

## 4.伪类选择器 通常是给超链接的不同状态加样式 正常状态 link 访问过的状态：visited 放上状态：hover 激活状态：active 注意：需要严格按照以上顺序来写

```
<style>

a.baidu:link{color:#ff0000}
a.baidu:visited{color:#00ff00}
a.baidu:hover{color:#ff00ff}
a.baidu:active{color:#0000ff}
</style>
<b><a class="baidu" href="#" target="_blank">百度</a></b>
```

## 5.交集选择器

两个选择器共同的部分

```
<style>
p{color:red;}
.special{color:green;}
p.special{font-size:40px;}
</style>

<body>
<h1 class="special">这是一个标题</h1>
<p>这是一个段落</p>
<p class="special">这是一个交集选择器做出的效果</p>

</body>
```

作业：

①使用列表来做
②通过开发者工具查看css样式，并设置成相同的样式
③使用伪类选择器将所有的超链接设置成如下样式：
显示：color为#ff0000
访问后：color为#00ff00
鼠标放在上面的时候color为#ff00ff
鼠标单击时color为0000ff