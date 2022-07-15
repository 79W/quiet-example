---
title: 'Java,Python输出所有的水仙花数'
categories: 技术笔记
tags:
  - Java
  - Python
  - 水仙花数
excerpt: 输出所有的水仙花数把谓水仙花数是指一个数3位数，其各各位数字立方和等于其本身
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bea9027db142.png'
articleLink: e75e9206dc845513
date: 2018-11-13 16:51:55
---

输出所有的水仙花数，把谓水仙花数是指一个数 3 位数，其各各位数字立方和等于其本身，

例如： 153 = 1\*1\*1 + 3\*3\*3 + 5\*5\*5

```
package zy01;

public class Zy01 {
	
	/**
	 * @param args
	 */
	public static void main(String[] args){
		System.out.println("以下是所有的水仙花数");

		int number = 100;	// 由于水仙花数是三位数，故由 100 开始算起

		int i, j, k; // i j k 分别为 number 的百位、十位、个位 
		
		for (int sum; number<1000; number++){

		i=number/100; 
                j=(number-i*100)/10; 
                k=number-i*100-j*10; 
                sum=i*i*i+j*j*j+k*k*k;
		if (sum==number) 
                System.out.println(number+" is a dafodil number! "); }

		
	}
	} 
```

![Java,Python输出所有的水仙花数](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bea9027db142.png "Java,Python输出所有的水仙花数")

Python 水仙花数算法

    sum =0
    for n in range (100,1000):
        i = n // 100
        
        j = n // 10 % 10
        k = n % 10
        if n == i ** 3 + j ** 3 + k ** 3:
            sum+=1
            # 计数器 
            print(n)
    print(sum)

![Java,Python输出所有的水仙花数](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5beb7e7d93fed.png "Java,Python输出所有的水仙花数")