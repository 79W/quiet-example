---
title: 【学习笔记】html结构化元素html标签的学习html5的学习
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 基础知识
  - 学习笔记
excerpt: 对html基础标签的学习与认识html5的了解性的学习
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921214913.png'
articleLink: 8776c6c50bfc0905
date: 2020-01-05 15:47:09
---

## 什么是结构化元素

**结构化元素**就是指 HTML 元素中具有明确含义和作用的元素，例如 <p>元素表示段落。相对于 HTML 4.01 版本而言，HTML5 版本新增了一系列结构化元素。

HTML4.01版本的结构化元素

| 名称          | 代码            |
| ------------- | --------------- |
| 标题元素      | `<h1>`~`<h6>`   |
| 段落元素      | `<p>`           |
| 粗体元素      | `<b>`           |
| 斜体元素      | `<i>`           |
| 上标/下标元素 | `<sup>`/`<sub>` |
| 换行符        | `<br>`          |
| 水平线元素    | `<hr>`          |

HTML5新增的结构元素

- `<article>` 元素
- `<section>` 元素
- `<nav>` 元素
- `<aside>` 元素
- `<header>` 元素
- `<main>` 元素
- `<footer>` 元素

### 标题元素

```html
<h1>这是标题一</h1>
<h2>这是标题二</h2>
<h3>这是标题三</h3>
<h4>这是标题四</h4>
<h5>这是标题五</h5>
<h6>这是标题六</h6>
```

关于标题元素在开发中的使用时需要注意如下要点：

- 对于搜索引擎抓取 HTML 页面的内容，`<h1>` 元素仅次于`<title>`元素。为了可以被搜索引擎抓取，建议一个 HTML 页面只包含一个 `<h1>` 元素。
- 不要为了减小标题的字体而使用低级别的标题， 而是通过使用 CSS font-size 属性实现。
- 避免跳过某级标题，始终要从 `<h1>` 元素开始，依次使用 `<h2>` 元素、`<h3>` 元素、… …

### 段落元素

HTML `<p>` 元素表示一个段落，该元素通常呈现出当前段落的文本与其他段落的文本之间会以空白进行隔离。如下示例代码展示了 `<p>` 元素的用法：

```
<p>这是第一个段落内容.</p>
<p>这是第二个段落内容.</p>
```

**注意**：如果想要改变段落之间的间隔空间，建议使用 CSS margin 属性实现，而不是插入空的段落元素或者 ` br` 元素。

### 粗体元素

HTML `<b>` 元素用来定义需要提醒注意的内容。该元素在过去被认为是**粗体元素**，因为绝大多数浏览器解析该元素呈现的是粗体效果。

> 如果不是出于语义目的而使用 `<b>` 元素，那么让文本显示粗体更好的方式是使用将 CSS 的 font-weight 属性设置为 bold。

`<b>` 元素的应用场景例如摘要中的**关键字、评论中的产品名称**，或其他**典型的应该加粗显示的文字**。

以前 `<b>` 元素的含义就是让文本变成粗体效果。但从 HTML4 版本开始，不赞成标签表示带样式信息，于是 `<b>` 元素的含义发生了变化。

### 斜体元素

HTML` <i>` 元素用来定义表现因某些原因需要区分普通文本的一系列文本，例如技术术语、外文短语或是小说中人物的思想活动等。浏览器运行解析 `<i>` 元素一般呈现的效果是斜体。

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galpwavfnzj20qu071t90.jpg)

### 上标/下标元素

HTML `<sup>` 元素表示为上标元素，HTML `<sub>` 元素表示为下标元素。这两个元素的特点如下：

`<sup>` 元素定义的文本内容与主体内容相比，显示更高更小。
`<sub>` 元素定义的文本内容与主体内容相比，显示更低更小。
如下示例代码展示了 `<sup>` 元素和 `<sub>` 元素的用法：

```
<p>你将在下学期学习到 E=MC<sup>2</sup> 公式。</p>
<p>水的化学成分叫做H<sub>2</sub>O。</p>
```

<p>你将在下学期学习到 E=MC<sup>2</sup> 公式。</p>
<p>水的化学成分叫做H<sub>2</sub>O。</p>

### 换行符

HTML `<br>` 元素会在 HTML 页面中生成一个换行符。编写在 `<br>` 元素后的文本内容会呈现在第二行中。

```html
<span>哈哈哈</span>
<span>呵呵呵</span>

<span>哈哈哈</span>
<br>
<span>呵呵呵</span>

```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakledrw18j207q02owe9.jpg)

### 水平线元素

HTML `<hr>` 元素用来表示段落元素之间的主题转换。在较早版本的 HTML 中，`<hr>` 元素表示一个水平线，并且浏览器运行解析也是水平线效果。但目前 `<hr>` 元素被定义为语义上的，而不是表现上。

如下示例代码展示了 `<br>` 元素的用法：

```html
<p>§1: 这是一个段落内容.</p>
<hr>
<p>§2: 这是另一个段落内容.</p>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gaklhf69vxj20mo034dfo.jpg)

## HTML5 版本的结构化元素

### ``article`` 元素

HTML `article` 元素用来定义 HTML 页面中的可独立分配或可复用结构，例如论坛的帖子、新闻网站的文章等。

如下示例代码展示了 `article` 元素的用法：

```
<article>
	<h1>前端开发</h1>
	<p>前端开发现在已经是软件开发领域中的主流。</p>
	<p><small>版权归属 *** 公司所有。</small></p>
</article>
```

上述示例代码运行效果如下图所示：

![0109.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img15b3b160-23d4-461d-a837-87832dc56407.png)

### ``section`` 元素

HTML `section` 元素用来定义 HTML 页面中的独立部分，该独立部分没有更具体的的语义元素来描述该元素。

如下示例代码展示了 `section` 元素的用法：

```
<section>
  <h1>前端开发</h1>
  <p>前端开发现在已经是软件开发领域中的主流。</p>
</section>
```

上述示例代码运行效果如下图所示：

![0110.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img6834267e-61af-4496-b59e-042dc030533a.png)

关于 `section` 元素在开发中的使用时需要注意如下要点：

- 一般通过是否包含一个标题元素（  `<h1>` ~<h6> ``）作为子级元素来识别每一个 `` 元素。

- 如果元素内容可以分为几个部分的话，应该使用 `` 元素 而不是 `` 元素。

  ```
  <section>
  	<article>
  		<p>这是第一个测试内容.</p>
  	</article>
    <article>
      <p>这是第二个测试内容.</p>
    </article>
  </section>
  ```

- 不要将 `` 元素作为一个普通容器使用，这应该是 `` 元素的用法。

#### `<nav>` 元素

HTML `<nav>` 元素用来定义 HTML 页面中的导航链接，比较常见的是菜单，目录和索引。

```html
<nav>
	<ul>
		<li><a href="#">设计与构建静态网站</a></li>
		<li><a href="#">JavaScript基础核心语法</a></li>
		<li><a href="#">DOM编程艺术</a></li>
	</ul>
</nav>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakltaf2tuj207u02p0sl.jpg)

> 上述示例所示效果是 `<ul>` 列表元素的呈现效果，因为没有为 `<nav>` 元素设定任何 CSS 样式，默认是没有任何效果的。

关于` <nav>` 元素在开发中的使用时需要注意如下要点：

并不是所有的链接都必须使用 `<nav>` 元素，该元素只用于将一些热门的链接放入导航栏。
一个 HTML 页面可能存在多个 `<nav>` 元素。

#### `<aside>` 元素

HTML `<aside>` 元素用来定义一个和 HTML 页面中其余内容几乎无关的内容，被认为是独立于该内容的一部分并且可以被单独的拆分出来而不会使整体受影响。通常比较常见的是侧边栏或者标注框。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>aside元素</title>
  <style>
    aside {
      width: 40%;
      padding-left: .5rem;
      margin-left: .5rem;
      float: right;
      box-shadow: inset 5px 0 5px -5px #29627e;
      font-style: italic;
      color: #29627e;
    }
  </style>
</head>

<body>
  <article>
    <p>
      迪斯尼电影<cite>海的女儿</cite>（<cite>The Little Mermaid</cite>）于 1989 年首次登上银幕。
    </p>
    <aside>
      在首次发行期间，该片便收获了 8700 万美元的票房。
    </aside>
    <p>
      更多有关该电影的信息…
    </p>
  </article>
</body>

</html>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gaklw0hvrfj20tt02ywei.jpg)



#### `<header>` 元素

HTML `<header>` 元素用来定义 HTML 页面中的具有引导和导航作用的内容，比较常见的是 Logo、搜索框、作者名称等。

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galqbil3tsj20v00ccq3y.jpg)

```html
<header>
  <h1>主页标题</h1>
</header> 
<article>
  <header>
    <h2>前端开发</h2>
    <p>前端开发现在已经是软件开发领域中的主流。</p>
  </header>
  <p>这是第一个测试内容.</p>
  <p><a href="#">更多内容....</a></p>
</article>
```

> 一个 HTML 页面并没有限制只能出现一个 `<header>` 元素，可以为每个内容区块添加一个 `<header>` 元素。

#### `<main>` 元素

HTML `<main>` 元素用来定义 HTML 页面中的主要内容。主内容区块指与页面标题或主要功能直接相关的内容。这部分内容在HTML页面中应当是独一无二的，不包含任何任何重复的内容。

> 关于 `<main>` 元素在开发中的使用时需要注意如下要点：
>
> 一个 HTML 页面中只能出现一个 `<main>` 元素。
> `<mian>` 元素不能出现在 `<article>` 元素、`<aside>` 元素、`<nav>` 元素、`<header>` 元素和 `<footer>` 元素的内部。

```html
<main>
	<h2>Coder 比赛</h2>
	<p>目前正在紧张进行中...</p>
</main>
```

![6.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galqe69gylj20u508tt94.jpg)

#### `<footer>` 元素

HTML `<footer>` 元素用来定义 HTML 元素中的一个章节内容或根元素的页脚。一个页脚通常包含该章节作者、版权数据或文档相关链接等信息。

关于` <footer>` 元素在开发中的使用时需要注意 `<footer>`元素中的作者信息应该包含在 `<address>` 元素中。

```html
<footer>
	<ul>
		<li>版权信息</li>
		<li>站点地图</li>
		<li>联系方式</li>
	</ul>
</footer>
```

![7.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galqetg1ktj20sh0aawex.jpg)

### 空白

当浏览器运行并解析 HTML 页面时，遇到两个或两个以上的连续空格时，只将其显示为一个空格效果。这种特性叫做**白色空间折叠**。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<p>多个空格        但只会显示一个空格</p>
	</body>
</html>

```

![8.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galqh4fa14j20sq09ejro.jpg)

### 转义字符

| **原义字符** | **描述**          | **转义字符** |
| ------------ | ----------------- | ------------ |
| ` `          | 空格              | `&nbsp;`     |
| `<`          | 小于号            | `&lt;`       |
| `>`          | 大于号            | `&gt;`       |
| `&`          | 和号              | `&amp;`      |
| `"`          | 引号              | `&quot;`     |
| `©`          | 版权（copyright） | `&copy;`     |
| `®`          | 注册商标          | `&reg;`      |
| `™`          | 商标              | `&trade;`    |
| `×`          | 乘号              | `&times;`    |
| `÷`          | 除号              | `&divide;`   |

## 语义化元素

**语义化元素**与结构化元素类似，都是具有具体含义的元素，区别在于语义化元素更多定义一个单词、一行内容的语义或样式。

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>语义化元素</title>
	</head>
	<body>
		<!-- 加粗元素 -->
		<p><strong>说明：</strong>HTML声明并不是一个HTML元素。</p>
		<!-- 强调元素 -->
		<p>在HTML中，有些内容是需要<em>着重</em>来阅读的.</p>
		<!-- 引用元素 -->
		<blockquote>
		  <p>
		    这是一段用来引用的文本内容.
		  </p>
		</blockquote>
		<p>说明：<q>声明并不是一个HTML元素。</q></p>
		<!-- 引文元素 -->
		<p>更多信息可以阅读<cite>[ISO-0000]</cite>.</p>
		<!-- 定义元素 -->
		<p>
		  <dfn id="def-internet">The Internet</dfn> is a global system of interconnected networks that use the Internet Protocol Suite (TCP/IP) to serve billions of users worldwide.
		</p>
		<!-- 地址元素 -->
		<address>
		  <a href="mailto:jim@rock.com">jim@rock.com</a><br>
		  <a href="tel:+13115552368">(311) 555-2368</a>
		</address>
		<!-- 内容修改元素 -->
		<p>前端好<del>难</del><ins>容易</ins>学习啊</p>
	</body>
</html>

```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galqudohm0j213l0lz417.jpg)

如下列表所示展示了部分语义化元素：

| 名称         | 代码                    | 释义                                                         | 效果                                                         |
| ------------ | ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 加粗元素     | `<strong>`              | HTML 页面中的十分重要的文本内容                              | 浏览器运行 HTML 页面默认呈现的是粗体效果                     |
| 强调元素     | `<em>`                  | HTML 页面中的需要用户着重阅读的文本内容                      | 浏览器运行 HTML 页面默认呈现的是斜体效果                     |
| 引用元素     | `<blockquote>` 和 `<q>` | HTML `blockquote` 元素用来定义 HTML 页面中的标记较长的引用内容，一般浏览器解析后会对其进行缩进。HTML `q` 元素用来定义 HTML 页面中的较短的引用内容，浏览器解析后会在其两侧使用引号包裹。 | 代码1                                                        |
| 引文元素     | `<cite>`                | HTML 页面中的对一个作品的引用，该元素必须包含引用作品的符合简写格式的标题或者 URL，可能是一个根据添加引用元数据的约定的简写形式。 | 浏览器解析后会呈现斜体效果                                   |
| 定义元素     | `<dfn>`                 | 定义 HTML 页面中的术语                                       | 有些浏览器会将 `<dfn>` 元素解析后呈现为斜体，但 Safari 和 Chrome 浏览器则不会改变其样式。 |
| 地址元素     | `<address>`             | HTML 页面中提供了某个人或某个组织的联系信息                  | 浏览器解析后会呈现为斜体效果                                 |
| 内容修改元素 | `<del>` 和 `<ins>`      | HTML `del`元素用来定义 HTML 页面中删除的文字内容，HTML `ins` 元素用来定义 HTML 页面中插入的文字内容。 | 代码2                                                        |

代码1

```html
<blockquote>
  <p>
    这是一段用来引用的文本内容.
  </p>
</blockquote>
<p>说明：<q>声明并不是一个HTML元素。</q></p>
```

![QQ截图20200104164342.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakmglsqzsj20a002p744.jpg)

引文元素

代码2

```html
<p>前端好<del>难</del><ins>容易</ins>学习啊</p>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakmkd01qtj205f01bq2p.jpg)

## 字体样式

### 设置字体

CSS `font-family` 属性通过一个字体名或字体族名组成的列表来设置 HTML 页面中的字体。

```html
p {
  font-family: 宋体;
}
```

#### 设置一个字体名或字体族名

两种设置方法。并不推荐只为 font-family 属性设置一个值。因为这样的话，浏览器运行 HTML 页面时很可能找不到这种字体或字体族，从而导致字体不能按照预期的效果进行展示。

##### 字体族名字

字体族名必须是**有效**的。具体可以划分成如下几种情况：

- 如果字体族名是一个或多个合法标识串构成的话，是可以没有引号的。
- 如果字体族名是一个或多个非合法标识串构成的话，是需要使用引号的。
- 在没有带引号的字体族名的开头是不能使用标点符号字符和数字字符的。

##### 通用字体族名字

通用字体族名是用来在指定的字体不可用时给出较好的字体。通用字体族名都是关键字，所以不可以加引号。通用字体族名由如下几种：

- serif：带衬线字体，笔画结尾有特殊的装饰线或衬线。
- sans-serif：无衬线字体，即笔画结尾是平滑的字体。
- monospace：等宽字体，即字体中每个字宽度相同。
- cursive：草书字体。这种字体有的有连笔，有的还有特殊的斜体效果。
- fantasy：主要是那些具有特殊艺术效果的字体。

#### 设置多个字体名或字体族名

为 font-family 属性设置多个值时，值之间需要使用逗号进行分隔。浏览器会选择列表中**第一个**该计算机上有安装的字体。

```html
p {
  font-family: "Gill Sans Extrabold", Helvetica, sans-serif;
}
```

### 字体大小

```html
p {
  font-size: 24px;
}
```

font-size 属性的值

| 名称       | 代码                                                         |
| ---------- | ------------------------------------------------------------ |
| 绝对大小值 | `xx-small`、`x-small`、`small`、`medium`、`large`、`x-large`、`x-large` |
| 相对大小值 | `larger`、`smaller`                                          |
| 长度值     | `px`、`em`、`rem`、`ex`                                      |
| 百分比值   | 该值是相对于父级元素的字体大小的百分比。                     |

关于标题元素在开发中的使用时需要注意如下要点：

- 建议最好使用用户默认字体大小的相对大小，避免使用除了 `em` 或 `ex` 的绝对大小单位。
- 如果一定要使用绝对大小的话，`px` 是众多单位中最好的选择。



### 粗细程度

```html
p {
  font-weight: bolder;
}
```

font-weight 属性的值

| 名称   | 代码                                                         |
| ------ | ------------------------------------------------------------ |
| 绝对值 | `normal` 和 `bold` 两个值。`normal` 与数字值 400 等价，`bold` 与数字值 700 等价。 |
| 相对值 | `lighter` 和 `bolder` 两个值。比从父元素继承来的值更细（lighter）/粗（bolder）（处在字体可行的粗细值范围内）。 |
| 数字值 | 介于 100 ~ 900 之间的值。                                    |

> **注意**：一些字体只提供 `normal` 和 `bold` 两种值。



### 设置斜体

```css
.italic {
  font-style: italic;
}

.oblique {
  font-style: oblique;
}
```

> 不是所有的字体都有确切的 `oblique` 和 `italic` 字形。即便如此，浏览器也会通过使用现有的字形来模拟所缺少的字形。



### font属性

CSS font 属性是用来作为 font-family、font-size、font-weight、font-style、font-variant 和 line-height 属性的简写形式，或将 HTML 元素的字体设置为系统字体。

>  如果将 font 属性作为简写形式的话，该属性必须包含 font-family 和 font-size 属性的值。而且指定相关属性的值时，需要注意如下要点：
>
>  - font-style、font-variant 和 font-weight 属性必须定义在 font-size 属性之前。
>  - line-height 属性必须定义在 font-size 属性后面，由 `/` 分隔，例如 `16px/3`。
>  - font-family 属性必须最后定义。

#### 系统字体

如果将 font 属性的值指定为系统关键字的话，必须是如下系统关键字之一：

- caption：用于标题控件（如按钮，下拉列表等）的系统字体。
- icon：用于标签图标的系统字体。
- menu：菜单中（如下拉菜单和菜单列表）使用的系统字体。
- message-box：用于对话框的系统字体。
- small-caption：用于小标题控件的系统字体。
- status-bar：用于窗口状态栏的系统字体。

```css
.p1 {
  font: 12px/14px sans-serif;
}

.p2 {
  font: 80% sans-serif;
}

.p3 {
  font: bold italic large serif;
}

.p4 {
  font: status-bar;
}
```



### 嵌入web字体

**@font-face** 是 CSS 的 @规则 中的一种，用来为 HTML 页面引入在线字体。通过 @font-face 我们可以自己来准备字体文件，从而可以消除对用户电脑字体的依赖。

```
@font-face {
  [ font-family: <family-name>; ] ||
  [ src: <src>; ] ||
  [ unicode-range: <unicode-range>; ] ||
  [ font-variant: <font-variant>; ] ||
  [ font-feature-settings: <font-feature-settings>; ] ||
  [ font-variation-settings: <font-variation-settings>; ] ||
  [ font-stretch: <font-stretch>; ] ||
  [ font-weight: <font-weight>; ] ||
  [ font-style: <font-style>; ]
}
```

- font-family：所指定的字体名字将会被用于 font 或 font-family 属性。
- src：通过 `url()` 函数指定远程字体文件的位置，或者通过 `local()` 函数指定用户的本地计算机上的字体。
- font-variant：同 font-variant 属性。
- font-stretch：同 font-stretch 属性。
- font-weight：同 font-weight 属性。
- font-style：同 font-style 属性。

```css
@font-face {
  font-family: "Alibaba PuHuiTi";
  src: url("/fonts/Alibaba-PuHuiTi-Regular.ttf");
}

body {
  font-family: "Alibaba PuHuiTi", serif
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galrf4o8okg210y0gfq3q.gif)

## 文本样式

| 名称          | 代码                            | 释义                                                         |
| ------------- | ------------------------------- | ------------------------------------------------------------ |
| 文本修饰      | `text-decoration`               | 设置 HTML 页面中文本排版                                     |
| 行间距        | `line-height`                   | 设置 HTML 页面中多行元素之间的空间量                         |
| 字母/单词间距 | `letter-spacing`/`word-spacing` | 字距是印刷行业用来描述字母之间空隙的一个术语                 |
| 水平对齐方式  | `text-align`                    | 设置 HTML 页面中文本内容相对于其所在元素在水平方式的对齐方式 |
| 垂直对齐方式  | `vertical-align`                | 设置 HTML 页面中内联元素或表格单元格元素在垂直方向上的对齐方式 |
| 文本缩进      | `text-indent`                   | 设置 HTML 页面中块级元素首行文本内容之前的缩进量             |
| 文本阴影      | `text-shadow`                   | 设置 HTML 页面中文本内容的阴影                               |
| 文本换行      | `word-wrap`和`word-break`       | 让文本和浏览器的右端自动实现换行                             |
| 处理空白      | `white-space`                   | 设置如何处理 HTML 元素中的空白                               |



### 文本修饰（text-decoration）

CSS text-decoration 属性用来设置 HTML 页面中文本排版（下划线、顶划线、删除线或者闪烁）。text-decoration 属性是一个简写属性，并且可以使用普通属性三个值中的任何一个。普通属性如下所示：

- text-decoration-line属性：用于设置元素中的文本的修饰类型。
- text-decoration-color属性：用于设置文本修饰线的颜色。
- text-decoration-style属性：用于设置由 text-decoration-line 设定的线的样式。

如下示例代码展示了 text-decoration 的 3 个普通属性的用法：

```css
p {
  text-decoration-color: lightcoral; /* 下划线颜色 */
  text-decoration-line: underline; /* 下划线 */
  text-decoration-style: solid; /* 实线 */
}
```

text-decoration 属性会延伸到后代元素。这意味着如果祖先元素指定了文本修饰属性，后代元素则不能将其删除

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galrw7skv7j20o80ec3z0.jpg)

### 行间距（line-height）

使用印刷机印刷出来的每一个字都位于一个块中，而控制两行文字之间的垂直距离就是**行间距**

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakpmjbdmfj216t0tetcj.jpg)

如上图所示，红色线是顶线、紫色线是中线、绿色线是基线、黄色线是底线，在本小节后面中会讲解到的 vertical-align 属性的 top、middle、baseline、bottom 指的就是这 4 条线。

- **行间距**也称为**行高**指的是两行文本内容中基线的距离，即两条绿色线之间的距离。也就是上图中的 1、2、3 和 4 的区域。
- **行距**指的是上一行的底线到下一行的顶线之间的距离，即上一行的黄色线到下一行的红色线之间的距离。
- **字体大小**值的是顶线到底线的距离，即红色线到黄色线之间的距离。也是就是上图中的 1、2 和 4 的区域。
- 

line-height属性值

| 属性          | 含义                                                         |
| ------------- | ------------------------------------------------------------ |
| normal 关键字 | 该值取决于用户电脑。一般情况下，浏览器使用的默认值为 1.2。   |
| 数字值        | 最终的效果值是该数字值乘以该元素的字体大小（font-size 属性值） |
| 长度值        | 如果使用 `em` 单位的可能会产生不确定的效果。                 |
| 百分比值      | 最终的效果值是该百分比值乘以该元素的字体大小（font-size 属性值）。 |

```css
.div1 {
      line-height: 1.2;
      font-size: 10pt;
    }

    /* 无单位数值 number/unitless */
    .div2 {
      line-height: 1.2em;
      font-size: 10pt;
    }

    /* 长度 length */
    .div3 {
      line-height: 120%;
      font-size: 10pt;
    }

    /* 百分比 percentage */
    .div4 {
      font: 10pt/1.2 Georgia, "Bitstream Charter", serif;
    }
 
```



### 字母间距（letter-spacing）与单词间距（word-spacing）

#### 字母间距

CSS letter-spacing 属性原意是用来设置文本字符之间的间距。在英文中是可以分为单词和字符的，但在中文中只有文字，中文中的文字就相当于英文的字符，所以 letter-spacing 属性可以适用于中文环境。

letter-spacing 属性的值

| 属性   | 含义                                         |
| ------ | -------------------------------------------- |
| normal | 该值是按照当前字体的正常间距确定的。         |
| 长度值 | 指定文字间的间距以替代默认间距，可以是负值。 |

```css
.first-example {
  letter-spacing: 0.4em;
}

.second-example {
  letter-spacing: 1em;
}

.third-example {
  letter-spacing: -0.05em;
}

.fourth-example {
  letter-spacing: 6px;
}
```

#### 单词间距

CSS word-spacing 属性用来设置 HTML 页面中标签之间或单词之间的距离，**该属性对英文是有效的，但对中文是无效的。**

| 属性     | 含义                                       |
| -------- | ------------------------------------------ |
| normal   | 该值是按照当前字体的正常间距确定的。       |
| 长度值   | 指定单词间的间距以替代默认间距。           |
| 百分比值 | 指定单词之间的间距以替代默认间距的百分比。 |

```css
.first-example {
  word-spacing: 15px;
}

.second-example {
  word-spacing: 5em;
}
```



### 水平对齐方式（text-align）

CSS text-align 属性用来设置 HTML 页面中文本内容相对于其所在元素在水平方式的对齐方式。值得注意的是，text-align 属性并不能设置 HTML 元素本身在水平方向的对齐，而是设置 HTML 元素内部的文本内容在其元素内水平方向的对齐。

```css
.example {
  text-align: right;
}
```

text-align 属性的值具有 7 种类型

| 属性          | 含义                                                  |
| ------------- | ----------------------------------------------------- |
| `start`       | 如果内容方向是左至右的话则等于 left，反之则为 right。 |
| `end`         | 如果内容方向是左至右的话则等于 right，反之则为 left。 |
| `left`        | 行内内容向左侧边对齐。                                |
| `right`       | 行内内容向右侧边对齐。                                |
| `center`      | 行内内容居中。                                        |
| `justify`     | 文字向两侧对齐，对最后一行无效。                      |
| `justify-all` | 和 `justify` 一致，但是强制使最后一行两端对齐。       |

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gakpycxtnzj21320tsajs.jpg)

### 垂直对齐方式（vertical-align）

CSS vertical-align 属性用来设置 HTML 页面中内联元素或表格单元格元素在垂直方向上的对齐方式。vertical-align 属性可以被应用于 2 种环境：

- 设置某个内联元素的盒子模型与该内联元素的父级容器元素的垂直对齐方式。
- 设置表格中某个单元格中内容的垂直对齐方式。

> vertical-align 属性只针对内联元素和表单单元格有效，对块级元素是无效的。

vertical-align 属性的值根据 2 种应用环境会有所不同：

- 应用于内联元素的值
  - 相对于父级元素的值
    - baseline：使元素的基线与父元素的基线对齐。
    - sub：使元素的基线与父元素的下标基线对齐。
    - super：使元素的基线与父元素的上标基线对齐。
    - text-top：使元素的顶部与父元素的字体顶部对齐。
    - text-bottom：使元素的底部与父元素的字体底部对齐。
    - middle：使元素的中部与父元素的基线加上父元素 x-height 的一半对齐。
  - 相对于行的值
    - top：使元素及其后代元素的顶部与整行的顶部对齐。
    - bottom：使元素及其后代元素的底部与整行的底部对齐。
- 应用于表单单元格的值
  - baseline：使单元格的基线，与该行中所有以基线对齐的其它单元格的基线对齐。
  - top：使单元格内边距的上边缘与该行顶部对齐。
  - middle：使单元格内边距盒模型在该行内居中对齐。
  - bottom：使单元格内边距的下边缘与该行底部对齐。

```css
img.top {
  vertical-align: text-top;
}

img.bottom {
  vertical-align: text-bottom;
}

img.middle {
  vertical-align: middle;
}
```



### 文本缩进（text-indent）

CSS text-indent 属性用来设置 HTML 页面中块级元素首行文本内容之前的缩进量.

```css
.example {
  text-indent: 5em;
}
```

| 属性      | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| 长度值    | 允许使用负值                                                 |
| 百分比值  | 使用所在块级元素的宽度的百分比作为缩进                       |
| each-line | 文本缩进会影响第一行，以及使用 `<br>` 元素强制断行后的第一行。 |
| hanging   | 该值会对所有的行进行反转缩进：除了第一行之外的所有的行都会被缩进，看起来就像第一行设置了一个负的缩进值。 |



### 文本阴影（text-shadow）

CSS text-shadow 属性用来设置 HTML 页面中文本内容的阴影。

```css
selector {
	text-shadow: color offset-x offet-y blur-raduis;
}
```

- color：可选项，设置文本内容的阴影颜色。

- offset-x：必选项，设置文本内容的阴影在水平方向的偏移量。

  - 如果值小于 0 的话，则表示向左偏移。
  - 如果值等于 0 的话，则表示水平方向不发生任何偏移。
  - 如果值大于 0 的话，则表示向右偏移。

- offset-y：必选项，设置文本内容的阴影在垂直方向的偏移量。

  - 如果值小于 0 的话，则表示向上偏移。
  - 如果值等于 0 的话，则表示垂直方向不发生任何偏移。
  - 如果值大于 0 的话，则表示向下偏移。

- blur-raduis：可选项，设置文本内容的阴影模糊半径。

  如果没有指定，则默认为 0。值越大，模糊半径越大，阴影也就越大越淡。

#### 设置单一阴影

```css
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			.example {
			  text-shadow: red 0 -2px;
			}
		</style>
	</head>
	<body>
		<p class="example">这是文本阴影测试</p>
	</body>
</html>

```



#### 设置多重阴影

```css
.example {
  text-shadow: 1px 1px 2px black, 0 0 1em blue, 0 0 0.2em blue;
}
```

当通过 text-shadow 属性为文本内容设置多重阴影时，阴影的应用顺序是从左到右的，第一个指定的阴影在顶部。

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1gals0huobjj20r10cgwfh.jpg)

### 文本换行（word-wrap 和 word-break）

- 对于西方文本，浏览器会在半角空格或连字符的地方自动换行。
- 对于中文文本，可以在任何文字后面换行。通常标点符号以及前面的文字作为整体统一换行。

#### word-wrap属性

word-wrap 属性属于微软的一个私有属性，在 CSS3 的文本规范中被重命名为 overflow-wrap。word-wrap 作为 overflow-wrap 的*别名*。

overflow-wrap 属性的值具有如下 2 种：

- normal：表示在正常的单词结束处换行。
- break-word：表示如果行内没有多余的地方容纳该单词到结尾，则那些正常的不能被分割的单词会被强制分割换行。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>overflow-wrap属性</title>
  <style>
    .example {
      width: 13em;
      background: gold;
    }

    .break {
      overflow-wrap: break-word;
    }
  </style>
</head>

<body>
  <p class="example">
    FStrPrivFinÄndG (Gesetz zur Änderung des Fernstraßenbauprivatfinanzierungsgesetzes und straßenverkehrsrechtlicher
    Vorschriften)
  </p>
  <p class="example break">
    FStrPrivFinÄndG (Gesetz zur Änderung des Fernstraßenbauprivatfinanzierungsgesetzes und straßenverkehrsrechtlicher
    Vorschriften)
  </p>
</body>

</html>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galsf3702wj20u50knabm.jpg)

#### word-break 属性

CSS word-break 属性用来设置 HTML 页面中文本内容自动换行的处理方式。通过具体的属性值设置，可以告知浏览器实现任意位置的换行。

word-break 属性的值具有如下 3 种：

- normal：使用默认的断行规则。
- break-all：对于除中文、日文和韩文外的文本内容，设置可以在任意字符间断行。
- keep-all：中文、日文和韩文的文本内容不断行，其他语言的文本内容等同于 normal。

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>word-break属性</title>
  <style>
    .narrow {
      width: 60%;
      padding: 20px;
      text-align: start;
      border: solid 1px #a9a9a9;
    }

    .normal {
      word-break: normal;
    }

    .breakAll {
      word-break: break-all;
    }

    .keepAll {
      word-break: keep-all;
    }
  </style>
</head>

<body>
  <p>1. <code>word-break: normal</code></p>
  <p class="normal narrow">This is a long and
    Honorificabilitudinitatibus califragilisticexpialidocious
    Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
    グレートブリテンおよび北アイルランド連合王国という言葉は本当に長い言葉</p>

  <p>2. <code>word-break: break-all</code></p>
  <p class="breakAll narrow">This is a long and
    Honorificabilitudinitatibus califragilisticexpialidocious
    Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
    グレートブリテンおよび北アイルランド連合王国という言葉は本当に長い言葉</p>

  <p>3. <code>word-break: keep-all</code></p>
  <p class="keepAll narrow">This is a long and
    Honorificabilitudinitatibus califragilisticexpialidocious
    Taumatawhakatangihangakoauauotamateaturipukakapikimaungahoronukupokaiwhenuakitanatahu
    グレートブリテンおよび北アイルランド連合王国という言葉は本当に長い言葉</p>
</body>

</html>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galsf3fikcj216i0nptbh.jpg)

### 处理空白（white-space）

CSS white-space 属性用来设置如何处理 HTML 元素中的空白

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>white-space属性</title>
  <style>
    .example-element {
      width: 16rem;
    }

    .example-element p {
      background-color: #eee;
      padding: .75rem;
      text-align: left;
    }

    #example1 {
	  white-space: normal;
    }
	#example2{
		white-space: nowrap;
	}
	#example3{
		white-space: pre;
	}
	#example4{
		white-space: pre-wrap;
	}
	#example5{
		white-space: pre-line;
	}
	#example6{
		white-space: normal;
	}
  </style>
</head>

<body>
  <div class="example-element" id="example1">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'

      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
  <div class="example-element" id="example2">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'
  
      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
  <div class="example-element" id="example3">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'
  
      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
  <div class="example-element" id="example4">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'
  
      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
  <div class="example-element" id="example5">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'
  
      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
  <div class="example-element" id="example6">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'
  
      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
</body>

</html><!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>white-space属性</title>
  <style>
    #example-element {
      width: 16rem;
    }

    #example-element p {
      background-color: #eee;
      padding: .75rem;
      text-align: left;
    }

    .example {
      /* 设置 white-space 属性不同的值用于测试效果 */
    }
  </style>
</head>

<body>
  <div id="example-element" class="example">
    <p>But ere she from the church-door stepped
      She smiled and told us why:
      'It was a wicked woman's curse,'
      Quoth she, 'and what care I?'

      She smiled, and smiled, and passed it off
      Ere from the door she stept—</p>
  </div>
</body>

</html>
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img832afe33ly1galsp7owcbg213u0nx15k.gif)

如下列表总结了各种 white-space 属性值的行为：

|                | 换行符 | 空格和制表符 | 文字转行 |
| :------------- | :----- | :----------- | :------- |
| `normal`       | 合并   | 合并         | 转行     |
| `nowrap`       | 合并   | 合并         | 不转行   |
| `pre`          | 保留   | 保留         | 不转行   |
| `pre-wrap`     | 保留   | 保留         | 转行     |
| `pre-line`     | 保留   | 合并         | 转行     |
| `break-spaces` | 保留   | 保留         | 转行     |