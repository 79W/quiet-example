---
title: python 字典(Dictionary)的一些内置函数和经典例题
categories: 技术笔记
tags:
  - Python
  - 个人笔记
  - Dictionary
  - 内置函数
  - 字典
excerpt: 字典是另一种可变容器模型，且可存储任意类型对象。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918152034.png'
articleLink: 8b2d5ae8840d2423
date: 2019-05-23 18:43:24
---

字典是另一种可变容器模型，且可存储任意类型对象。

字典的每个键值 key=>value 对用冒号 : 分割，每个键值对之间用逗号 , 分割，整个字典包括在花括号 {} 中

## 修改字典

向字典添加新内容的方法是增加新的键/值对，修改或删除已有键/值对如下实例:

```
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
 
dict['Age'] = 8 # 更新
dict['School'] = "RUNOOB" # 添加
 
 
print "dict['Age']: ", dict['Age']
print "dict['School']: ", dict['School']
```




## 删除字典元素

能删单一的元素也能清空字典，清空只需一项操作。

显示删除一个字典用del命令，如下实例：

```
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
 
del dict['Name']  # 删除键是'Name'的条目
dict.clear()      # 清空词典所有条目
del dict          # 删除词典
 
print "dict['Age']: ", dict['Age'] 
print "dict['School']: ", dict['School']
```



## 字典键的特性

字典值可以没有限制地取任何python对象，既可以是标准的对象，也可以是用户定义的，但键不行。

### 两个重要的点需要记住：

> 1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，  
> 2）键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行，

## 字典的一些函数：

`cmp(dict1, dict2)`  
比较两个字典元素。

`len(dict)`  
计算字典元素个数，即键的总数。

`str(dict)`  
输出字典可打印的字符串表示。

`type(variable)`  
返回输入的变量类型，如果变量是字典就返回字典类型。

`dict.clear()`  
删除字典内所有元素

`dict.copy()`  
返回一个字典的浅复制

`dict.fromkeys(seq[, val])`  
创建一个新字典，以序列 seq 中元素做字典的键，val 为字典所有键对应的初始值

`dict.get(key, default=None)`  
返回指定键的值，如果值不在字典中返回default值

`dict.has_key(key)`  
如果键在字典dict里返回true，否则返回false

`dict.items()`  
以列表返回可遍历的(键, 值) 元组数组

`dict.keys()`  
以列表返回一个字典所有%9

### 老师给的重点例题： 

```
#定义字典
cities = {'CA':'San Francisco','MI':'Detroit','FL':'Jacksonville'}
#打印字典

#定义函数
#print(cities['CA'])
#此函数是 返回你查询的字段 
#传入的是一个 themap 字典 state是查询的字段
def find_city(themap,state):
    if state in themap:
        #如果有就返回
        #print(cities['CA'])
        return themap[state]
    else:
        return"没有找到"
#添加字段
#'find': 
# 这个也等于函数体
#也可以倒着 find_city = cities['find']
cities['find'] = find_city

while True:
    print("回车结束")
    state = input(">")
    if not state:break
    #给函数传值 并设置变量
    city_found = cities['find'](cities,state)
    #打印 结果
    print(city_found)
```

