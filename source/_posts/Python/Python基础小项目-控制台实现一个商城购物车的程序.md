---
title: Python基础小项目 控制台实现一个商城购物车的程序
categories: 技术笔记
tags:
  - Python
  - 作业练习
  - 购物车
excerpt: Python基础小项目控制台实现一个商城购物车的程序
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191029-092426@2x.png'
articleLink: ce5d01b425bf3929
date: 2019-10-29 09:25:39
---


![Python基础小项目 控制台实现一个商城购物车的程序](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgQQ20191029-092426@2x.png "Python基础小项目 控制台实现一个商城购物车的程序")

# 项目需求：

1.  商品信息在文件存储的，存储形式：​ name price
    
     电脑 1999
    
     鼠标 10
    
     游艇 20
    
     美女 998
    
     …
    
2. 用户先给自己的账户充钱：比如先充3000元。

3. 读取商品信息文件将文件中的数据转化成下面的格式：

   ```
    goods = \[{“name”: “电脑”, “price”: 1999},  
   ​ {“name”: “鼠标”, “price”: 10},  
   ​ {“name”: “游艇”, “price”: 20},  
   ​ {“name”: “美女”, “price”: 998},
   
    … \]
   ```

4. 页面显示 序号 + 商品名称 + 商品价格，如：

 1 电脑 1999

 2 鼠标 10

 …

5.  n 购物车结算
    6.  q或者Q退出程序。
    7.  用户输入选择的商品序号，然后打印商品名称及商品价格,并将此商品，添加到购物车，用户还可继续添加商品。
    8.  如果用户输入的商品序号有误，则提示输入有误，并重新输入。
    9.  用户输入n为购物车结算，依次显示用户购物车里面的商品，数量及单价，若充值的钱数不足，则让用户删除某商品，直至可以购买，若充值的钱数充足，则可以直接购买。
    10.  用户输入Q或者q退出程序。
    11.  退出程序之后，依次显示用户购买的商品，数量，单价，以及此次共消费多少钱，账户余额多少，并将购买信息写入文件。

## 实现基本思路：（本人的）

1.  熟练使用 if与while语法
2.  熟练操作字符串
3.  然后会对文件的存取
4.  难点是对数据存入到文件和取出后对数据进行一定的格式要求

*   第一步我先把这个整体的一个大while写好
*   然后在去实现每个功能
*   具体看代码
*   基本功能实现但是和s老师代码一比自己还只是个孩子啊！
*   加油！


## 代码：

```
#第一章大作业
def run():
    print("""
    1.管理员界面
    2.用户界面
    """)
    intext1 = input("请选择功能")
    if intext1 == '1':
        print("""
        1.添加商品
        2.显示商品
        """)
        intext2 = input("请选择功能")
        #添加商品
        if intext2 == '1':
            file = open('text.text', 'a+')
            name = input("请输入名称：")
            price = input("请输入价格：")
            file.write("\n")
            zong = "%s %s" % (name, price)
            file.write(zong)
            file.close()
        #显示商品（按一定格式显示）
        elif intext2 == '2':
            text2 = []
            file = open('text.text', 'r')
            text = file.read().split( )
            # print(len(text))
            for i in range(2,len(text),2):
                # print(i)
                dictzip = {}
                dictzip[text[0]]=text[i]
                dictzip[text[1]] = text[i+1]
                text2.append(dictzip)
            print(text2)
            file.close()
        else:
            print("输入错误")
    elif intext1 == '2':
        #充值金额
        money = input("请输入充值金额：")
        file = open('usermoney.text','a+')
        file.write("用户余额：%s\n"%money)
        print("充值成功")
        file.close()
        #购物车
        text2 = []
        text3 = []
        file = open('text.text', 'r')
        text = file.read().split()
        # print(len(text))
        for i in range(2, len(text), 2):
            # print(i)
            dictzip = {}
            dictzip[text[0]] = text[i]
            dictzip[text[1]] = text[i + 1]
            text2.append(dictzip)
        num = 0
        for i in text2:
            num+=1
            if num == 1:
                print("序号 名称   价格")
            print(" %s   %s   %s"%(num,i['name'],i['price']))

        # print(text2)
        file.close()

        while True:

            userin = input("请选择序号添加到购物车：")
            if userin == 'q' or userin == 'Q':
                print("退出程序")
                break
            elif userin == 'n' or userin == 'N':
                print("结算购物车")
                # while True:
                umoney = 0
                for i in text3:
                    umoney +=int(i['price'])
                num = 0
                for i in text3:
                    num += 1
                    if num == 1:
                        print("现在购物车内的商品")
                        print("序号 名称   价格")
                    print(" %s   %s   %s" % (num, i['name'], i['price']))
                print("商品总价：%s，你的钱包有：%s"%(umoney,money))
                print(type(umoney))
                print(type(money))
                while umoney > int(money):
                    print("你的钱包不够需要删除商品")
                    spnum = input("请输入删除商品的编号：")
                    if int(spnum) <= len(text3):
                        text3.pop(int(spnum)-1)
                        umoney = 0
                        for i in text3:
                            umoney += int(i['price'])
                        print("商品总价：%s，你的钱包有：%s" % (umoney, money))
                        num = 0
                        for i in text3:
                            num += 1
                            if num == 1:
                                print("现在购物车内的商品")
                                print("序号 名称   价格")
                            print(" %s   %s   %s" % (num, i['name'], i['price']))
                    else:
                        print("输出错误")
                if umoney <= int(money):
                    # 显示用户购买的商品，数量，单价，以及此次共消费多少钱，账户余额多少，并将购买信息写入文件。
                    print("购买成功")
                    print("===================商品清单=====================")
                    num = 0
                    import datetime
                    nowTime = datetime.datetime.now().strftime('%Y%m%d%H%M%S')  # 现在
                    for i in text3:
                        num += 1
                        if num == 1:
                            print("序号 名称   价格")
                        text4 = " %s   %s   %s" % (num, i['name'], i['price'])
                        print(text4)
                        with open('%s.text' % nowTime, 'a+',encoding='utf-8') as filejin:
                            if num == 1:
                                filejin.write('===============消费记录=============\n')
                                filejin.write('序号 名称   价格\n')
                            filejin.write("%s\n"%text4)

                    text5 = "花费了：%s，余额为：%s" % (umoney, (int(money) - umoney))
                    with open('%s.text' % nowTime, 'a+') as filejin:
                        filejin.write('===================================\n')
                        filejin.write("%s\n" % text5)
                        filejin.write("订单生成时间：%s"%nowTime)
                    print(text5)

                    print("消费清单保存文件%s.text"%nowTime)

                    print("===============================================")

                    break

            elif int(userin) > len(text2):
                print("输出错误商品不存在")
            else:
                #添加到购物车
                text3.append(text2[int(userin)-1])
                num = 0
                for i in text3:
                    num += 1
                    if num == 1:
                        print("现在购物车内的商品")
                        print("序号 名称   价格  ")
                    print(" %s   %s   %s" % (num, i['name'], i['price']))
    else:
        print("输入错误")



if __name__ == '__main__':
    run()
```

