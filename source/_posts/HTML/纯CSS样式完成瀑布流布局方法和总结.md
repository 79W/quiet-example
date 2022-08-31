---
title: 纯CSS样式完成瀑布流布局方法和总结
categories: 技术笔记
tags:
  - HTML
  - CSS
  - 瀑布流
excerpt: 使用css样式flex和columns分别完成瀑布流布局
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20210320151948.png'
articleLink: 1db4d0a1
date: 2021-03-20 15:22:09
---

实现瀑布流有很多种办法我们

每种办法都适用不同的场景大家自己按照需求来选择

#### 第一种：使用 `column` 来进行实现瀑布流

```
//用法
columns: column-width column-count;
```

*column-width*：列的宽度

*column-count*：列数

我们可以规定当前容器分为几列从而实现瀑布流

##### 实现

我们首先定义一个`DIV` 为主要容器

```
<div id="content">
// 主要容器
</div>
```

然后我们在里面写上内容

```
<div id="content">
  <div class="item">
  	内容一
  </div>
  <div class="item">
  	内容二
  </div>
  <div class="item">
  	内容三
  </div>
</div>
```

我们只需要将 ID 加上 `column` 样式就可以了

```
#content {
  width: 500px;
  height: 10000px;
  background: red;
  column-count: 2; // 重要样式
  padding: 30px;
}
```

我这里是将这个容器分为了两列

![瀑布流](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/chongxinpubuliubujudiyige.png)

#### 第二种：多列并排的布局方法

![瀑布流布局](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/paibuliedetupianyanshi.png)

我们使用代码实现这种大块布局后将内容向里面进行填充即可

并且 **小红书** 也是在使用这种布局方式说明还是不错的哦～

![小红书布局](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/xiaohongshubujufangshiyancha.png)

所以我自己的项目也在使用这个布局

![流式布局](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/xiaongappxiaochengxuliushibuju.png)

首先我们还是定义个DIV进行总体包裹

并且在里面定义两个 div 进行左右布局

```
<div id="content">
  // 左面
	<div class='item-left'></div>
	// 右面
	<div class='item-right'></div>
</div>
```

我们使用 左浮动 也可以实现那种布局

这里我就是用 `flex` 布局进行调整可控性更大一些

```
#content{
	display: flex;
}
```

就可以了 然后我们在定义 左右里面的内容样式就可以了这里你自己进行

这种布局的好处就是我们可以自己定义 内容块

比如第一种是 竖着进行排列那么当你新增内容的时候你就会发现排列不对劲 

而使用第二种布局方式的时候我们 就可以自己进行 排序想放什么就放什么很是自由

所以第二种我才用的是横向排序

![流式布局](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/neirongyanshihengxiangbujuk.png)

还有用`flex`复杂的布局 具体看自己需求什么根据自己的需求来进行选择解决方案



