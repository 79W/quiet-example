---
title: Python小甲鱼猜数字完整版代码！
categories: 技术笔记
tags:
  - Python
  - 个人笔记
  - 作业练习
  - 小甲鱼
  - 猜数字
excerpt: Python小甲鱼猜数字完整版代码！
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918153739.png'
articleLink: d6e4413906371811
date: 2018-10-11 16:14:18
---

**代码：**

```
num=input ("来设定游戏次数：")
num =  int (num)
num=num-1
import random
secret = random.randint(1,10)
temp = input ("来猜一下我心中的数字吧！：")
guess =  int (temp)
if guess == secret:
          print  ("你是我肚子里的蛔虫吗？")
          print  ("猜中也没有奖励哦！？")
else:
          if guess>secret:
              print("猜大了哦！")
          else:
              print ("猜小了哦！")
while guess != secret and num>0:
     temp = input ("猜错了哦 在猜一下吧！！：")
     guess =  int (temp)
     num=num-1
     if guess > secret :
          print("猜大了哦！")
     if guess < secret:
          print("猜小了哦！")
     if guess == secret:
          print  ("你是我肚子里的蛔虫吗？")
          print  ("猜中也没有奖励哦！？")
          break
print("游戏结束了！")
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918153739.png)

[资源下载](https://www.lanzous.com/i22orqb)

