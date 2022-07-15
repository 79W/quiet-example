---
title: Java面向对象之继承的用法和理解
categories: 技术笔记
tags:
  - Java
  - 面向对象
excerpt: Java面向对象之继承的用法和理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img79bkjicheng06300326.png'
articleLink: 3f738ce93ab30517
date: 2019-05-17 17:06:05
---


![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img79bkjicheng06300326.png "Java面向对象之继承的用法和理解")

### 继承的作用：

1\. 解决了代码重复的问题  
2\. 真正的作用：表现出一个体系

**Object:为所有类的直接父类或间接父类**

### 子类复用了父类的状态和功能，继承了那些（访问权限修饰符）：

1\. 父类成员使用public修饰，子类继承  
2\. 父类成员使protected修饰,子类继承,即使父类和子类不再一个包下也可以用  
3\. 如果子类和父类作用一个包下, 子类可以继承父类中没有修饰符的成员  
4\. 父类成员使用private修饰,子类继承不到  
5.父类的构造器，子类不能继承.构造器必须和当前类名相同

![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img530113643.png "Java面向对象之继承的用法和理解")

![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgzileibujicheng90517162212.png "Java面向对象之继承的用法和理解")

### 继承的编写格式:

需要使用 extends 关键字来表示子类和父类之间的关系

```
public class 子类名称 extends 父类名称{

//写自己的方法等

}
```

java中类和类之间是继承关系 只允许单继承不可以多继承

![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjichengjieshi55041.png "Java面向对象之继承的用法和理解")

![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdajieshijicheng517155612.png "Java面向对象之继承的用法和理解")

### override：方法重写/方法覆盖/方法覆写

#### 判断的原则：一同两小一大

一同：方法签名相同  
两小：1.子类方法返回类型必须和父类方法返回类型相同或者是其子类  
2.子类方法声明抛出的异常必须是父类方法声明抛出异常相同 或者是其 子类  
一大：子类方法的访问权限比父类方法访问权限更大或者相等

**只有方法存在覆盖的概念，字段没有！**  
方法覆盖解决问题：  
当父类的某个行为不符合子类具体的特征时候 就需要子类需要重新定义父类的方法，并写在方法体里

方法重载（overload）和方法重写（override）

注：重写父类里的fly()方法

![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgchongxie17170257-20200918151057218.png "Java面向对象之继承的用法和理解")

super关键字![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img7163524-20200918151107558.png "Java面向对象之继承的用法和理解")![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img2-1.png "Java面向对象之继承的用法和理解")![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img2-2.png "Java面向对象之继承的用法和理解")![Java面向对象之继承的用法和理解](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img2-3.png "Java面向对象之继承的用法和理解")

构造方法调用时，super与this不能同时出现。

