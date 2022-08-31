---
title: java如何复制一个文件夹内的内容并创建新的文件
categories: 技术笔记
tags:
  - Java
  - 作业练习
  - 基础知识
excerpt: 有一个名字叫123.txt的文本文件里面存放了1一句话“我喜欢编程123”将这个文件复制一份并最终
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b72d4c77879c.png'
articleLink: a8abe7c71bbc3614
date: 2018-08-14 21:11:36
---

问题：有一个名字叫123.txt的文本文件，里面存放了1一句话“我喜欢编程：123”，将这个文件复制一份并最终将复制后的结果(fz.txt)存放在D盘的cms文件夹下

```
package com.sj.zy;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Zy02 {
	private static String nu11;
	public static void main(String[] args) throws IOException {
		BufferedReader br =new BufferedReader(new FileReader("d:/123.txt"));
		File file=new File("D:/cms");
		if(!file.isDirectory()) {
			file.mkdir();
		}
		BufferedWriter bw =new BufferedWriter (new FileWriter("D:/cms/fz.txt"));
		String line= nu11;
		while((line = br.readLine())!=nu11) {
			bw.write(line);
		}
		bw.close();
		br.close();

	}

}
```

![java如何复制一个文件夹内的内容并创建新的文件](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b72d4c77879c.png "java如何复制一个文件夹内的内容并创建新的文件")![java如何复制一个文件夹内的内容并创建新的文件](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b72d4d08ac51-20200918151323375.png "java如何复制一个文件夹内的内容并创建新的文件")