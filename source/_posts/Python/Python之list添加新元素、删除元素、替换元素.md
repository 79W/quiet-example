---
title: Python之list添加新元素、删除元素、替换元素
categories: 技术笔记
tags:
  - Python
  - 个人笔记
  - list
  - 删除元素
  - 替换元素
excerpt: Python之list添加新元素、删除元素、替换元素
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918152034.png'
articleLink: 838dd19a524d0122
date: 2018-11-22 11:55:01
---

## Python之list添加新元素

现在，班里有3名同学：

```
>>> L = ['Adam', 'Lisa', 'Bart']
```

今天，班里转来一名新同学 Paul，如何把新同学添加到现有的 list 中呢？

第一个办法是用 list 的` append() `方法，把新同学追加到 list 的末尾：

```
>>> L = ['Adam', 'Lisa', 'Bart']
>>> L.append('Paul')
>>> print L
['Adam', 'Lisa', 'Bart', 'Paul']
```

**append()**总是把新的元素添加到 list 的尾部。

如果 Paul 同学表示自己总是考满分，要求添加到第一的位置，怎么办？

方法是用list的` insert()`方法，它接受两个参数，第一个参数是索引号，第二个参数是待添加的新元素：

```
>>> L = ['Adam', 'Lisa', 'Bart']
>>> L.insert(0, 'Paul')
>>> print L
['Paul', 'Adam', 'Lisa', 'Bart']
```

**L.insert(0, ‘Paul’)** 的意思是，’Paul’将被添加到索引为 0 的位置上（也就是第一个），而原来索引为 0 的Adam同学，以及后面的所有同学，都自动向后移动一位。

假设新来一名学生Paul，Paul 同学的成绩比Bart好，但是比Lisa差，他应该排到第三名的位置，用代码实现是这样：

```
L = ['Adam', 'Lisa', 'Bart']
L.insert(2, 'Paul')
print L
```

## Python从list删除元素

Paul同学刚来几天又要转走了，那么我们怎么把Paul 从现有的list中删除呢？

如果Paul同学排在最后一个，我们可以用list的`pop()`方法删除：

```
>>> L = ['Adam', 'Lisa', 'Bart', 'Paul']
>>> L.pop()
'Paul'
>>> print L
['Adam', 'Lisa', 'Bart']
```

**pop()**方法总是删掉list的最后一个元素，并且它还返回这个元素，所以我们执行 L.pop() 后，会打印出 ‘Paul’。

如果Paul同学不是排在最后一个怎么办？比如Paul同学排在第三：

```
>>> L = ['Adam', 'Lisa', 'Paul', 'Bart']
```

要把Paul踢出list，我们就必须先定位Paul的位置。由于Paul的索引是2，因此，用` pop(2)`把Paul删掉：

```
>>> L.pop(2)
'Paul'
>>> print L
['Adam', 'Lisa', 'Bart']
```

## Python中替换元素

假设现在班里仍然是3名同学：

```
>>> L = ['Adam', 'Lisa', 'Bart']
```

现在，Bart同学要转学走了，碰巧来了一个Paul同学，要更新班级成员名单，我们可以先把Bart删掉，再把Paul添加进来。

另一个办法是直接用Paul把Bart给替换掉：

```
>>> L[2] = 'Paul'
>>> print L
L = ['Adam', 'Lisa', 'Paul']
```

对list中的某一个索引赋值，就可以直接用新的元素替换掉原来的元素，list包含的元素个数保持不变。

由于Bart还可以用 -1 做索引，因此，下面的代码也可以完成同样的替换工作：

```
>>> L[-1] = 'Paul'
```

班里的同学按照分数排名是这样的：

```
L = ['Adam', 'Lisa', 'Bart']
```

 

但是，在一次考试后，Bart同学意外取得第一，而Adam同学考了倒数第一。

请通过对list的索引赋值，生成新的排名。

```
L = ['Adam', 'Lisa', 'Bart']
L[0]='Bart'
L[-1]='Adam'
print L
```

