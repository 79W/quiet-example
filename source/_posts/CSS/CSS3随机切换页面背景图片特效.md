---
title: CSS3随机切换页面背景图片特效
categories: 项目案例
tags:
  - HTML
  - css3
excerpt: 定义和用法 css3的随机背景图片淡入淡出切换特效 通过 @keyframes 规则，您能够创建动画。创建动
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bed879291c10.png'
articleLink: 1569e3425d300715
date: 2018-11-15 22:55:07
---

## 定义和用法

css3的随机背景图片淡入淡出切换特效

通过 @keyframes 规则，您能够创建动画。

创建动画的原理是，将一套 CSS 样式逐渐变化为另一套样式。

在动画过程中，您能够多次改变这套 CSS 样式。

以百分比来规定改变发生的时间，或者通过关键词 “from” 和 “to”，等价于 0% 和 100%。

0% 是动画的开始时间，100% 动画的结束时间。

为了获得最佳的浏览器支持，您应该始终定义 0% 和 100% 选择器。

注释：请使用动画属性来控制动画的外观，同时将动画与选择器绑定。

核心css部分(记得切换图片地址)

* * *

```
body {
	background: #000;
	background-attachment: fixed;
	word-wrap: break-word;
	-webkit-background-size: cover;
	-moz-background-size: cover;
	background-size: cover;
	background-repeat: no-repeat
}

ul {
	list-style: none
}

.cb-slideshow li:nth-child(1) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow li:nth-child(2) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow li:nth-child(3) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow li:nth-child(4) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow li:nth-child(5) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow li:nth-child(6) span {
	background-image: url(http://www.79bk.cn/ip/api.php)
}

.cb-slideshow,.cb-slideshow:after {
	position: fixed;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	z-index: -2
}

.cb-slideshow:after {
	content: ''
}

.cb-slideshow li span {
	width: 100%;
	height: 100%;
	position: absolute;
	top: 0;
	left: 0;
	color: transparent;
	background-size: cover;
	background-position: 50% 50%;
	background-repeat: none;
	opacity: 0;
	z-index: -2;
	-webkit-backface-visibility: hidden;
	-webkit-animation: imageAnimation 36s linear infinite 0s;
	-moz-animation: imageAnimation 36s linear infinite 0s;
	-o-animation: imageAnimation 36s linear infinite 0s;
	-ms-animation: imageAnimation 36s linear infinite 0s;
	animation: imageAnimation 36s linear infinite 0s
}

.cb-slideshow li:nth-child(2) span {
	-webkit-animation-delay: 6s;
	-moz-animation-delay: 6s;
	-o-animation-delay: 6s;
	-ms-animation-delay: 6s;
	animation-delay: 6s
}

.cb-slideshow li:nth-child(3) span {
	-webkit-animation-delay: 12s;
	-moz-animation-delay: 12s;
	-o-animation-delay: 12s;
	-ms-animation-delay: 12s;
	animation-delay: 12s
}

.cb-slideshow li:nth-child(4) span {
	-webkit-animation-delay: 18s;
	-moz-animation-delay: 18s;
	-o-animation-delay: 18s;
	-ms-animation-delay: 18s;
	animation-delay: 18s
}

.cb-slideshow li:nth-child(5) span {
	-webkit-animation-delay: 24s;
	-moz-animation-delay: 24s;
	-o-animation-delay: 24s;
	-ms-animation-delay: 24s;
	animation-delay: 24s
}

.cb-slideshow li:nth-child(6) span {
	-webkit-animation-delay: 30s;
	-moz-animation-delay: 30s;
	-o-animation-delay: 30s;
	-ms-animation-delay: 30s;
	animation-delay: 30s
}

@-webkit-keyframes imageAnimation {
	0% {
		opacity: 0;
		-webkit-animation-timing-function: ease-in
	}

	8% {
		opacity: 1;
		-webkit-transform: scale(1.05);
		-webkit-animation-timing-function: ease-out
	}

	17% {
		opacity: 1;
		-webkit-transform: scale(1.1) rotate(0)
	}

	25% {
		opacity: 0;
		-webkit-transform: scale(1.1) rotate(0)
	}

	100% {
		opacity: 0
	}
}
```

当然还是需要配合HTML代码的  
HTML部分(其中的文字部分和<li>的数量是可以随意更改的)

```
<ul class="cb-slideshow">
	<li>
		<span>乔越</span></li>
	<li>
		<span>博客</span></li>
	<li>
		<span>一个</span></li>
	<li>
		<span>专业的</span></li>
	<li>
		<span>分享 </span></li>
	<li>
		<span>教程</span></li>
</ul>
```

注意：<li>的数量要和css对应上，并且至少要两个以上。另外ie浏览器是不支持CSS3的特效

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5bed879291c10.png)