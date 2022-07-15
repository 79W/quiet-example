---
title: Pug模版引擎学习与语法理解
categories: 技术笔记
tags:
  - Pug
  - 模版引擎
excerpt: Pug模版引擎学习与语法理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200925191049.png'
articleLink: 4b07a99ab7225725
date: 2020-09-25 15:09:57
---

模板引擎是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的文档

**就是将模板文件和数据通过模板引擎生成一个HTML代码**

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200925174003.png)

## Pug

**安装**

```
$ npm install pug
```

### 编写标签

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="X-UA-Compatible" content="ie=edge" />
  <title>Pug模版引擎</title>
</head>
<body></body>
</html>
```

将上面这段 html 基本的框架代码用我们的 pug 进行编写就变成了下面的这样的代码

```
<!DOCTYPE html>
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        meta(http-equiv="X-UA-Compatible", content="ie=edge")
        title Pug模版引擎
    body
```

可以看出Pug的标签是单标签并且是以缩进来表示层级关系的！

我们需要在 `body` 内写个 `h1` 标签

```
<!DOCTYPE html>
html(lang="en")
    head
        meta(charset="UTF-8")
        meta(name="viewport", content="width=device-width, initial-scale=1.0")
        meta(http-equiv="X-UA-Compatible", content="ie=edge")
        title Pug模版引擎
    body
    		h1 我是H1标签
```

那么我们的做法就是将`h1`标签进行锁进 就成为 `body`内的`h1`标签了

```
<body>
   <h1>我是H1标签</h1>
</body>
```

如果你讲 `h1` 标签没有缩进 并且和`body` 同级别

```
body
h1 我是H1标签
```

那么转换成`html的格式就是

```
<body></body>
<h1>我是H1标签</h1>
```

其他的标签也是同样的原理

#### 标签中的纯文本块

一个典型的例子就是用 `script` 和 `style` 内嵌 JavaScript 和 CSS 代码

只需要在标签后面紧接一个点 `.`

```
script.
  if (usingPug)
    console.log('这是明智的选择。')
  else
    console.log('请用 Pug。')
```

```
<script>
  if (usingPug)
    console.log('这是明智的选择。')
  else
    console.log('请用 Pug。')
</script>
```

### 标签属性

标签属性和 HTML 语法非常相似，但它们的值就是普通的 JavaScript 表达式。您可以用逗号作为属性分隔符，不过不加逗号也是允许的。

使用 `pug` 创建个`a` 标签并加上跳转地址`href`

```
a(href='79bk.cn') 乔博客
```

转换后的效果就是

```
<a href="79bk.cn">乔博客</a>
```

#### 多行属性

如果使用`pug`的时候编写一个标签有多个属性

```
input(
  type='checkbox'
  name='agreement'
  checked
)
```

转换后的效果

```
<input type="checkbox" name="agreement" checked="checked" />
```

#### 用引号括起来的属性

如果您的属性名称中含有某些奇怪的字符，并且可能会与 JavaScript 语法产生冲突的话，请您将它们使用 `""` 或者 `''` 括起来。您还可以使用逗号来分割不同的属性。

```
//- 在这种情况下，`(click)` 会被当作函数调用而不是
//- 属性名称，这会导致一些稀奇古怪的错误。
div(class='div-class' (click)='play()')
```

上面的代码会出现以下的错误

```
index.pug:3:11
    1| //- 在这种情况下，`(click)` 会被当作函数调用而不是
    2| //- 属性名称，这会导致一些稀奇古怪的错误。
  > 3| div(class='div-class' (click)='play()')
-----------------^

Syntax Error: Assigning to rvalue
```

正确的写法

```
div(class='div-class', (click)='play()')
div(class='div-class' '(click)'='play()')
```

转换的结果

```
<div class="div-class" (click)="play()"></div>
<div class="div-class" (click)="play()"></div>
```

#### 变量属性

那么有两个问题

1. 如何在pug文件声明变量
2. 如何调用变量

**声明变量**

在前面加上一个 `-` 然后写上一个空格 就可以按照js的方式声明变量了

```
- let url = 'pug-test.html';
```

**调用**

直接在变量的下面的内容里面写上 变量名称即可

```
- var url = 'pug-test.html';
a(href='/' + url) 链接
```

转换的结果

```
<a href="/pug-test.html">链接</a>
```

#### 布尔值属性 

在 Pug 中，布尔值属性是经过映射的，这样布尔值（`true` 和 `false`）就能接受了。当没有指定值的时候，默认是 `true`。

```
input(type='checkbox' checked)
input(type='checkbox' checked=true)
input(type='checkbox' checked=false)
input(type='checkbox' checked=true.toString())
```

```
<input type="checkbox" checked="checked" />
<input type="checkbox" checked="checked" />
<input type="checkbox" />
<input type="checkbox" checked="true" />
```

#### 样式属性

`style`（样式）属性可以是一个字符串（就像其他普通的属性一样）还可以是一个对象，这在部分样式是由 JavaScript 生成的情况下非常方便。

```
a(style={color: 'red', background: 'green'})
```

```
<a style="color:red;background:green;"></a>
```

#### 类属性

```
div(class="mydiv") 我是类名为mydiv的div
```

转换的结果

```
<div class="mydiv">我是类名为mydiv的div</div>
```

#### 类或ID的字面值

```
a.button
```

```
<a class="button"></a>
```

下面是不写前面的标签显示的是div标签

```
.mydiv2(style={width:"100px",height:"100px",background:"blue"}) 我是div2
#myid 我是id为myid的div
```

转换的结果

```
<div class="mydiv2" style="width:100px;height:100px;background:blue;">我是div2</div>
<div id="myid">我是id为myid的div</div>
```

### 分支条件

#### Case

这个跟js当中的 switch语句 一样的

```
- var friends = 10
case friends
  when 0
    p 您没有朋友
  when 1
    p 您有一个朋友
  default
    p 您有 #{friends} 个朋友
```

```
<p>您有 10 个朋友</p>
```

Case传递会在遇到非空的语法块前一直进行下去。

在某些情况下，如果您不想输出任何东西的话，您可以明确地加上一个原生的 `break` 语句：

```
- var friends = 0
case friends
  when 0
    - break
  when 1
    p 您的朋友很少
  default
    p 您有 #{friends} 个朋友
```

这样就什么也不会打印出来了

如果这种情况下你没有进行添加 `- break` 语句那么就会打印出 ：您的朋友很少 

#### 条件if

可以在pug 当中使用`if else if else `语句

```
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green 描述
    p.description= user.description
  else if authorised
    h2.blue 描述
    p.description.
      用户没有添加描述。
      不写点什么吗……
  else
    h2.red 描述
    p.description 用户没有描述
```

```
<div id="user">
  <h2 class="green">描述</h2>
  <p class="description">foo bar baz</p>
</div>
```

Pug 同样也提供了它的反义版本 `unless`

```
unless user.isAnonymous
  p 您已经以 #{user.name} 的身份登录。
```

等同于

```
if !user.isAnonymous
  p 您已经以 #{user.name} 的身份登录。
```

### 循环迭代

Pug 目前支持两种主要的迭代方式： `each` 和 `while`。 但是for也可以写

#### each

```
ul
  each val, index in ['〇', '一', '二']
    li= index + ': ' + val
```

```
<ul>
  <li>0: 〇</li>
  <li>1: 一</li>
  <li>2: 二</li>
</ul>
```

您还能添加一个 `else` 块，这个语句块将会在数组与对象没有可供迭代的值时被执行

```
- var values = [];
ul
  each val in values
    li= val
  else
    li 没有内容
```

```
<ul>
  <li>没有内容</li>
</ul>
```

#### for

可以直接替换掉 `each`

```
- var values = [];
ul
  for val in values
  	li= val
  else
  	li 没有内容
```

和`each` 是同等的效果

#### while

```
- var n = 0;
ul
  while n < 4
    li= n++
```

```
<ul>
  <li>0</li>
  <li>1</li>
  <li>2</li>
  <li>3</li>
</ul>
```

### 混入 Mixin

混入是一种允许您在 Pug 中重复使用一整个代码块的方法。

```
//- 定义
mixin list
  ul
    li foo
    li bar
    li baz
//- 使用
+list
+list
```

```
<ul>
  <li>foo</li>
  <li>bar</li>
  <li>baz</li>
</ul>
<ul>
  <li>foo</li>
  <li>bar</li>
  <li>baz</li>
</ul>
```

也可以携带一些参数

```
mixin pet(name)
  li.pet= name
ul
  +pet('猫')
  +pet('狗')
  +pet('猪')
```

```
<ul>
  <li class="pet">猫</li>
  <li class="pet">狗</li>
  <li class="pet">猪</li>
</ul>
```

#### 混入的属性

混入也可以隐式地，从“标签属性”得到一个参数 `attributes`：

```
mixin link(href, name)
  //- attributes == {class: "btn"}
  a(class!=attributes.class href=href)= name

+link('/foo', 'foo')(class="btn")
```

```
<a class="btn" href="/foo">foo</a>
```

### 包含Include

在编写网站的时候 有时候我们的头部和底部是一样的传统写法我们会写出很多重复的代码后期会很难进行维护

那么我们就可以进行一次编写多次引入

编写头部

```
//- includes/head.pug
head
  title 我的网站
  script(src='/javascripts/jquery.js')
  script(src='/javascripts/app.js')
```

编写底部

```
//- includes/foot.pug
footer#footer
  p Copyright (c) foobar
```

然后我们只需要在需要的页面进行引入就可以了

那么我们在首页进行引入

```
//- index.pug
doctype html
html
  include includes/head.pug
  body
    h1 我的网站
    p 欢迎来到我这简陋得不能再简陋的网站。
    include includes/foot.pug
```

我们打开首页的到的结果就是

```
<!DOCTYPE html>
<html>

<head>
  <title>我的网站</title>
  <script src="/javascripts/jquery.js"></script>
  <script src="/javascripts/app.js"></script>
</head>

<body>
  <h1>我的网站</h1>
  <p>欢迎来到我这简陋得不能再简陋的网站。</p>
  <footer id="footer">
    <p>Copyright (c) foobar</p>
  </footer>
</body>

</html>
```

### 注释

带输出的注释和 JavaScript 的单行注释类似

```
// 一些内容
p foo
p bar
```

```
<!-- 一些内容-->
<p>foo</p>
<p>bar</p>
```

不输出的注释，只需要加上一个横杠

这些内容仅仅会出现在 Pug 模板之中，*不会*出现在渲染后的 HTML 中

```
//- 这行不会出现在结果里
p foo
p bar
```

```
<p>foo</p>
<p>bar</p>
```

#### 块注释

```
body
  //-
    给模板写的注释
    随便写多少字
    都没关系。
  //
    给生成的 HTML 写的注释
    随便写多少字
    都没关系。
```

```
<body>
  <!--给生成的 HTML 写的注释
随便写多少字
都没关系。-->
</body>
```

