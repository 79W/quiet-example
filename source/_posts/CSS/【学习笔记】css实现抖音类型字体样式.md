---
title: 【学习笔记】css实现抖音类型字体样式
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 基础知识
  - 学习笔记
excerpt: css属性text-shadow属性的学习进行完成阴影式的字体
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921213016.png'
articleLink: c9c1a18c9e6c4006
date: 2020-01-06 10:58:40
---



## 分析

1. 首先要有一个字
2. 对这个字进行 添加阴影的操作
3. 前景色是红色后景色是蓝色
4. 也就是字的左上是红色阴影 右下这样是红色
5. 需要使用 `text-shadow` css阴影样式进行调试即可

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgp4nczFDYiy1Pus3.png)

## 代码实现

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>字体调整</title>
		<style>
		body{
			background: #333;
		}
		h1{
			color: #fff;
			text-shadow: #f03740 -0.5px -1px, 1px 0px #2addfd;
		}
		</style>
	</head>
	<body>
		<h1>changeq</h1>
	</body>
</html>
```

## text-shadow 参数介绍

```css
/* x轴 | y轴 | 模糊度 | 颜色 */
/* 加个颜色然后按照xy轴去偏移 然后加上模糊度 不脱离本体*/
text-shadow: 1px 1px 2px black; 

/* 颜色 | x轴 | y轴 | 模糊度 */
/* 加个颜色然后按照xy轴去偏移 然后加上模糊度 不脱离本体*/
text-shadow: #fc0 1px 0 10px; 

/* x轴 | y轴 | 颜色 */
/* 加个颜色然后按照xy轴去偏移 */
text-shadow: 5px 5px #558abb;

/* 颜色 | x轴 | y轴 */
/* 加个颜色然后按照xy轴去偏移 */
text-shadow: white 2px 5px;

/* x轴 | y轴
/* 和本字体大小颜色相同 但是偏移了 */
text-shadow: 5px 10px;
```

参考：[mdn](https://developer.mozilla.org/zh-CN/docs/Web/CSS/text-shadow)

