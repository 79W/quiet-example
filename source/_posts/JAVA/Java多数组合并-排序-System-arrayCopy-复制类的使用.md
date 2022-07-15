---
title: 'Java多数组合并,排序,System.arrayCopy 复制类的使用'
categories: 技术笔记
tags:
  - Java
  - 数组合并
  - 数组排序
excerpt: 'Java多数组合并,排序,System.arrayCopy 复制类的使用'
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c3068e1de5c9-20200918144350544.png
articleLink: ce67991acdf63505
date: 2019-01-05 16:33:35
---

作业题目：

![Java多数组合并,排序,System.arrayCopy 复制类的使用](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c3068e1de5c9-20200918144350544.png "Java多数组合并,排序,System.arrayCopy 复制类的使用") 思路 ：

* 首先 先定义两个 静态数组   数组a 和数组b；

* 然后 定义个动态数组 长度为  数组a 的长度 加上 数组b的长度；      `int[] dd = new int[aa.length + bb.length];`

* 使用 System.arrayCopy 复制类 进行复制到新数组中： 

*   ```
    System.arrayCopy 复制类的简单介绍： {
    
    System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)
    代码解释:
    　　Object src : 原数组
       int srcPos : 从元数据的起始位置开始
    　　Object dest : 目标数组
    　　int destPos : 目标数组的开始起始位置
    　　int length : 要copy的数组的长度
    
    
    }
    
    ```
    
*   最后 合并完后再选择一个排序的方法！进行排序 详情看下面的代码！

##### 代码实现：

```
/*数组合并 并排序*/
package shuzuhbpx;

import java.util.Arrays;

public class paixunhebing {

	public static void main(String[] args) {
		int[] aa = {234,4365,675,234,645,24267,24,55,66,88,};
		int[] bb = {121,323,434,545,665,7754,83,53,};
		

		//定义一个动态数组 长度为 上面两个数组的长度之和
		int[] dd = new int[aa.length + bb.length];
		//将 aa数组的 0 位置开始拷贝到dd数组中从0开始  拷贝的长度为 aa数组的长度
		System.arraycopy(aa, 0, dd, 0, aa.length);
		//将bb数组的0位置开始拷贝到dd数组的aa数组长度的地方开始  拷贝的长度为 bb数组的长度
		System.arraycopy(bb, 0, dd, aa.length, bb.length);
		System.out.println("合并后数组："+Arrays.toString(dd));
		
		//冒泡排序
		for(int i = 0 ; i<dd.length-1;i++) {
			for(int j =0;j<dd.length-1-i;j++ ) {
				if(dd[j]>dd[j+1]) {
					int tem = dd[j];
					dd[j] = dd[j+1];
					dd[j+1] = tem;
				}
			}
		}
		
		System.out.println("冒泡排序：\n升序："+Arrays.toString(dd));
		
		
		for(int i = 0 ; i<dd.length;i++) {
			int xiabiao = i;
			for(int j = i+1 ; j<dd.length;j++) {
				if(dd[j]<dd[xiabiao]) {
					xiabiao = j;
				}
			}
			int tem = dd[i];
			dd[i] = dd[xiabiao];
			dd[xiabiao] = tem;
			
		}
		
		System.out.println("选择排序：\n升序："+Arrays.toString(dd));
		
		
		

	}

}
```

输出结果：

![Java多数组合并,排序,System.arrayCopy 复制类的使用](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c306b5f35baa-20200918144355094.png "Java多数组合并,排序,System.arrayCopy 复制类的使用")

