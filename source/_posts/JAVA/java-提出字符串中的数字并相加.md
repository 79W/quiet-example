---
title: java提出字符串中的数字并相加
categories: 技术笔记
tags:
  - Java
  - 基础知识
  - 正则表达式
excerpt: 输入一行字符串（少于80个字符），求其中数字的和。输入数据包含一行字符串中间存在多于两个数字在一行上输出字符串中数字的和，输出完后，不要回车换行。
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img190530114619-20200918141114232.png
articleLink: b1e1ff3fd2c30108
date: 2018-08-08 19:39:01
---

问题：

输入一行字符串（少于80个字符），求其中数字的和。

输入数据包含一行字符串，中间存在多于两个数字。

在一行上输出字符串中数字的和，输出完后，不要回车换行。

```
import java.util.Scanner;
public class Test {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String line = scanner.nextLine();
        int result = 0;
        for (int i = 0; i < line.length(); i++) {
            if (Character.isDigit(line.charAt(i))) {
                result = result + Integer.parseInt(line.charAt(i) + "");
            }
        }
        System.out.print(result);
    }
}
```

`例：输入：123`  
`输出：6`  
`兼容字符串中存在字母的情况：`  
`输入：12ab3`  
`输出：6`

![java 提出字符串中的数字并相加](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img190530114619-20200918141114232.png "java 提出字符串中的数字并相加")