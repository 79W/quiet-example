---
title: Java给定一个百分制的分数，输出相应的等级
categories: 技术笔记
tags:
  - Java
excerpt: Java给定一个百分制的分数，输出相应的等级。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bea6e82a2fe1.png'
articleLink: ec332728d2153313
date: 2018-11-13 14:08:33
---

| 90 分以上 | A 级 |
| --------- | ---- |
| 80~89     | B 级 |
| 70~79     | C 级 |
| 60~69     | D 级 |
| 60 分以下 | E 级 |

```
import java.util.Scanner;

class Mark{

public static void main(String[] args){

System.out.println("请输入一个分数");

//定义输入的分数为“mark”，且分数会有小数

double mark;

Scanner scanner = new Scanner(System.in);

mark = scanner.nextDouble();

//判断是否有输入错误。
 

if(mark<0||mark>100){

System.out.println("输入有误！ ");

System.exit(0);

}

/*判断分数的等级

90 分以上者 A 级， 80~89 分者 B 级，70~79 分者 C 级， 60~69 者 D 级，60 分以下 E 级 */

if (mark>=90) System.out.println("this mark is grade \'A\' "); 
else if (mark>=80) System.out.println("this mark is grade \'B\' "); 
else if (mark>=70) System.out.println("this mark is grade \'C\' "); 
else if (mark>=60) System.out.println("this mark is grade \'D\' "); 
else System.out.println("this mark is grade \'E\' ");
}

} 

```

![Java给定一个百分制的分数，输出相应的等级。](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bea6e82a2fe1.png "Java给定一个百分制的分数，输出相应的等级。")