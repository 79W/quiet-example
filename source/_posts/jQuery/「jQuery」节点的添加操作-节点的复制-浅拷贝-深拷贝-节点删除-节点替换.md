---
title: 「jQuery」节点的添加操作&节点的复制(浅拷贝&深拷贝)&节点删除&节点替换
categories: 技术笔记
tags:
  - HTML
  - jquery
excerpt: 节点添加内部插入append(content&#124;fn)
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1-1-20200916134804238.gif'
articleLink: 25013e7fe9cd3718
date: 2020-06-18 11:05:37
---

## 节点添加

### 内部插入

#### append(content|fn)

向每个匹配的元素内部追加内容。

```js
$(元素).append("<b>Hello</b>");
```

#### appendTo(content)

把所有匹配的元素追加到另一个指定的元素元素集合中。

实际上，使用这个方法是颠倒了常规的$(A).append(B)的操作，即不是把B追加到A中，而是把A追加到B中。

```js
要被添加到元素.appendTo("添加到这个元素里面");
```

#### prepend(content)

向每个匹配的元素内部前置内容。

```js
$(元素).prepend("<b>Hello</b>");
```

#### prependTo(content)

把所有匹配的元素前置到另一个、指定的元素元素集合中。

实际上，使用这个方法是颠倒了常规的$(A).prepend(B)的操作，即不是把B前置到A中，而是把A前置到B中。

```js
要被添加到元素.prependTo("添加到这个元素里面");
```

### 外部插入

#### after(content|fn)

在每个匹配的元素之后插入内容。

这个插入 是插入到被插入元素的外部一层

```js
$(元素).after("<b>Hello</b>");
```

#### before(content|fn)

在每个匹配的元素之前插入内容。

```js
$("元素").before("<b>Hello</b>");
```

#### insertAfter(content)

把所有匹配的元素插入到另一个、指定的元素元素集合的后面。

实际上，使用这个方法是颠倒了常规的$(A).after(B)的操作，即不是把B插入到A后面，而是把A插入到B后面。

```js
被插入的元素.insertAfter("添加到这个元素里面");
```

#### insertBefore(content)

把所有匹配的元素插入到另一个、指定的元素元素集合的前面。

实际上，使用这个方法是颠倒了常规的$(A).before(B)的操作，即不是把B插入到A前面，而是把A插入到B前面。

```js
被插入的元素.insertBefore("添加到这个元素里面");
```

![「jQuery」节点的添加操作&节点的复制(浅拷贝&深拷贝)&节点删除&节点替换](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1-1-20200916134804238.gif "「jQuery」节点的添加操作&节点的复制(浅拷贝&深拷贝)&节点删除&节点替换")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>添加节点操作-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <script>
        $(function(){
            $("button").eq(0).click(function(){
                var $li = $("<li>新增的</li>");
                //内部添加 添加到最下面
                $("ul").append($li);
            })
            $("button").eq(1).click(function(){
                var $li = $("<li>新增的</li>");
                //内部添加 添加到最下面
                $li.appendTo("ul")
            })
            $("button").eq(2).click(function(){
                var $li = $("<li>新增的</li>");
                // 内部添加 添加到最上面
                $("ul").prepend($li);
            })
            $("button").eq(3).click(function(){
                var $li = $("<li>新增的</li>");
                // 内部添加 添加到最上面
                $li.prependTo("ul")
            })
            $("button").eq(4).click(function(){
                var $li = $("<li>新增的</li>");
                // 外部添加 添加到最下面
                $("ul").after($li);
            })
            $("button").eq(5).click(function(){
                var $li = $("<li>新增的</li>");
                // 外部添加 添加到最上面
                $("ul").before($li)
            })
            $("button").eq(6).click(function(){
                var $li = $("<li>新增的</li>");
                // 外部添加 添加到最下面
                $li.insertAfter("ul");
            })
            $("button").eq(7).click(function(){
                var $li = $("<li>新增的</li>");
                // 外部添加 添加到最上面
                $li.insertBefore("ul")
            })
        })
    </script>
</head>
<body>
    <button>内部append</button>
    <button>appendTo</button>
    <button>prepend</button>
    <button>prependTo</button>
    <button>外部after</button>
    <button>before</button>
    <button>insertAfter</button>
    <button>insertBefore</button>
    <ul>
        <li>第一个</li>
        <li>第二个</li>
        <li>第三个</li>
        <li>第四个</li>
    </ul>
</body>
</html>
```

## 节点删除

### remove(_\[expr\]_)

从DOM中删除所有匹配的元素。

```html
<!--html-->
<p>Hello</p> how are <p>you?</p>
```

```js
//jquery
$("p").remove();
//--------------
$("p").remove(".hello");
```

```
//结果1
how are
//结果2
how are <p>you?</p>
```

### empty()

清空匹配的元素集合中所有的子节点。

```html
<!--html-->
<p>Hello, <span>Person</span> <a href="#">and person</a></p>
```

```js
//jquery
$("p").empty();
```

```
//结果
<p></p>
```

## 节点替换

### replaceAll(selector)

用匹配的元素替换掉所有 selector匹配到的元素。

```html
<!--html-->
<p>Hello</p><p>cruel</p><p>World</p>
```

```js
//jquery
$("<b>Paragraph. </b>").replaceAll("p");
// 将新的（<b>Paragraph. </b>） 元素 替换原来元素P的
```

```
//结果
<b>Paragraph. </b><b>Paragraph. </b><b>Paragraph. </b>
```

#### replaceWith(content|fn)

将所有匹配的元素替换成指定的HTML或DOM元素。

```html
<!--html-->
<p>Hello</p><p>cruel</p><p>World</p>
```

```js
//jquery
$("p").replaceWith("<b>Paragraph. </b>");
//将<b>Paragraph. </b> 替换p中的
```

```
//结果
<b>Paragraph. </b><b>Paragraph. </b><b>Paragraph. </b>
```

## 节点复制

### clone(\[Even\[,deepEven\]\])

克隆匹配的DOM元素并且选中这些克隆的副本。

在想把DOM文档中元素的副本添加到其他位置时这个函数非常有用。

#### 参数：

一个布尔值（true 或者 false）指示事件处理函数是否会被复制。V1.5以上版本默认值是：false

```html
<!--html-->
<b>Hello</b><p>, how are you?</p>
```

```js
//jquery
$("b").clone(false).prependTo("p");
```

```html
<!--结果-->
<b>Hello</b><p><b>Hello</b>, how are you?</p>
```

如果 为 true的时候当被复制的这个 元素节点有方法函数的是会被同时复制过去。

