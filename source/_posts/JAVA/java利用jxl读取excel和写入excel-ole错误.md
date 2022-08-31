---
title: 'java利用jxl读取excel和写入excel,ole错误'
categories: 技术笔记
tags:
  - Java
  - jxl
excerpt: java处理Excel导入jxl首先需要下载jxl包然后在目录创建个lib文件夹把jxl.jar复制到这个目录文件夹下
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjxxl-20200918150517047.png
articleLink: f568b174545b0228
date: 2019-09-28 17:35:02
---

## java处理Excel

### 导入jxl

首先需要下载jxl包

然后在目录创建个 lib文件夹 把 jxl.jar复制到这个目录文件夹下

![java利用jxl读取excel和写入excel,ole错误](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjxxl-20200918150517047.png "java利用jxl读取excel和写入excel,ole错误")

右键项目 点击 Properties

![java利用jxl读取excel和写入excel,ole错误](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgxiangyou.png "java利用jxl读取excel和写入excel,ole错误")

解析路径

![java利用jxl读取excel和写入excel,ole错误](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaddjxl.png "java利用jxl读取excel和写入excel,ole错误")

然后 就可以使用了！

### jxl读取Excel

```
package com.rj.excle;
/**
 * 
 * @desc java jxl解析excel 读取
 * @author qiaoyue
 * @time 2019-09-28
 * 
 */
import java.io.File;
import java.io.IOException;
import jxl.Cell;
import jxl.Sheet;
import jxl.Workbook;

public class Excel {
	public static void main(String[] args) throws Exception, IOException {
		//实例化文件 解析excel的文件地址
		File file = new File("123.xls");
		//实例化jxl中的 workbook类方法 将文件实例传给他
		Workbook workbook = Workbook.getWorkbook(file);
		//获取工作表sheet1      0已经代表第一个表以此类推
		Sheet sheet = workbook.getSheet(0);
		//获取行数 getColumns
		System.out.println("列 = "+sheet.getColumns());
		//获取列数 getRows
		System.out.println("行 = "+sheet.getRows());
		//循环 行数
		for(int i= 0;i<sheet.getRows();i++) {
			//循环列数 
			for(int j= 0;j<sheet.getColumns();j++) {
				//将行列都传进来 就获取了全部的单元格
				//获取单元格属性值  行，列
				Cell cell=sheet.getCell(i,j);
				//打印这个 单元格里面的值打印出来
				System.out.print(cell.getContents()+"\t");
			}
			//换行
			System.out.println();
		}
		//关闭这个资源
		workbook.close();
	}
	
}
```

### **可能会报的错误**

##### Unable to recognize OLE stream

1.使用jxl方式读取，可能只能支持xls格式的文件，对于xlsx格式就不再支持

2.如果是从网站导出的excel文件，有的网站比较坑，导出的并不是标准格式的excel，而是将html改扩展名为xls的“伪”excel文件。当用excel打开这类文件时，会弹窗提示其**“扩展名和文件类型不匹配**”是否还要打开。 而且，使用文本编辑器打开，会发现这个所谓xls文件其实是xml标签的文件。

##### 解决方案

就是把excel表的后缀改为 xls 即可

![java利用jxl读取excel和写入excel,ole错误](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgcuowuole-20200918150807741.png "java利用jxl读取excel和写入excel,ole错误")

### java写入excel

```
package com.rj.excle;
/**
 * @desc java利用jxl写入excel
 * @author qiaoyue
 * @time 2019-09-28
 */

import java.io.File;
import java.io.IOException;

import jxl.Workbook;
import jxl.write.Label;
import jxl.write.WritableSheet;
import jxl.write.WritableWorkbook;
import jxl.write.WriteException;
import jxl.write.biff.RowsExceededException;

public class Excel01 {

	public static void main(String[] args) throws IOException, RowsExceededException, WriteException {
		//实例化文件 （创建excel文件）
		File file = new File("123.xls");
		//检测是否存在文件 不存在就创建 存在就抛出异常
		file.createNewFile();
		//创建工作表
		WritableWorkbook workbook =Workbook.createWorkbook(file);
		//创建sheet 名字和第几个
		WritableSheet sheet = workbook.createSheet("乔越",0);
		//循环 单元格 
		for(int i = 0;i<10;i++) {
			for(int j = 0;j<10;j++) {
				//设置单元格
				Label label = new Label(i,j,"79bk+"+i+j);
				//添加到单元格
				sheet.addCell(label);
			}
		}
		//写入
		workbook.write();
		//关闭资源
		workbook.close();
		System.out.println("写入成功");

		
	}

}
```

[资源下载](https://www.lanzous.com/i6ggxsj)