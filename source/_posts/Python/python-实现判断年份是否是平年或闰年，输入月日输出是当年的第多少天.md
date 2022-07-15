---
title: python实现判断年份是否是平年或闰年，输入月日输出是当年的第多少天
categories: 技术笔记
tags:
  - Python
  - 个人笔记
  - 作业练习
  - 判断年份
excerpt: 实现判断年份是否是平年或闰年，输入月日输出是当年的第多少天
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img0531201349.png'
articleLink: 01590908cf922131
date: 2019-05-31 20:15:21
---

作业描述

*   定义一个函数，输入年，月，日，能够获得这个日期是这一年的第几天。要求：
*   对输入的数据进行验证
*   对平年闰年进行判断

本人的方法比较笨 只为记录  python有摸块可以直接实现

```
def Bissextile(sun1,sun2,sun3):
    zhi = True
    month1 = 0
    month2 = 0
    if sun1 > 0 and sun1 <= 3000:
        if sun1 % 4 == 0 and sun1 % 100 != 0 or sun1 % 400 == 0:
            print("%d是闰年" % sun1)
            zhi = True
        else:
            print("%d不是闰年" % sun1)
            zhi = False
    else:
        print("你输入的数据有错 年不大于3000 ")

    if sun2 > 0 and sun2 <=12 and sun3 > 0 and sun3 <= 31:
        if zhi:
            if sun2 == 2  and sun3 >29:
                print("你家润年二月 有 %d天？"%sun3)
            else:
                for n in range(sun2) :
                    n +=1
                    # print(n)
                    if n == 4 or n == 6 or n == 9 or n == 11 :
                        month1 += 1
                    elif n == 1 or n == 3 or n == 5 or n == 7 or n == 8 or n == 10 or n == 12 :
                        month2 += 1
                sun =month1*30 + month2 *31 + sun3
                print("你输入的%d月%s日是%d年的%d天"%(sun2,sun3,sun1,sun))

        else:
            if sun2 == 2  and sun3 >28:
                print("你家平年年二月 有 %d天？"%sun3)
            else:
                for n in range(sun2):
                    n += 1
                    if n == 4 or n == 6 or n == 9 or n == 11:
                        month1 += 1
                    elif n == 1 or n == 3 or n == 5 or n == 7 or n == 8 or n == 10 or n == 12:
                        month2 += 1
                sun = month1 * 30 + month2 * 31 + sun3
                print("你输入的%d月%s日是%d年的%d天" % (sun2, sun3, sun1, sun))
    else :
        print("你输入的数据有错 月不能大于12 日不能大于31")


def user_int():
    while True:
        exituser = int(input("退出请输入0 开始请输入1："))
        if exituser == 0:
            exit()
        else:
            year = int(input("请输入年份："))
            month = int(input("请输入月份："))
            day = int(input("请输入天数："))

            Bissextile(year, month, day)

if __name__ == '__main__':
    user_int()
```

![python 实现判断年份是否是平年或闰年，输入月日输出是当年的第多少天](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img0531201349.png "python 实现判断年份是否是平年或闰年，输入月日输出是当年的第多少天")