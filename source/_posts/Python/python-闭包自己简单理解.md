---
title: python闭包自己简单理解
categories: 技术笔记
tags:
  - Python
  - 个人笔记
  - 闭包
excerpt: 1)必须有一个内嵌函数(函数里定义的函数）——这对应函数之间的嵌套
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918152034.png'
articleLink: 5c362c59db793207
date: 2019-06-07 18:09:32
---

### 条件：

1)必须有一个内嵌函数(函数里定义的函数）——这对应函数之间的嵌套  
2)内嵌函数必须引用一个定义在闭合范围内(外部函数里)的变量——内部函数引用外部变量  
3)外部函数必须返回内嵌函数——必须返回那个内部函数

**闭包是由函数及其相关的引用环境组合而成的实体**

**(即：闭包=函数+引用环境)(想想Erlang的外层函数传入一个参数a, 内层函数依旧传入一个参数b, 内层函数使用a和b, 最后返回内层函数)**

函数可以作为另一个函数的参数或返回值

简单应用：我想计算学生1的总成绩 然后我 计算到一半在计算学生2的成绩 他们两个不影响 我再回去计算学生1的成绩还是接着那个计算

请看下面的列子（自己理解）

```
#定义一个初始值
origin = 0
def school(point):
    #传入一个值
    def grow(step):
        #使用外层函数的 变量
        nonlocal point
        # 进行 加法操作
        new_point = point+step
        # 给初始值赋值
        point = new_point
        return new_point
    return grow
#定义一个新的 传入初始值为  0
student1 = school(origin)
#定义一个新的 传入初始值为  0
student2 = school(origin)
# 传入的是 0 第一次 结果是10
print(student1(10))
#第二次已经加了10 了 就会在 10的基础上在进行加
print(student1(10))
#此时传入的 是 0 所以就会 12+0
print(student2(12))
```

