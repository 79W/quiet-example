---
title: 'Java封装的理解和用法,get,set方法权限修饰符'
categories: 技术笔记
tags:
  - Java
  - 权限修饰符
excerpt: >-
  private：Java语言中对访问权限限制的最窄的修饰符，一般称之为“私有的”。被其修饰的属性以及方法只能被该类的对象访问，其子类不能访问，更不能允许跨包访问。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgquanxian82555.png'
articleLink: 60b24eb5bb5e3515
date: 2019-05-15 18:44:35
---

## 权限修饰符概念：

1.private：Java语言中对访问权限限制的最窄的修饰符，一般称之为“私有的”。被其修饰的属性以及方法只能被该类的对象访问，其子类不能访问，更不能允许跨包访 问。  
2.default：即不加任何访问修饰符，通常称为“默认访问权限“或者“包访问权限”。该模式下，只允许在同一个包中进行访问。  
3.protected：介于public 和 private 之间的一种访问修饰符，一般称之为“保护访问权限”。被其修饰的属性以及方法只能被类本身的方法及子类访问，即使子类在不同 的包中也可以访问。  
4.public： Java语言中访问限制最宽的修饰符，一般称之为“公共的”。被其修饰的类、属性以及方法不仅可以跨类访问，而且允许跨包访问。

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgquanxian82555.png "Java封装的理解和用法,get,set方法权限修饰符")

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjbguifan5181702.png "Java封装的理解和用法,get,set方法权限修饰符")

在面向对象程式设计方法中，封装（英语：Encapsulation）是指一种将抽象性函式接口的实现细节部份包装、隐藏起来的方法。

封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。

要访问该类的代码和数据，必须通过严格的接口控制。

封装最主要的功能在于我们能修改自己的实现代码，而不用修改那些调用我们代码的程序片段。

适当的封装可以让程式码更容易理解与维护，也加强了程式码的安全性。

![Java封装的理解和用法,get,set方法权限修饰符](https://www.79bk.cn/wp-content/uploads/2019/05/fengzhuang515.png "Java封装的理解和用法,get,set方法权限修饰符")

### 封装的好处：

*   1\. 良好的封装能够减少耦合。
*   2\. 类内部的结构可以自由修改。
*   3\. 可以对成员变量进行更精确的控制。
*   4\. 隐藏信息，实现细节。

![Java封装的理解和用法,get,set方法权限修饰符](https://www.79bk.cn/wp-content/uploads/2019/05/haochu515182107.png "Java封装的理解和用法,get,set方法权限修饰符")

### 实现Java封装的步骤

1\. 修改属性的可见性来限制对属性的访问（一般限制为private），例如：

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgname5182014.png "Java封装的理解和用法,get,set方法权限修饰符")

这段代码中，将 **name** 和 **age** 属性设置为私有的，只能本类才能访问，其他类都访问不了，如此就对信息进行了隐藏。

2\. 对每个值属性提供对外的公共方法访问，也就是创建一对赋取值方法，用于对私有属性的访问，例如：

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggatsai90515181851.png "Java封装的理解和用法,get,set方法权限修饰符")

#### 调用：

1.要先new

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgnew0515183443.png "Java封装的理解和用法,get,set方法权限修饰符")

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img190530113439-20200918144656908.png "Java封装的理解和用法,get,set方法权限修饰符")

2.在调用 ste是进行赋值 get进行获取打印

![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggetsa190515183535-20200918144705240.png "Java封装的理解和用法,get,set方法权限修饰符")![Java封装的理解和用法,get,set方法权限修饰符](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20190530113431.png "Java封装的理解和用法,get,set方法权限修饰符")

eclipse中快速生成get和set的快捷键 shift+alt+s