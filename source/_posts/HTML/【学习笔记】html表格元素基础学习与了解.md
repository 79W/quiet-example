---
title: 【学习笔记】html表格元素基础学习与了解
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 基础知识
  - 学习笔记
  - 表格元素
excerpt: 学习html内的表格元素对其进行了解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921214913.png'
articleLink: 5143830f1a731915
date: 2020-01-15 08:23:19
---

## 表格元素

### 1. 什么是表格?

现实生活中,表格是比较常见的,例如:报名表,简历

在HTML页面中,也提供了表格相关的一组元素来在HTML页面中实现表格的效果

**表格**是一组由行和列组成的结构化数据集，可以将不同类型的数据集成在一张表中以显示不同类型数据之间存在的某种关系，如下图所示：

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgresize,w_1500-20200921214934694.png)

如上述所示，在表格中存在着一些概念，如下所示：

- 行：水平方向上的一组数据，一般为某个主题的各种数据内容，例如上图中是某个人的各项信息。
- 列：垂直方向上的一组数据，一般为某一项数据，例如上图中的“姓名”一栏。
- 单元格：表格中的显示某一项数据的方块。

### 2. 什么是表格元素?

为了可以在 HTML 页面中实现表格的效果，HTML 提供了一系列表格元素。根据表格中的一些概念，HTML 页面中提供的表格元素如下：

- `` 元素：表示一个表格，作为表格的容器。
- 元素：表示一个表格中的行。
- ` 元素：表示一个表格中的单元格。

### 3. 表的基本结构

表格元素的最为基本的结构为：

1. ``元素：表示 HTML 页面中的一个表格，作为表格的容器。
2. 元素：表示 HTML 页面中一个表格的行。一个表格可以拥有一行或多行。
3. `元素：表示 HTML 页面中的一个表格中一行的单元格。一行可以拥有一个单元格或多个单元格。

表格的基本结构在一般表格应用案例中的分析如下图所示：

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgresize,w_1500-20200921214943102.png)

### 4. 标题单元格

HTML `元素用来定义为一组单元格的标题。该元素的用法与 `元素保持一致，定义在 元素中。

我们可以发现 `元素中的文本内容在浏览器运行时会自动加粗并且居中显示的效果。

` 元素的 scope 属性用来定义与该标题单元格相关联的单元格，是一个枚举类型，具体的值如下所示：

- `row`：表示当前标题单元格关联一行中的所有单元格。
- `col`：表示当前标题单元格关联一列中的所有单元格。
- `rowgroup`：表示当前标题单元格属于某一个行组并与其中所有单元格相关联。
- `colgroup`：表示当前标题单元格属于某一个列组并与其中所有单元格相关联。
- `auto`：由浏览器自动分配。

### 5. 表格的标题

HTML ` 元素用来定义 HTML 页面中表格的标题。该元素一般作为 `` 元素的第一个子级元素出现，同时会显示在表格内容中的最前面，并且会在水平方向居中显示。

如下示例代码展示了 `元素的用法：

```
<table>
  <caption>Color names and values</caption>
  <tr>
    <th scope="col">Name</th>
    <th scope="col">HEX</th>
    <th scope="col">HSLa</th>
    <th scope="col">RGBa</th>
  </tr>
```

### 6. 跨行与跨列

HTML 页面中的表格除了常规的之外，还可以实现跨行或跨列的效果。具体实现方式是通过如下属性来实现的：

- `colspan`属性：用来规定表格单元格可横跨的列数。
- `rowspan` 属性：用来规定表格单元格可横跨的行数。

#### 代码

```
<body>
        <table border="1" cellspacing="10" cellpadding="10">
                <caption>Web前端课程</caption>
                <tr>
                    <th></th>
                    <th scope="col">设计与构建静态网站</th>
                    <th scope="col">JavaScript基础核心语法</th>
                </tr>
                <tr>
                    <th scope="row">是否完成</th>
                    <td colspan="2">已完成</td>
                </tr>
        </table>
</body>
```

#### 补充?

<table border="1"   cellspacing="10"    cellpadding="10">

![1.jpg](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1578990595586-278ff29a-5393-46fc-b175-f9808035707b.jpeg)

#### 遇到问题

如果不加****这句话,表格自身不会出现边框效果

如图:

![3.jpg](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1578990609675-4bbfb3d4-ffad-413b-a800-adcd12a00003.jpeg)



### 7. 长表格

HTML 页面还提供如下 3 个元素来丰富表格：

- 元素：用来定义 HTML 页面中表格的标题单元格的行。
- 元素：用来定义 HTML 页面中表格的主体（表格中真正的数据内容）。
- 元素：用来定义 HTML 页面中表格一组各列的汇总行。

如下示例代码展示了元素、 元素和 元素的用法：

```
<table>
  <caption>Color names and values</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">HEX</th>
      <th scope="col">HSLa</th>
      <th scope="col">RGBa</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Teal</th>
      <td><code>#51F6F6</code></td>
      <td><code>hsla(180, 90%, 64%, 1)</code></td>
      <td><code>rgba(81, 246, 246, 1)</code></td>
    </tr>
    <tr>
      <th scope="row">Goldenrod</th>
      <td><code>#F6BC57</code></td>
      <td><code>hsla(38, 90%, 65%, 1)</code></td>
      <td><code>rgba(246, 188, 87, 1)</code></td>
    </tr>
  </tbody>
</table>
```

上述示例代码运行效果如下图所示：

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgresize,w_1500-20200921214958688.png)

如上图所示， 元素、 元素和 元素不会使表格更易于屏幕阅读器用户访问，也不会造成任何视觉上的改变。然而，它们在应用样式和布局上会起到作用，可以更好地让 CSS 应用到表格上。