---
title: Java经典算法之选择排序
categories: 技术笔记
tags:
  - Java
  - 个人笔记
  - 经典算法
  - 选择排序
excerpt: 选择排序（Selection sort）是一种简单直观的排序算法。
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c3067f0be8b8.png'
articleLink: 27664362b1851705
date: 2019-01-05 16:19:17
---

选择排序（Selection sort）是一种简单直观的排序算法。

它的工作原理如下：

首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

选择排序的主要优点与数据移动有关。如果某个元素位于正确的最终位置上，则它不会被移动。选择排序每次交换一对元素，它们当中至少有一个将被移到其最终位置上，因此对n个元素的表进行排序总共进行至多n-1次交换。在所有的完全依靠交换去移动元素的排序方法中，选择排序属于非常好的一种。

排序过程：

![Java经典算法之选择排序](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c30681e8f759.gif "Java经典算法之选择排序")

![Java经典算法之选择排序](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5c3067f0be8b8.png "Java经典算法之选择排序")代码实现：

```
/*选择排序*/
package xuanzepaixu;

import java.util.Arrays;

public class xuanzepaixu {

	public static void main(String[] args) {
		//定义静态数组
		int[] pai = {13,16,22,533,78,53,221,785,};
		//循环 轮数
		for(int i=0;i<pai.length;i++) {
			//保存下标 	随着外层往后移
			int xiabiao = i;
			//比较下标 
			for(int j=i+1;j<pai.length;j++) {
				//判断下标对应的大小
				if(pai[j]<pai[xiabiao]) {
					//下标更改新位置
					xiabiao = j;
				}
			}
			//进行交换
			int tem = pai[i];
			pai[i] = pai[xiabiao];
			pai[xiabiao] = tem;
			
		}
		
		System.out.println(Arrays.toString(pai));
	}

}

```


