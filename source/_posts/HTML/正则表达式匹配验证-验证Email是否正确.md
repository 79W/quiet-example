---
title: 正则表达式匹配验证-验证Email是否正确
categories: 技术笔记
tags:
  - 作业练习
  - Java
  - 基础知识
  - 正则表达式
excerpt: 正则表达式匹配验证-验证Email是否正确
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b6ad2feec092.png'
articleLink: 46c3a7b3c1434208
date: 2018-08-08 18:44:42
---

```
Zy01类：

package com.sj.zy;

public class Zy01 {
	public String sayHi(String name) {
		
		String regex="^[a-zA-Z_]{1,}[0-9]{0,}@(([a-zA-z0-9]-*){1,}\\.){1,3}[a-zA-z\\-]{1,}$";
		if(name.matches(regex)) {
			return"这个是邮箱："+name;
		}
		else {
			return "这个不是邮箱 ："+name;
		}
		
		
	}
	

}
```

```
CeShi类

package com.sj.zy;

import java.util.Scanner;

public class CeShi {

	public static void main(String[] args) {

    Scanner cin=new Scanner (System.in);
    System.out.println("请输入一个邮箱");
    String name =cin.nextLine();
    Zy01 zy1=new Zy01();
    String cs_sayHi=zy1.sayHi(name);
    System.out.println(cs_sayHi);
		
	}

}
```

## **简单基本基础演示：**

![正则表达式匹配验证-验证Email是否正确](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b6ad2feec092.png "正则表达式匹配验证-验证Email是否正确")

