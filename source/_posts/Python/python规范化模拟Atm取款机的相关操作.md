---
title: python规范化模拟Atm取款机的相关操作
categories: 技术笔记
tags:
  - Python
  - 作业练习
  - ATM
  - 代码规范
excerpt: python规范化模拟Atm取款机的相关操作
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191104-093753@2x-20200918152756245.png'
articleLink: 41a17e26d72a2004
date: 2019-11-04 09:45:20
---

## 项目功能：

1.  登录（可支持多个账户（非同时）登录）。
2.  注册。
3.  查看余额。
4.  存钱。
5.  转账（给其他用户转钱）。
6.  查看账户流水。
7.  退出

## 提供的思路：

ATM直译就是取款机，但是咱们是模拟一个取款机，此取款机可以完成实现存钱，转账，查看余额，以及查看账户流水等功能。

要求以及分值分配：

1.  利用装饰器完成登录验证功能（3,4,5,6功能需要验证）。
2.  登录功能要求：用户名、密码（密码需要md5加密）从文件中读取，进行三次验证，验证不成功则退出整个程序。
3.  注册功能要求：
    *   用户名要求：只能含有字母数字不能含有特殊字符并且要确保唯一性。
    *   密码的要求：长度在6与14个字符之间，密文存储。
    *   初始钱数：money: 0.
    *   **注意**：每个用户的以上信息通过字典以及json模块，以 用户名.json的形式存储，用户的json文件存储在db文件夹中。
4.  查看余额功能要求：用户登录成功之后，选择此功能即可显示账户余额，并且将每次查看记录通过日志的方式记录到用户日志中（用户日志文件建议为：用户名.log）。
5.  存钱功能要求：用户通过输入存储的钱数，然后将存储的钱累加到用户名.json那个json文件的字典中，并且将每次存钱记录通过日志的方式记录到用户日志中（用户日志文件建议为：用户名.log）。
6.  转账功能要求：用户通过输入对方用户名以及转账钱数完成给对方转账功能。
    *   要检测输入的对方用户账户是否存在。
    *   要检测你账户余额是否够用。
    *   将每次转账记录通过日志的方式记录到用户日志中（用户日志文件建议为：用户名.log）。
7.  查看流水要求：用户通过选择此功能将用户专属的log打印出来。
8.  整个项目完成流畅，逻辑清楚，极少bug。

* * *

本人写这个的一个大体流程：

写目录-写功能-写装饰器-写日志-测试

（只供参考，可能我的思路都有问题）

![python规范化模拟Atm取款机的相关操作](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191104-093753@2x-20200918152756245.png "python规范化模拟Atm取款机的相关操作")

目录结构：

![python规范化模拟Atm取款机的相关操作](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgmulujiegou-20200918152802232.png "python规范化模拟Atm取款机的相关操作")

#### start.py文件：

```
#start.py
from core import src
if __name__ == '__main__':
    src.run()
```

 

#### settings.py文件：

```
#settings.py
# 路径
pathbd = '../db/'
pathlog = '../log/'
```

 

#### src.py文件：

```
#src.py

import json
import hashlib
from lib.common import isuserlist,dblistname,userlog,auth
from conf.settings import pathbd,pathlog

denguser = {
    'name':None,
    'isdeng' : False,
    'username':None,
}
iswhile = True
# 登陆页面
def login():
    exitnum = 0
    while exitnum<3:
        print('===========登陆页面============')
        userz = input('请输入账号：输入 "退出" ').strip()
        userm = input('请输入密码：').strip()
        md5 = hashlib.md5()
        md5.update(userm.encode("utf-8"))
        pwd = md5.hexdigest()
        userlist = dblistname()
        if '%s.json'%userz in userlist:
            with open('%s%s.json'%(pathbd,userz), 'r',encoding='utf-8') as userzmy:
                userj = json.loads(userzmy.read())
                if userj['userm'] == pwd:
                    print("登陆成功")
                    denguser['name'] = userz
                    denguser['isdeng'] = True
                    exitnum = 3
                elif not denguser['isdeng']:
                    print("密码错误！")
                    exitnum += 1
        elif userz == '退出':
            print("已退出")
            exitnum = 3
        else:
            print("没有此用户名")
            exitnum += 1

# 注册页面
def zhuceuser():
    exitnum = 0
    while exitnum < 3:
        print("===========注册页面============")
        userz = input("请输入账号：输入'退出'").strip()
        userm = input("请输入密码：").strip()
        userlist = dblistname()
        if exitnum == 2:
            print("你没机会了请重新进入注册页面")
            break
        elif '%s.json'%userz in userlist:
            print('用户存在')
            exitnum +=1
            print('你还有%s次机会'%(3-exitnum))
        elif not userz.isalnum():
            print("你输入的账号存在非法字符")
            exitnum += 1
            print('你还有%s次机会' %(3-exitnum))
            # 方法检测字符串是否由字母和数字组成。
        elif userz == '退出':
            print("已退出")
            exitnum = 3
        elif 6 < len(userm) < 14:
            print("你输入的账号密码正常")
            with open('%s%s.json'%(pathbd,userz),'a+',encoding='utf-8') as cunuser:
                dic = {}
                dic['username'] = userz
                md5 = hashlib.md5()
                md5.update(userm.encode("utf-8"))
                pwd = md5.hexdigest()
                dic['userm'] = pwd
                dic['money'] = 0
                strr = json.dumps(dic)
                cunuser.write(strr)
                print('注册成功')
                exitnum = 3

# 查看余额
@auth
def shoumoney():
    with open('%s%s.json'%(pathbd,denguser['name']),'r',encoding='utf-8') as shouuser:
        moneyu = json.loads(shouuser.read())
        print('用户名：%s，余额：%s元'%(denguser['name'],moneyu['money']))
        userlog(denguser['name'],'查看余额，当前余额为：%s元'%moneyu['money'])

# 存钱
@auth
def addmoney():
    newmoney = input("请输入你要充值的金额:")
    with open('%s%s.json'%(pathbd,denguser['name']),'r+',encoding='utf-8') as ismoney:
        newtext =  json.loads(ismoney.read())
        newtext['money'] = int(newtext['money']) + int(newmoney)
        with open('%s%s.json' %(pathbd, denguser['name']), 'w+') as xieru:
            xienew = json.dumps(newtext)
            xieru.write(xienew)
            print("存款成功")
            print('目前您用户名：%s的余额是：%s'%(denguser['name'],newtext['money']))
            userlog(denguser['name'], '存款操作，余额为：%s元 增加：%s元，增加后余额为：%s' % (int(newtext['money']) - int(newmoney), newmoney, newtext['money']))

# 转账
@auth
def zhuanzhang():
    username2 = input("请输入你要转账的用户名：")
    userlist = dblistname()
    if '%s.json'%username2 in userlist:
        with open('%s%s.json'%(pathbd,denguser['name']),'r',encoding='utf-8') as userjson:
            user1json = json.loads(userjson.read())
            print("您现在的余额是：%s"%user1json['money'])
            zhuanmoney = input("请输入你要给他转多少")
            if int(zhuanmoney) <= int(user1json['money']): user1json['money'] = int(user1json['money']) - int(zhuanmoney) with open('%s%s.json' % (pathbd,denguser['name']), 'w+',encoding='utf-8') as xieru: xienew = json.dumps(user1json) xieru.write(xienew) with open('%s%s.json' %(pathbd,username2), 'r+',encoding='utf-8') as ismoney2: newtext2 = json.loads(ismoney2.read()) newtext2['money'] = int(newtext2['money']) + int(zhuanmoney) with open('%s%s.json' %(pathbd,username2), 'w+') as xieru2: xienew2 = json.dumps(newtext2) xieru2.write(xienew2) print("转账成功") userlog(denguser['name'], '转账操作，余额为：%s元 转走：%s元，转走后余额为：%s，转给：%s' % (int(user1json['money']) - int(zhuanmoney), zhuanmoney, user1json['money'],username2)) userlog(username2, '收款，加：%s元 ，转账人：%s' % (zhuanmoney,denguser['name'])) elif int(zhuanmoney) >  int(user1json['money']):
                print("您的余额不足")
    else:
        print("你输入的用户名不存在")


#查看账户流水
@auth
def shouliushui():
    with open('%s%s.log'%(pathlog,denguser['name']),encoding='utf-8') as shoulog:
        print('--------------用户日志---------------')
        print(shoulog.read())
        print('------------------------------------')

# 退出程序
def exitatm():
    global iswhile
    iswhile = False
    return iswhile

def run():
    choice_dict = {
        1: login,
        2: zhuceuser,
        3: shoumoney,
        4: addmoney,
        5: zhuanzhang,
        6: shouliushui,
        7: exitatm,
    }
    while iswhile:
        print('''
        欢迎来到ATM系统
        1:请登录
        2:请注册
        3:查看余额
        4:存钱
        5:转账
        6:查看账户流水
        7:退出程序''')
        choice = input('请输入您选择的序号:').strip()
        # isdigit 检测是否由字符串组成
        if choice.isdigit():
            choice = int(choice)
            if 0 < choice <= len(choice_dict):
                # 动态调用函数
                choice_dict[choice]()
            else:
                print("您输入的超出范围")
        else:
            print("输入不合规范,重新输入")
```

 

#### json文件：

```
#json文件
{"username": "qiaoyue", "userm": "9bf3df1dded4af5abcf2971e0195620c", "money": 5801}
```

 

#### common.py文件：

```
#common.py
import json
import os
import logging
from core import src
from conf.settings import pathbd,pathlog

# 装饰器
def auth(f):
    def inner():
        if src.denguser["isdeng"]:
            ret = f()
            return ret
        else:
            src.login()
            ret = f()
            return ret
    return inner

# 公共函数 获取用户文件的 用户名
def isuserlist():
    unamelist = []
    with open('%.json','r') as isuser:
        for lint in isuser:
            unamelist.append(json.loads(lint)['username'])
    return unamelist

# 获取目录下的文件名称
def dblistname():
    userlist = os.listdir(pathbd)
    return userlist


# 生成日志
def userlog(path,nei):
    logger = logging.getLogger()
    # 创建一个handler，用于写入日志文件
    fh = logging.FileHandler('%s%s.log'%(pathlog,path), encoding='utf-8')
    # 再创建一个handler，用于输出到控制台
    ch = logging.StreamHandler()
    formatter = logging.Formatter('时间：%(asctime)s - 系统用户：%(name)s - 操作内容：%(message)s')
    fh.setLevel(logging.DEBUG)
    fh.setFormatter(formatter)
    ch.setFormatter(formatter)
    logger.addHandler(fh)  # logger对象可以添加多个fh和ch对象
    logger.addHandler(ch)
    logger.critical('%s'%nei)
    fh.close()
    ch.close()

    logger.removeHandler(fh)
    logger.removeHandler(ch)
```

 

#### 日志文件：

```
#日志
时间：2019-11-02 16:18:31,871 - 系统用户：root - 操作内容：查看余额，当前余额为：0元
时间：2019-11-02 16:51:00,489 - 系统用户：root - 操作内容：查看余额，当前余额为：0元
时间：2019-11-02 16:51:04,014 - 系统用户：root - 操作内容：存款操作，余额为：0元 增加：3456元，增加后余额为：3456
时间：2019-11-02 16:51:14,731 - 系统用户：root - 操作内容：存款操作，余额为：3456元 增加：2345元，增加后余额为：5801
```

------

 

[资源下载](https://www.lanzous.com/i75rush)