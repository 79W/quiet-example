---
title: 'Java类的继承,封装,重写和重载的区别'
categories: 技术笔记
tags:
  - Java
  - 基础知识
  - 数组
excerpt: 'Java类的继承,封装,重写和重载的区别'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b8543db4229c.png'
articleLink: 6f5062d146854505
date: 2018-08-05 12:52:45
---

![Java类的继承,封装,重写和重载的区别](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b8543db4229c.png "Java类的继承,封装,重写和重载的区别")

![Java类的继承,封装,重写和重载的区别](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b8543f4b47e1.png "Java类的继承,封装,重写和重载的区别")

//—————————–掌握理解——————–

1. 类的继承：其关键字是extends, 是指Java中类与类之间的关系，类似于现实世界中的父子关系, 就是爸爸有什么儿子就有什么，

   1.1类的继承的特性：

   1）：父类有什么子类就有什么

   2）Java中类的继承是单继承的，简而言之就是一个儿子有一个亲爹

   3）一个父类可以被多个子类所继承，且他们相互之间是不干扰的

2.封装：是给Java语言的三特性之一，其实质就是将功能相近的代码利用重构的手段(shift+alt+m)写在一起，进而达到构成一个功能强大的类，目的是省去了繁琐臃肿重复的代码，精简代码且增强代码的可读性

//—————————–作业和补充—————-

- **super:**指向父类中的属性或方法
- 重写和重载的区别
  - **重写：即是**Override，其目的是为丰富父类中方法的内容
  - **重载：即是**Overload,其目的是扩展父类中的方法
- **this:**指向本类中的属性或方法