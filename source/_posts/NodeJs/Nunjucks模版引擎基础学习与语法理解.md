---
title: Nunjucks模版引擎基础学习与语法理解
categories: 技术笔记
tags:
  - Nunjucks
  - 模版引擎
excerpt: Nunjucks模版引擎基础学习与语法理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgnunjucks.png'
articleLink: d09d3ec902072726
date: 2020-09-26 09:22:27
---

### 文件拓展名

虽然你可以用任意扩展名来命名你的Nunjucks模版或文件，但Nunjucks社区还是推荐使用`.njk`。

如果你在给Nunjucks开发工具或是编辑器上的语法插件时，请记得使用`.njk`扩展名。

### 变量

变量会从模板上下文获取，如果你想显示一个变量可以：

```
{{ username }}
{{ foo.bar }}
{{ foo["bar"] }}
```

会从上下文查找 `username` 然后显示，可以像 javascript 一样获取变量的属性

如果变量的值为 `undefined` 或 `null` 将不显示

### if

与 javascript 中的 `if` 类似。

```
{% if variable %}
  It is true
{% endif %}
```

```
{% if hungry %}
  I am hungry
{% elif tired %}
  I am tired
{% else %}
  I am good!
{% endif %}
```

### for

`for` 可以遍历数组 (arrays) 和对象 (dictionaries)。

```
var items = [{ title: "foo", id: 1 }, { title: "bar", id: 2}];
```

```
<h1>Posts</h1>
<ul>
{% for item in items %}
  <li>{{ item.title }}</li>
{% else %}
  <li>This would display if the 'item' collection were empty</li>
{% endfor %}
</ul>
```

### macro

宏 (`macro`) 可以定义可复用的内容，类似与编程语言中的函数

```html
{% macro field(name, value='', type='text') %}{% endmacro %}
  <div class="field">
    <input type="{{ type }}" name="{{ name }}" value="{{ value | escape }}" />
  </div>

```

现在 `field` 可以当作函数一样使用了：

```
{{ field('user') }}
{{ field('pass', type='password') }}
```

### set

可以设置和修改变量。

```
{{ username }}
{% set username = "joe" %}
{{ username }}
```

如果 `username` 初始化的时候为 "james', 最终将显示 "james joe"。

可以设置新的变量，并一起赋值。

```
{% set x, y, z = 5 %}
```

### 正则表达式

你可以像在JavaScript中一样创建一个正则表达式:

```
{{ /^foo.*/ }}
{{ /bar$/g }}
```

正则表达式所支持的标志如下

- `g`: 应用到全局
- `i`: 不区分大小写
- `m`: 多行模式
- `y`: 粘性支持（sticky）

### 过滤器

过滤器是一些可以执行变量的函数，通过管道操作符 (`|`) 调用，并可接受参数。

```
{{ foo | title }}
{{ foo | join(",") }}
{{ foo | replace("foo", "bar") | capitalize }}
```

第三个链式过滤器，最终会显示 "Bar"，第一个过滤器将 "foo" 替换成 "bar"，第二个过滤器将首字母大写。

#### replace

返回值的副本，其中所有出现的子字符串均替换为新的子字符串

第一个参数是应该替换的子字符串

第二个参数是替换字符串。

如果给出了可选的第三个参数`count`，则仅`count`替换第一个 出现的参数

```
{{ "Hello World"|replace("Hello", "Goodbye") }}
//-> Goodbye World
```

```
{{ "aaaaargh"|replace("a", "d'oh, ", 2) }}
//-> d'oh, d'oh, aaargh
```

#### round

将数字四舍五入到给定的精度。

第一个参数指定精度（默认值为`0`）

第二个参数舍入方法

- `'common'` 向上或向下取整
- `'ceil'` 总是四舍五入
- `'floor'` 总是四舍五入

```
{{ 42.55|round }}
//-> 43.0
{{ 42.55|round(1, 'floor') }}
//-> 42.5
```

#### max

返回序列中最大的项目

```
{{ [1, 2, 3]|max }}
//-> 3
```

#### min

返回序列中的最小项

```
{{ [1, 2, 3]|min }}
//-> 1
```

#### join

返回一个字符串，该字符串是序列中字符串的串联。

```
{{ [1, 2, 3]|join('|') }}
//-> 1|2|3

{{ [1, 2, 3]|join }}
//-> 123
```

还有很多过滤器就不一一演示了

### 全局函数

#### range

如果你需要遍历固定范围的数字可以使用 `range`，`start` (默认为 0) 为起始数字，`stop` 为结束数字，`step` 为间隔 (默认为 1)。

```
% for i in range(0, 5) -%}
  {{ i }},
{%- endfor %}
```

### 模板继承

模板继承可以达到模板复用的效果，当写一个模板的时候可以定义 "blocks"，子模板可以覆盖他，同时支持多层继承。

`parent.html`

```
{% block header %}
This is the default content
{% endblock %}

<section class="left">
  {% block left %}{% endblock %}
</section>

<\section class="right">
  {% block right %}
  This is more content
  {% endblock %}
</section>
```

然后再写一个模板继承他

```
{% extends "parent.html" %}

{% block left %}
This is the left side!
{% endblock %}

{% block right %}
This is the right side!
{% endblock %}
```

以下为渲染结果

```
This is the default content

<section class="left">
  This is the left side!
</section>

<section class="right">
  This is the right side!
</section>
```

#### extends

用来指定模板继承，被指定的模板为父级模板

```
{% extends "base.html" %}
```

`extends`也可以接受任意表达式，只要它最终返回一个字符串或是模板所编译成的对象：

```
{% extends name + ".html" %}
```

#### block

区块(`block`) 定义了模板片段并标识一个名字，在模板继承中使用

父级模板可指定一个区块，子模板覆盖这个区块

```
{% block name %}
	这里是块内容
{% endblock %}
```

覆盖

```
{% block name %}
	这里就可以写新的内容了
{% endblock %}
```

如果你想调用 父级模板 内的内容怎么办呐 ？

你可以调用特殊的`super`函数

它会渲染父级区块中的内容

#### super

你可以通过调用`super`从而将父级区块中的内容渲染到子区块中

父级：

```
{% block name %}
我是父级内的东西
{% endblock %}
```

子级内调用

```
{% block name %}
{{ super() }}
我在写点自己子级的内容
{% endblock %}
```

结果

```
我是父级内的东西
我在写点自己子级的内容
```

### 注释

Nunjucks注释

```
{# 我是Nunjucks注释我不会显示 #}
```

