---
title: Python中两个变量进行交换的五种方法
categories: 技术笔记
tags:
  - 个人笔记
  - Python
  - 变量交换
excerpt: 所有语言都可以通过这种方式进行交换变量)通过新添加中间变量的方式交换数值.
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918152034.png'
articleLink: 4984882937d24710
date: 2019-05-10 17:04:47
---

**方法一：(所有语言都可以通过这种方式进行交换变量)**

通过新添加中间变量的方式，交换数值.

下面通过一个demo1函数进行演示：

```
def demo1(a,b):
    temp = a
    a = b
    b = temp
    print(a,b)
```

**方法二：（此方法是Python中特有的方法）**

直接将a， b两个变量放到元组中，再通过元组按照index进行赋值的方式进行重新赋值给两个变量。

下面通过一个demo2函数进行演示:

```
def demo2(a,b):
    a,b = b,a
    print(a,b)
```

**方法三：**

通过简单的逻辑运算进行将两个值进行互换

下面通过一个demo3函数进行演示:

```
def demo3(a, b):
    a = a + b
    b = a - b
    a = a - b
    print(a, b)
```

**方法四：**

通过异或运算 将两个值互换 异或运算的原理是根据二进制中的 “1^1=0 1^0=1 0^0=0”

下面通过一个demo4函数进行演示:

```
def demo4(a,b):
    a = a^b  
    b = a^b  # b = (a^b)^b = a
    a = a^b  # a = (a^b)^a = b
    print(a,b)
```

**方法五：**
方法和加减法是一样的！
下面通过一个demo5函数进行演示:

```
def demo5(a,b):
    a = a * b

    b = a / b

    a = a / b

    print(a,b)
```

 

这五种方法就是标准的五个方法了！欢迎你有更好的方法！