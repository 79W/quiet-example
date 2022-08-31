---
title: CSS优先级排序顺序和提升优先级方法!important
categories: 技术笔记
tags:
  - HTML
  - 优先级顺序
  - 提升优先级
excerpt: >-
  优先级排序
  浏览器根据优先级来决定给元素应用哪个样式，而优先级仅由选择器的匹配规则来决定。内联》ID选择器》伪类=属性选择器=类选择器》元素选择器【p】》通用选择器(*)》继承的样式
  权重：内
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2b98c53726.jpg'
articleLink: d9df652225b25019
date: 2019-04-19 15:09:50
---

## 1.优先级排序

浏览器根据优先级来决定给元素应用哪个样式，而优先级仅由选择器的匹配规则来决定。

**内联》ID选择器》伪类=属性选择器=类选择器》元素选择器【p】》通用选择器(\*)》继承的样式**

**权重：内联样式1000》id选择器100》class选择器10》标签选择器1**



## 2.!important

**一、语法**
选择器{样式:值!important

;}
**二、说明**
提升指定样式规则的应用优先权，即!important为开发者提供了一个增加样式权重的方法，让浏览器首选执行这个语句。

![CSS优先级排序顺序和提升优先级方法!important](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2b98c53726.jpg)

就是说他们会默认让color：red!important; 这条语句生效，下面的不带!important声明的样式将不会覆盖它，换句话说就是他的级别最高，下面的人都不能取代我！

**注意：**
1、IE6及更早浏览器下，!important在同一条规则集内不生效。
2、如果!important被用于一个简写的样式属性，那么这条简写的样式属性所代表的子属性都会被作用上!important。
3、关键字!important必须放在一行样式的末尾并且要放在该行分号前，否则就没有效果。



演示代码：

```
p{

color: red!important;

}

#only{

color: yellow;

}

span{

color: rgb(117, 255, 4);

}

.abc{

color: pink;

}
```

```
<body>

<p>

css样式特点

<spanclass="abc"id="only"style="clor:blue">

优先级问题

</span>

</p>

</body>

 
```

