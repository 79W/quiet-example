---
title: javaSocket实现简单客户端与服务端交互
categories: 技术笔记
tags:
  - Java
  - Socket
  - 聊天室
excerpt: 主要学习java.net下ServerSocket和Socket两个核心类进行客户端与服务端的简单交互
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200918143746.png'
articleLink: e2a49762b9da2408
date: 2019-10-08 17:11:24
---

## 简要说明：

主要学习

java.net下 ServerSocket 和 Socket 两个核心类

进行客户端与服务端的简单交互！实现通讯

自己也是初学欢迎交流。。。。

## 服务端

```
package com.bj.rj;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;

/**
 * @desc 服务端
 * @author qiaoyue
 * @time 2019-10-08
 */
public class FuWuDuan {
	public static void main(String[] args) throws IOException {
	//创建服务 端口号1002
	ServerSocket ssocket = new ServerSocket(1002);
	System.out.println("开启服务等待连接");
	//如果多个 客户端连接就 while循环就可以循环以下的内容
	//等待客户端的连接
	Socket socket = ssocket.accept();
	System.out.println("客户端连接成功");
	//获取服务端给发过来的消息
	//获取输入流
	//InputStream操作的是字节、实现类分为几个方向、内存、本地文件、网络、其他线程管道
	//getInputStream 返回此套接字的输入流
	InputStream inputs = socket.getInputStream();
	//读取 字符输入流
	//InputStreamReader 能将字节流输出为字符流
	InputStreamReader inputsr = new InputStreamReader(inputs);
	//加入缓冲区
	//BufferedReader 提供通用的缓冲方式文本读取
	BufferedReader bufferedReader = new BufferedReader(inputsr);
	//readLine读取一个文本行
	//从字符输入流中读取文本，缓冲各个字符，从而提供字符、数组和行的高效读取。
	String neirong = bufferedReader.readLine();
	//输出字符
	System.out.println("客户端发来的内容："+neirong);
	}
}
```

## 客户端

```
package com.bj.rj;

import java.io.IOException;
import java.io.OutputStream;
import java.io.PrintStream;
import java.net.Socket;
import java.net.UnknownHostException;
import java.util.Scanner;

/**
 * @desc 客户端
 * @author qiaoyue
 * @time 2019-10-08
 */

public class KeHuDuan {
	public static void main(String[] args) throws UnknownHostException, IOException {
		//连接服务端
		Socket socket = new Socket("127.0.0.1",1002);
		//发送消息 给服务端
		//getOutputStream 返回此套接字的输出流。
		OutputStream output = socket.getOutputStream();
		//输入字符串
		System.out.println("发送话：");
		Scanner cin = new Scanner(System.in);
		String neirong = cin.next();
		//内容写进去对象
		PrintStream printStream = new PrintStream(output);
		//发送
		printStream.println(neirong);
		
	}

}
```

