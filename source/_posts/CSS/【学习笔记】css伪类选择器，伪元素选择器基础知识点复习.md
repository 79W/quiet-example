---
title: 【学习笔记】css伪类选择器，伪元素选择器基础知识点复习
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - css
  - 伪元素选择器
  - 伪类选择器
excerpt: 伪类选择器 伪类选择器是一种允许通过未包含在 HTML 元素的状态信息来定位
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgDf3HGASjCgaPtBy.png'
articleLink: 40f762be87e52402
date: 2020-01-02 20:22:24
---

## 伪类选择器

**伪类选择器**是一种允许通过未包含在 HTML 元素的状态信息来定位 HTML 元素的用法。**伪类选择器**的具体用法就是向已有的选择器增加关键字，表示定位的 HTML 元素的状态信息。

**伪类选择器**的语法结构如下所示：

```
选择器:伪类 {
   属性 : 属性值;
}
```

**伪类选择器**的具体语法格式为 `:伪类`，这里一定不要忘记 `:`。

**说明**：CSS 允许在同一个选择器上同时编写多个伪类选择器。

### 伪类选择器种类

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgDf3HGASjCgaPtBy.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

**提示**：上图由 [MDN 网站提供](https://www.79bk.cn/go/?url=aHR0cHM6Ly9kZXZlbG9wZXIubW96aWxsYS5vcmcvemgtQ04vZG9jcy9XZWIvQ1NTL1BzZXVkby1jbGFzc2Vz)。

**伪类选择器**的数量这么多，为了更好地梳理**伪类选择器**，我们可以按照用途的不同分为如下 5 种类型：

1.  动态伪类选择器：常与 “ 元素配合使用，用来表示用户的某种行为状态。
2.  目标伪类选择器：常与 “ 元素配合使用，用来定位当前 HTML 页面中唯一一个目标元素。
3.  元素状态伪类选择器：常与表单组件元素配合使用，用来表示表单组件的某种状态。
4.  结构伪类选择器：常与 “ 元素配合使用，利用 HTML 元素之间的关系定位目标元素。
5.  否定伪类选择器：用来定位与指定选择器不匹配的 HTML 元素。

### 否定伪类选择器

```
:not(selector) {
    属性 : 属性值;
}
```

**否定伪类选择器**的作用是用来定位不匹配 `selector` 选择器定位的 HTML 元素的元素。可能这句话看起来比较绕，不太好懂。

**注意**：

*   可以利用这个伪类提高规则的优先级。例如， `#foo:not(#bar)` 和 `#foo` 会匹配相同的元素。 但是前者的优先级更高。
*   `:not(foo)` 将匹配任何非 foo 元素，包括 html 和 body 元素。
*   这个选择器只会应用在一个元素上，你无法用它排除所有父元素。

此内容摘自 [MDN 网站的否定伪类选择器文章](https://www.79bk.cn/go/?url=aHR0cHM6Ly9kZXZlbG9wZXIubW96aWxsYS5vcmcvemgtQ04vZG9jcy9XZWIvQ1NTLzpub3Q=)

## 伪元素选择器

CSS 中**伪元素选择器**的用法与伪类选择器的用法类似，都是在指定 CSS 选择器增加关键字。但这两者的作用并不相同，伪类选择器是用来描述某个指定元素的状态信息，而伪元素选择器是用来描述某个指定元素的特定部分设定样式。

**伪元素选择器**的语法结构如下所示：

```css
/* CSS3 语法 */
选择器::伪元素 {
  属性 : 属性值;
}
/* CSS2 过时语法 (仅用来支持 IE8) */
选择器:伪元素 {
  属性 : 属性值;
}
```

**伪元素选择器**的语法格式为 `::伪元素`，一定不要忘记 `::`。

目前 CSS 提供的伪元素选择器的数量如下所示：

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imga8DT5sGHiz4mCfF.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

### ::before 和 ::after 伪元素

**::before 伪元素**的作用是作为定位的 HTML 元素的第一个子级元素

```
a::before {
  content: "♥";
}
```

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgtqRxvBwGC4EP5l9.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

**::after 伪元素**的作用是作为定位的 HTML 元素的最后一个子级元素。

```
a::after {
  content: "→";
}
```

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgIApFzqP9kjXnl68.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

### 内容生成

`content` 属性用于在元素的 `::before` 伪元素和 `::after` 伪元素中插入内容。该属性具有的值如下所示：

*   none：不会产生伪类元素。
*   normal：`::before` 伪元素和 `::after` 伪类元素中会被视为 none。
*   text：文本内容。
*   url：格式为 `url()`，指定一个外部资源（比如图片）。如果该资源或图片不能显示，它就会被忽略或显示一些占位。

由 `content` 属性增加的内容默认情况下为内联元素（有关内联元素的内容会在后续章节中进行详细讲解）。

### ::first-letter 伪元素

**::first-letter 伪元素**的作用是为匹配元素的文本内容的第一个字母设置样式内容

```
p::first-letter {
  font-size: 130%;
}
```

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgTVQr85sqhYvN2P4.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

参看 [MDN 网站的 ::first-letter 伪元素文章](https://www.79bk.cn/go/?url=aHR0cHM6Ly9kZXZlbG9wZXIubW96aWxsYS5vcmcvemgtQ04vZG9jcy9XZWIvQ1NTLzo6Zmlyc3QtbGV0dGVyIyVFNSU4NSU4MSVFOCVBRSVCOCVFNyU5QSU4NCVFNSVCMSU5RSVFNiU4MCVBNyVFNSU4MCVCQw==)

### ::first-line 伪元素

**::first-line 伪元素**的作用是为匹配 HTML 元素的文本内容的第一行设置样式内容

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgqwGnfKXQ4Mt6eRx.png "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

参看 [MDN 网站的 ::first-line 伪元素文章](https://www.79bk.cn/go/?url=aHR0cHM6Ly9kZXZlbG9wZXIubW96aWxsYS5vcmcvemgtQ04vZG9jcy9XZWIvQ1NTLzo6Zmlyc3QtbGluZSMlRTUlODUlODElRTglQUUlQjglRTclOUElODQlRTUlQjElOUUlRTYlODAlQTclRTUlODAlQkM=)

### ::selection 伪元素

**::selection 伪元素**的作用是匹配用户在 HTML 页面选中的文本内容（比如使用鼠标或其他选择设备选中的部分）设置高亮效果

```
p::selection {
    color: gold;
    background-color: red;
}
```

![【学习笔记】css伪类选择器，伪元素选择器基础知识点复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgSlNRwY4QZg9bkMd.gif "【学习笔记】css伪类选择器，伪元素选择器基础知识点复习")

只有一小部分 CSS 属性可以用于`::selection` 伪元素：

*   color 属性
*   background-color 属性
*   cursor 属性
*   caret-color 属性
*   outline 属性
*   text-decoration 属性
*   text-emphasis-color 属性
*   text-shadow 属性