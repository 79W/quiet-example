---
title: 'Java构造器,构造方法,构造函数的理解和用法'
categories: 技术笔记
tags:
  - Java
  - 构造方法
excerpt: 构造器的注意事项：1.构造器的名称必须和类名一致; 2.一个类中可以定义多个构造器，但是构造器的参数列表必须不同;
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggouzaoqi133134.png'
articleLink: e98637e9edaa2314
date: 2019-05-14 19:32:23
---

### 构造器的注意事项：

1.构造器的名称必须和类名一致;  
2.一个类中可以定义多个构造器，但是构造器的参数列表必须不同;  
3.如果我们没有手动定义构造器，则java系统会提供一个默认的构造器给我们使用。一旦我们定了构造器，则系统会把构造器收回;  
4.构造器的作用：实例化对象，给对象赋初始值;  
5.代码游离块优先执行。

```
public class Dog{
//属性
private String DogName;
private String DogColor;
private int DogAge;

//构造器的语法：
//[<访问控制修饰符1>] <类名>（[参数列表..]）{//初始化操作}
//1.默认无参的构造器
private Dog(){
System.out.println("默认无参的构造器");
}

//2.有一个String参数的构造器，并可以给DogName赋值
private Dog(String name){
DogName=name;
}

//3.可以给所有属性赋值的构造器
private Dog(String DogName,String DogColor,int DogAge){
this.DogName=DogName;
this.DogColor=DogColor;
this.DogAge=DogAge;
}
{
System.out.println("-----游离块开始执行");
boolean flag=true;
System.out.println("游离块"+flag);
}

public static void main(String[] args){
//构造器和new关键字一起使用，用于创建累的对象
//创建狗的对象
//类名 对象名=new 构造器

Dog dog1=new Dog();
String DogName=dog1.DogName;
System.out.println(DogName);

Dog dog2=new Dog("加菲狗");
String Dog=dog2.DogName;
System.out.println(Dog);

Dog dog3=new Dog("金毛","金色",3);
String Dog3=dog3.DogName+"是"+dog3.DogColor+"，年龄是"+dog3.DogAge;
System.out.println(Dog3);
}

}

```

![Java构造器,构造方法,构造函数的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggouzaoqi133134.png "Java构造器,构造方法,构造函数的理解和用法")

![Java构造器,构造方法,构造函数的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggouzaoqi3742-20200918145042449.png "Java构造器,构造方法,构造函数的理解和用法")

###  构造器的基本用法：

![Java构造器,构造方法,构造函数的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imggouzaoqi133549-20200918145050421.png "Java构造器,构造方法,构造函数的理解和用法")  


#### 构造器的调用 必须配合 new 使用

![Java构造器,构造方法,构造函数的理解和用法](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdiaopyonggopuzaoqi616.png "Java构造器,构造方法,构造函数的理解和用法")