---
title: python基础实现-模拟博客园系统（装饰器）
categories: 技术笔记
tags:
  - Python
  - 作业练习
  - python基础
  - 博客园系统
  - 装饰器
excerpt: 首先程序启动，页面显示下面内容供用户选择请登录请注册进入文章页面进入评论页面入日记页面进入收藏页面注销账号
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdierzhang-20200918153015794.png
articleLink: c109f05b7ddf4829
date: 2019-10-29 09:31:48
---

## 项目分析：

### 一．首先程序启动，页面显示下面内容供用户选择：

1.  请登录
2.  请注册
3.  进入文章页面
4.  进入评论页面
5.  进入日记页面
6.  进入收藏页面
7.  注销账号
8.  退出整个程序

### 二．必须实现的功能：

1.  注册功能要求：
    
    a. 用户名、密码要记录在文件中。
    
    b. 用户名要求：只能含有字母或者数字不能含有特殊字符并且确保用户名唯一。
    
    c. 密码要求：长度要在6~14个字符之间。
    
    d. 超过三次登录还未成功，则退出整个程序。
    
2.  登录功能要求：
    
    a. 用户输入用户名、密码进行登录验证。
    
    b. 登录成功之后，才可以访问37选项，如果没有登录或者登录不成功时访问37选项，不允许访问，让其先登录。（装饰器）
    
3.  入文章页面要求：
    
    a. 提示欢迎xx进入文章页面。
    
    b. 此时用户可以选择：直接写入内容，还是导入md文件。
    
    ① 如果选择直接写内容：让学生直接写文件名|文件内容…最后创建一个文章。
    
    ② 如果选择导入md文件：让用户输入已经准备好的md文件的文件路径（相对路径即可：比如函数的进阶.md），然后将此md文件的全部内容写入文章（函数的进阶.text）中。
    
4.  进入评论页面要求：
    
    提示欢迎xx进入评论页面。
    
5.  进入日记页面要求：
    
    提示欢迎xx进入日记页面。
    
6.  进入收藏页面要求：
    
    提示欢迎xx进入收藏页面。
    
7.  注销账号要求：
    
    不是退出整个程序，而是将已经登录的状态变成未登录状态（访问3~7选项时需要重新登录）。
    
8.  退出整个程序要求：
    
    就是结束整个程序。
    

### 三．**选做功能**：

1.  评论页面要求：
    
    a. 提示欢迎xx进入评论页面。
    
    b. 让用户选择要评论的文章。
    
    这个需要借助于os模块实现此功能。将所有的文章文件单独放置在一个目录中，利用os模块listdir功能,可以将一个目录下所有的文件名以字符串的形式存在一个列表中并返回。
    

```
import os
print(os.listdir(r'D:\teaching_show\article'))
['01 函数的初识.text', '02 函数的进阶.text']
```

 c. 选择要评论的文章之后，先要将原文章内容全部读一遍，然后输入的你的评论，评论要过滤掉这些敏感字符：“苍老师”, “东京热”, “武藤兰”, “波多野结衣”，替换成等长度的”\*”之后，写在文章的评论区最下面。

文章的结构：

 文章具体内容

 …

 评论区：

 —————————————–

 (用户名)xx:

 评论内容

 (用户名)oo:

 评论内容

原文章最下面如果没有以下两行：

“””

评论区：

—————————————–

“””

就加上这两行在写入评论，如果有这两行则直接在下面顺延写上：  
(用户名)xx:

 评论内容

* * *

## 项目思维导图：

![python基础实现-模拟博客园系统（装饰器）](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdierzhang-20200918153015794.png "python基础实现-模拟博客园系统（装饰器）")

思维可能有可能不和你的一样

## 代码演示：

```
import time
import os
import re

login_status = {
    'username': None,
    'status': False,
    'index' : 0,
}

keywords = ("暴力", "黑社会", "苍井空", "波多野吉衣")

def run():
    while True:
        print("===============欢迎来到博客园=============")
        print("""
                =========用户中心==========
                    0.登陆
                    1.进入文章页面
                    2.进入评论页面
                    3.进入日记页面
                    4.进入收藏页面
                    5.注销账号
                    6.退出整个程序
                    7.注册账号
                ===========================
                            """)
        runusernum = input("请输入功能序号：")
        if runusernum == '0':
            login()
        elif runusernum == '1':
            popst()
        elif runusernum == '2':
            pinglun()
        elif runusernum == '3':
            rizhi()
        elif runusernum == "7":
            register()
        elif runusernum == '4':
            shoucang()
        elif runusernum == '5':
            login_status['status'] = False
            print("注销成功，请重新登陆！")
        elif runusernum == '6':
            print("退出整个程序")
            exit()
        else:
            print("输入错误重新输入")

        if login_status['index'] == 3:
            print("错误三次以上")
            print("关闭程序")
            exit()

def auth(func):
    def inner(*args,**kwargs):
        if login_status['status']:
            ret = func()
            return ret
        print("==========登陆博客园==========")
        zhanghao = input('请输入用户名：').strip()
        mima = input('请输入密码：').strip()
        with open('user.text', 'r') as isuser:
            userlist = isuser.read().split()
            for i in range(len(userlist) - 1):
                if userlist[i] == zhanghao and userlist[i + 1] == mima:
                    while True:
                        print("登陆成功")
                        print("==========================")
                        login_status['status'] = True
                        login_status['username'] = zhanghao
                        login_status['index'] += 0
                        ret = func()
                        return ret
                else:
                    if i + 2 == len(userlist):
                        login_status['index'] +=1
                        print("账号密码错误！！！！！")
    return inner


@auth
def login():
    pass



def register():
    numbreak = 0
    numlen = 0
    istf = True
    while istf:
        user = 0
        print("账号要求：只能含有字母或者数字不能含有特殊字符")
        print("输入'q'或'Q'退出程序")
        zhanghao = input("请输入账号：")
        for i in zhanghao:
            if ord(i) >= 48 and  ord(i) <= 57 or ord(i) >= 65 and ord(i) <= 90 or ord(i) >= 97 and ord(i) <= 122:
                numlen+=1
                if len(zhanghao) ==  numlen:
                    print("账号符合要求")
                    with open('user.text','r') as isuser:
                        userlist = isuser.read().split()
                        for i in userlist[::2]:
                            if i != zhanghao:
                                user +=1
                                if user == int(len(userlist)/2):
                                    mima =  input("请输入密码：")
                                    if len(mima) <= 16:
                                        with open('user.text','a+') as user:
                                            user.write('%s %s\n'%(zhanghao,mima))
                                            print("注册成功")
                                            user.close()
                                            login()

                            else:
                                print("账号有重复")

            elif numbreak == 3:
                print("你已经注册错误超过3次了！程序结束")

                exit()
            elif zhanghao == 'Q' or zhanghao == 'q':
                print("退出程序")
                exit()
            else:
                numbreak += 1
                print("你的账号不符合要求请重新输入")
                break


@auth
def popst():
    usernam = login_status['username']
    print("欢迎%s进入文章页"%usernam)
    while True:
        print("""
        
            ===========文章页功能选择=============
                    1.写入文章
                    2.导入文章
                    3.退出
            """)
        ponum = input("请输入功能序号：")
        if ponum == '1':
            print("输入 q Q 退出")
            title = input("请输入文件名称，标题！")
            if title == 'q' or title == 'Q':
                break
            time2 = time.strftime('%Y%m%d%H%M%S', time.localtime(time.time()))
            Article = '%s%s'%(title,time2)
            text = input("请输入文章内容：")
            if os.path.exists('Article/%s'%usernam):
                print("文件夹存在")
            else:
                os.makedirs('Article/%s'%usernam)
                print("创建文件夹成功")
            with open('Article/%s/%s.text'%(usernam,Article),'w') as popotext:
                popotext.write(text)
                popotext.close()
                print("写入成功")
                break
        elif ponum == '2':

            while True:
                print("输入 q Q 退出")
                title = input("请输入文件名称，标题！")
                time2 = time.strftime('%Y%m%d%H%M%S', time.localtime(time.time()))
                Article = '%s%s' % (title, time2)
                if title == 'q' or title == 'Q':
                    break
                textpath = input("请输入文章路径 或者直接放在同级路径 直接输入文章名称即可")
                if os.path.exists(textpath):
                    with open('%s' % textpath, 'r') as tetxpath:
                        text = tetxpath.read()
                        # print(text)
                    if os.path.exists('Article/%s' % usernam):
                        print("文件夹存在")
                    else:
                        os.makedirs('Article/%s' % usernam)
                        print("创建文件夹成功")
                    with open('Article/%s/%s.text' % (usernam, Article), 'w') as popotext:
                        popotext.write(text)
                        popotext.close()
                        print("写入成功")
                        break
                else:
                    print("你输入的文章不存在")


        elif ponum == '3':
            print("退出文章页成功")
            break
        else:
            print("输入错误 请重新输入！")


@auth
def pinglun():
    usernam = login_status['username']
    print("欢迎%s进入评论页"%usernam)
    print(os.listdir(r'Article'))
    user = input("请输入查看那个用户的文章：")
    if os.path.exists('Article/%s'%(user)):
        print(os.listdir(r'Article/%s'%user))
        usertexttitle = input("请选择文章：（直接输入文章名称即可）")
        if os.path.exists('Article/%s/%s'%(user,usertexttitle)):
            with open('Article/%s/%s'%(user,usertexttitle),'a+') as textnei:
                textnei.seek(0, 0)
                utext = textnei.read()
                print('文章内容：\n%s'%utext)
                print("==============================")
                textnei.seek(0, 0)
                neilist = []
                for lin in textnei.readlines():
                    textlin = lin.split()
                    for i in textlin:
                        neilist.append(i)
                if '评论区：' in neilist:
                    pass
                else:
                    tping = "\n评论区：\n-----------------------------------------\n"
                    textnei.write(tping)

                upinglun2 = input("请输入你评论的内容：")
                upinglun = check_filter(keywords, upinglun2)
                ustepin = "\n用户名：%s\n评论内容：%s\n-------------------------------"%(usernam,upinglun)
                textnei.write(ustepin)
                print("评论成功")
        else:
            print("你输入文章不存在")
    else:
        print("你输入的用户不存在")



@auth
def rizhi():
    print("欢迎进入日志页")
@auth
def shoucang():
    print("欢迎进入收藏页")

def check_filter(keywords, text):
    return re.sub("|".join(keywords), "***", text)

if __name__ == '__main__':
    run()
```

