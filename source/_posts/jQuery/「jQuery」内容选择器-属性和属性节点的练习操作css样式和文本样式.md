---
title: '「jQuery」内容选择器,属性和属性节点的练习操作css样式和文本样式'
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - css
  - jquery
  - 内容选择器
  - 属性
  - 属性节点
excerpt: 内容选择器
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916132100.png'
articleLink: d5eff083cf513316
date: 2020-06-16 11:47:33
---

## 内容选择器

分别介绍 :empty :parent :contains() :has()

```js
 // :empty 作用找到既没有文本内容也没有子元素的指定元素
 var $div = $("div:empty");
 console.log($div);
 //:parent 作用找到有文本或有子元素的指定元素
 var $div2 = $("div:parent");
 console.log($div2);
 //:contains() 作用找到包含指定文本的指定元素
 var $div3 = $("div:contains('我是div')");
 console.log($div3);
 //:has() 找到包含指定子元素的元素
 var $div4 = $("div:has('span')");
 console.log($div4);
```

```html
<div></div>
<div>我是div</div>
<div>他们是div222</div>
<div><span></span></div>
<div><p></p></div>
```

## 属性和属性节点

### 什么是属性？

对象身上保存的变量就是属性

### 如何操作属性？

1.  对象.属性名称 = 值;
2.  对象.属性名称;
3.  对象\[‘属性名称’\] = 值;
4.  对象\[‘属性名称’\];

```js
function Person(){

}
var p = new Person();
p.name = "qy"
console.log(p.name)
```

### 什么是属性节点？

在编写html代码时在标签中添加的属性就是属性节点。

```html
 <span name = 'qiaoyue'></span>
```

在浏览器中dom元素展开后都是属性 而在 attributrs属性中保持的所有内容都是属性节点

### 如何操作属性节点？

```
DOM元素.setAttribute('属性名称','值');
```

### 属性和属性节点有什么区别？

任何对象都有属性，但只有DOM对象才有属性节点

## attr()和removeAttr()

prop和attr方法一样

```js
$(function(){
    // 1.attr(name｜pro｜key，val｜fn)
    // 作用获取或者设置属性节点的值
    // 可以传递一个参数也可以传递两个参数
    // 如果传递一个参数代表获取属性节点的值
    // 如果传递两个参数 代表设置属性节点的值
    // 注意 无论获取多少个元素 都只会返回第一个元素指定的属性节点的值
    console.log($("span").attr("class"))
    // 如果是设置找到多少个元素就会设置多少个元素
    // 如果设置的属性不存在 系统会自动新增
    console.log($("span").attr("class","box2"))

    // 2.removeAttr() 删除属性节点
    $('span').removeAttr('class');

})
```

```html
<span class="span1" name='qiaoyue1'></span>
<span class="span2" name='qiaoyue2'></span>
```

#### 注意点

prop方法不仅能操作属性，他还能操作属性节点

```js
console.log($('span').prop("class"))
$('span').prop('class','box')
```

选中复选框为true，没选中为false

```js
$("input[type='checkbox']").prop("checked");
```

当属性没有被设置时候，.attr()方法将返回undefined。若要检索和更改DOM属性,比如元素的checked, selected, 或 disabled状态，请使用.prop()方法。

## css类操作

### addClass(class|fn)

为每个匹配的元素添加指定的类名。

参数：

1.  一个或多个要添加到元素中的CSS类名，请用空格分开
2.  此函数必须返回一个或多个空格分隔的class名。接受两个参数，index参数为对象在这个集合中的索引值，class参数为这个对象原先的class属性值。

```js
$("dom元素").addClass("类名");
$("dom元素").addClass("类名1 类名2");
```

### removeClass(\[class|fn\])

从所有匹配的元素中删除全部或者指定的类。

```js
$("dom元素").removeClass("类名");
```

### toggleClass(class|fn\[,sw\])

如果存在（不存在）就删除（添加）一个类。

参数：

1.  CSS类名
2.  要切换的CSS类名.用于决定元素是否包含class的布尔值。

```js
$("dom元素").toggleClass("类名");
```

## html代码文本操作

### html(\[val|fn\])方法

取得第一个匹配元素的html内容。 有参数是设置没有参数是获取

```js
$('元素').html();//获取元素内文本
$("元素").html("Hello <b>world</b>!"); //设置元素内文本
```

## text(\[val|fn\])

取得所有匹配元素的内容。

结果是由所有匹配元素包含的文本内容组合起来的文本。这个方法对HTML和XML文档都有效。

```js
//返回p元素的文本内容。
$('p').text();
//设置所有 p 元素的文本内容
$("p").text("Hello world!");
```

### val(\[val|fn|arr\])

获得匹配元素的当前值。

在 jQuery 1.2 中,可以返回任意元素的值了。包括select。如果多选，将返回一个数组，其包含所选的值。

```js
//获取文本框中的值
$("input").val();
//设定文本框的值
$("input").val("hello world!");
```

## 设置css样式

### css(name|pro|\[,val|fn\])

访问匹配元素的样式属性。

```js
//取得第一个段落的color样式属性的值。
$("p").css("color");
//将所有段落的字体颜色设为红色并且背景为蓝色。
$("p").css({ "color": "#ff0011", 
            "background": "blue" 
           });
```

## 位置和尺寸

#### offset(\[coordinates\])

用于设置或返回当前匹配元素相对于当前文档的偏移，也就是相对于当前文档的坐标。该函数值对可见元素有效。

```js
//获取元素距离窗口的偏移量
$('元素class').offset().left｜top
//设置元素距离窗口的偏移量
$('元素class').offset({
                    left:10
                    top:10
                })
```

#### position()

获取匹配元素相对父元素的偏移。

返回的对象包含两个整型属性：top 和 left。为精确计算结果，请在补白、边框和填充属性上使用像素单位。此方法只对可见元素有效。

#### width(\[val|fn\])

```js
$('.father').width() //获取
$('.father').width('300px') //设置
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>位置尺寸 - 乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            height: 200px;
            background: red;
            border: 50px solid #000;
            margin-left: 50px;
            position: relative;
        }
        .son{
            width: 100px;
            height: 100px;
            background: blue;
            position:absolute;
            left: 50px;
            top: 50px;

        }
    </style>

    <script>

        $(function(){
            //编写相关jq代码
            var btns = document.getElementsByTagName('button');
            //监听
            btns[0].onclick = function (){
               console.log($('.father').width()) 
            //   获取元素距离窗口的偏移量
               $('.father').offset().left
               
            }
            btns[1].onclick = function(){
                // $('.father').width('300px')
                // 设置元素距离窗口的偏移量
                $('.father').offset({
                    left:10
                })
            }


        })



    </script>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>

<button>获取</button>
<button>设置</button>
</body>
</html>
```

#### scrollTop(\[val\])

取匹配元素相对滚动条顶部的偏移。

```js
$("div｜html｜body").scrollLeft(); //获取偏移量
$("div｜html｜body").scrollLeft(100); //设置偏移量
```



[资源下载](https://wwa.lanzous.com/iYWBsdpzxmd)