---
title: Java递归的理解和用法
categories: 技术笔记
tags:
  - Java
  - 递归
excerpt: 当函数直接或者间接调用自己时，则发生了递归。递归是一种常见的解决问题的方法，寄把问题逐渐简单化。递归的基本思想就是 “自己调用自己”
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdiguiyanshidaima.png'
articleLink: facbe9d5cc143014
date: 2019-05-14 19:49:30
---

## 递归

简单定义：当函数直接或者间接调用自己时，则发生了递归。

递归是一种常见的解决问题的方法，寄把问题逐渐简单化。递归的基本思想就是 “自己调用自己”

一个使用递归技术的方法会直接或间接的调用自己

递归构造包括两个部分：

定义递归头。什么时候不调用自身方法，如果没有头，将陷入死循环递归体。

其实递归算法很简单，简单点就是自己调用自己的方法，有条件判断什么时候停止！

### 演示案例1：

一列数的规则如下: 1、1、2、3、5、8、13、21、34 ，求第30位数是多少？使用递归实现

    public class FibonacciSequence {
    public static void main(String[] args){
             System.out.println(Fribonacci(9));
                                   }
    public static int Fribonacci(int n){
                if(n<=2)
                   return 1;
                else
                   return Fribonacci(n-1)+Fribonacci(n-2);
    
                                       }
                                   }





### 演示案例2：

![Java递归的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdiguiyanshidaima.png "Java递归的理解和用法")