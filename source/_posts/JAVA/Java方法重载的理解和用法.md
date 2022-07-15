---
title: Java方法重载的理解和用法
categories: 技术笔记
tags:
  - Java
  - 重载
excerpt: 1.参数列表不同包括：个数不同、顺序不同、类型不同。2.仅仅参数变量名称不同是不可以的。3.跟成员方法一样，构造方法也可以重载。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgchongzai3004.png'
articleLink: d6efb1f4a0c95814
date: 2019-05-14 19:32:58
---


![Java方法重载的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgchongzai3004.png "Java方法重载的理解和用法")

#### 理解：

1.参数列表不同包括：个数不同、顺序不同、类型不同。  
2.仅仅参数变量名称不同是不可以的。  
3.跟成员方法一样，构造方法也可以重载。  
4.声明为final的方法不能被重载。  
5.声明为static的方法不能被重载，但是能够被在此声明。

## **方法的重载的规则：**

1.方法名称必须相同。  
2.参数列表必须不同。  
3.方法的返回类型可以相同也可以不相同。  
4.仅仅返回类型不同不足以称为方法的重载。

## **方法重载的实现：**

方法名称相同时，编译器会根据调用方法的参数个数、参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错，这叫做重载分辨。

### 演示案例：

#### 调用方法：

![Java方法重载的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjava%E9%87%8D%E8%BD%BD3.png "Java方法重载的理解和用法")

![Java方法重载的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjavachongzaishuchu08.png "Java方法重载的理解和用法")