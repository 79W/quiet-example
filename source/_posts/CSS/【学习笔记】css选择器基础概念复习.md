---
title: 【学习笔记】css选择器基础概念复习
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - css
  - 基础知识
  - 选择器
excerpt: 选择器概念 选择器分类 基本选择器：共有 5 个基本选择器，是 CSS 选择器的最为基本的用法。 层级选择器：共有 4 个层级选择器，是根据 HTML
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgAUC6bvonepfaLmQ.png'
articleLink: da1665177ff42102
date: 2020-01-02 19:50:39
---

## 选择器概念

### 选择器分类

*   基本选择器：共有 5 个基本选择器，是 CSS 选择器的最为基本的用法。
*   层级选择器：共有 4 个层级选择器，是根据 HTML 元素之间的关系来定位 HTML 元素。
*   组合选择器：具有交集和并集两种用法，是将之前基本选择器和层级选择器进行组合。
*   伪类选择器：允许未包含在 HTML 页面中的状态信息选定位 HTML 元素。
*   伪元素选择器：定位所有未被包含 HTML 的实体。

## 基本选择器

1.  类型（Type）选择器（_有些中文资料中称为“元素选择器”_）
2.  类（Class）选择器
3.  ID 选择器
4.  通用选择器（_有些中文资料中称为“通配符选择器”_）
5.  属性选择器

### 类型选择器

**类型选择器**，又称为**元素选择器**

```
元素名{
    属性:属性值;
}
```

### 类选择器

```
.classname{
        属性:属性名;

}
```

![【学习笔记】css选择器基础概念复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgAUC6bvonepfaLmQ.png "【学习笔记】css选择器基础概念复习")

### ID选择器

```
#ID{
        属性:属性值;
}
```

![【学习笔记】css选择器基础概念复习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgKqaBQJlwoWH5VN8.png "【学习笔记】css选择器基础概念复习")

### 通用选择器

```
*{
    属性:属性值;
}
```

## 属性选择器

*   `[attr]` 属性选择器：通过 HTML 元素的 attr 属性名来定位具体 HTML 元素，而不关注 attr 属性的值是什么。
*   `[attr=value]` 属性选择器：通过 HTML 元素的 attr 属性名并且属性值为 value 来定位具体 HTML 元素。
*   `[attr~=value]` 属性选择器：通过 HTML 元素的 attr 属性名，属性值是一个以空格分隔的列表并且 value 值是该值列表中的之一，来定位具体 HTML 元素。
*   `[attr|=value]` 属性选择器：通过 HTML 元素的 attr 属性名并且属性值为 value 或者以 `value-` 为前缀来定位具体 HTML 元素。
*   `[attr^=value]` 属性选择器：通过 HTML 元素的 attr 属性名并且属性值是以 value 为开头来定位具体 HTML 元素。
*   `[attr$=value]` 属性选择器：通过 HTML 元素的 attr 属性名并且属性值是以 value 为结束来定位具体 HTML 元素。
*   `[attr*=value]` 属性选择器：通过 HTML 元素的 attr 属性名并且属性值是包含 value 来定位具体 HTML 元素。

## 选择器的优先级

### 优先级的计算方式如下：

1.  优先级就是分配给指定的 CSS 声明的一个权重，它由**匹配的选择器中的每一种选择器类型的数值**决定。
2.  当优先级与多个 CSS 声明中任意一个声明的优先级相等的时候，CSS 中最后的那个声明将会被应用到元素上。
3.  当同一个元素有多个声明的时候，优先级才会有意义。

### 选择器类型权重

1.  通配符选择器的优先级为 0
2.  类型（元素）选择器的优先级为 1
3.  类选择器和属性选择器的优先级为 10
4.  ID 选择器的优先级为 100
5.  内联样式的优先级为 1000

**内联样式总会覆盖内嵌样式表和外联样式表的任何样式**。

### !important 例外规则

如果使用 `!important` 规则的话，会覆盖掉所有其他选择器的声明样式。

**注意**：由于 `!important` 规则会破坏 CSS 选择器类型的权重规则，所以在开发中不建议使用。