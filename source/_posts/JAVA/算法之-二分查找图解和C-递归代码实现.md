---
title: 算法之-二分查找图解和C++递归代码实现
categories: 技术笔记
tags:
  - c
  - 二分查找
  - 算法
  - 递归
excerpt: 小时候玩的猜数字游戏：我心里想一个数字你去猜然后给你个区间（1-100）你每猜一个我都会告诉你是大了还是小了 这样你就知道那个区间去猜了
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgerfencha.png'
articleLink: 96a3c8404aa85724
date: 2019-07-24 00:21:57
---

> 思维理解

 小时候玩的猜数字游戏：我心里想一个数字你去猜然后给你个区间（1-100）你每猜一个我都会告诉你是大了还是小了 这样你就知道那个区间去猜了 （区间就会缩小）

> 下面的示例说明了二分查找的工作原理。

![算法之-二分查找图解和C++递归代码实现](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgerfencha.png "算法之-二分查找图解和C++递归代码实现")

* * *

> 算法讲解

 二分查找要求序列本身是**有序**的。因此对于无序的序列，我们要先对其进行排序。现在我们手头有一有序序列：  
​ int list\[10\] = {1,2,3,4,5,6,7,22,23,44};，则二分查找的过程为：

*   设置三个索引：low指向数组**待查范围**的起始元素，high指向数组**待查范围**的最后一个元素，middle=(low+high)/2。开始时待查范围为整个数组。
    
*   比较list\[middle\]与查找元素的大小关系：
    
    *   如果list\[middle\]等于查找元素，则查找成功
    *   如果list\[middle\]大于查找元素，则说明待查元素在数组的前半部分，此时缩小**待查范围**，令end = middle-1
    *   如果list\[middle\]小于查找元素，则说明待查元素在数组的后半部分，此时缩小**待查范围**，令start = middle +1
*   重复执行前面两步，直到list\[middle \] 等于查找元素则查找成功或low>hig查找失败。
    

![算法之-二分查找图解和C++递归代码实现](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgerfenn.png "算法之-二分查找图解和C++递归代码实现")

 可见，每一次元素比较都可以把待查范围缩小1/2，因此二分查找的时间复杂度为o(logn)。

> 注意的点

1.  循环的判定条件是：`low<high`
2.  为了防止数值溢出: `middle = (low+high)/2`
3.  当 `list[middle]`不等于`mun`时，`high = middle - 1`或`low = middle + 1`

> 代码演示（递归算法）

```cpp
#include <iostream>
//二分查找算法实现
using namespace std;
//定义函数 传入参数 mun要查找的数 low 起点 high终点 list指针列表
int lookup(int mun,int low,int high ,int *list){
    //求中间值（索引）
    int middle = (low+high)/2;
    //将z中间值的索引 的值给 listmiddle
    int listmiddle = list[middle];
    //判断 也是结束递归的条件
    // 当起始索引 大于 重点的时候返回值 停止
    if(low > high){
        return -1;
    }
    //判断中间值等于的时候直接返回
    else if(listmiddle == mun){
        return middle;
    }
    //如果 中间值 小于 要查找的值 就将起点 low 等于中间值
    //因为 从起点到中间值都小于要查找到值 所以 都不要了
    //再次进行调用函数
    //进入递归
    else if(listmiddle < mun){
        low = middle+1;
        return lookup(mun,low,high,list);
    }
    //判断中间的值 大于 要查找的值 就将 终点 high 等于 中间索引
    //并减去1。因为中间值 到 终点值 都大于 要查找的值
    //再次进入递归
    else if(listmiddle > mun){
        high = middle -1;
        return lookup(mun, low , high , list);
    }
    //int 要有返回
    return 0;
};
//主函数
int main() {
    //先定义一个列表 按从小到大的 顺序排列好
    int list[10] = {1,2,3,4,5,6,7,22,23,44};
    //将函数的返回值 赋给 run
    int run = lookup(2,0,10,list);
    cout<<run;
    return 0;
}
```

如有不足欢迎补充

[资源下载](https://www.lanzous.com/i54p9ng)