---
title: '「jQuery」基础知识 核心函数jquery对象的理解对静态方法each,map进行练习'
categories: 技术笔记
tags:
  - HTML
  - jquery
excerpt: jQuery核心函数 接受一个函数 接受一个字符串 接受一个字符串选择器 返回一个jquery对象对象中保存了dom元素
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img%E6%A0%B8%E5%BF%83%E5%87%BD%E6%95%B0-20200916134202773.png'
articleLink: b21b6c9c11984515
date: 2020-06-15 16:21:45
---

## jQuery核心函数

1.  接受一个函数
2.  接受一个字符串
    1.  接受一个字符串选择器
        
        返回一个jquery对象对象中保存了dom元素
        
    2.  接受一个字符串代码片段
3.  接收一个dom元素
    
    会被包装成一个jQuery在返回
    

![「jQuery」基础知识 核心函数jquery对象的理解对静态方法each,map进行练习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img%E6%A0%B8%E5%BF%83%E5%87%BD%E6%95%B0-20200916134202773.png "「jQuery」基础知识 核心函数jquery对象的理解对静态方法each,map进行练习")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jq核心函数-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <script>
        //$()就代表调用jQuery核心函数
        //1. 接受一个函数
        $(function(){
            alert('hello');
            // 2.接受一个字符串
            //2.1 接受一个字符串选择器
            //返回一个jquery对象对象中保存了dom元素 
            var $div1 = $('.box1');
            var $div2 = $('#box2');
            console.log($div1);
            console.log($div2);
            //2.2接受一个字符串代码片段
            var $p = $("<p>我是段落</p>");
            console.log($p);
            $div1.append($p);

            // 3.接收一个dom元素
            //会被包装成一个jQuery在返回
            var span = document.getElementsByTagName('span')[0];

            $span = $(span);
            console.log($span)
        })
    </script>
</head>
<body>
    <div class="box1"></div>
    <div id="box2"></div>
    <span>我是一个span</span>
</body>
</html>
```

## jQuery对象

### 什么是jquery对象

jquery是一个伪数组

### 什么是伪数组

有0到length-1的属性，并且有length属性

![「jQuery」基础知识 核心函数jquery对象的理解对静态方法each,map进行练习](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgjq%E5%AF%B9%E8%B1%A1-20200916134212441.png "「jQuery」基础知识 核心函数jquery对象的理解对静态方法each,map进行练习")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jq对象-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <script>
        // 入口函数
        $(function(){
            /*
            1.什么是jquery对象
                jquery是一个伪数组
            2.什么是伪数组
                有0到length-1的属性，并且有length属性
            */
            // 获取全部的div元素
            var $div = $('div');
            console.log($div);

            var arr = [1,2,4,67,3];
            console.log(arr)
        })

    </script>

</head>
<body>
    <div>div1</div>
    <div>div2</div>
    <div>div3</div>
    <div>div4</div>
</body>
</html>
```

## 实例方法和静态方法

### 原生js中的静态方法和实例方法

```js
//    1.定义一个类
       function AClass(){

       }
    //    2.给这个类添加一个静态方法
    // 直接添加给类的静态方法
    AClass.staticMethod = function(){
        alert('staticMethod');
    }
    //静态方法的调用
    AClass.staticMethod();
    //  3.给这个类添加一个实例方法
    AClass.prototype.instanceMethod = function(){
        alert('instanceMethod');
    }
    //实例方法通过类的实例调用
    //创建一个实例（创建一个对象）
    var a = new AClass();
    //通过实例的调用方法
    a.instanceMethod();
```

### 原生each遍历和jQuery的区别

1.  原生forEach 第一个参数为 元素 第二个参数为 索引 并且不可以遍历伪元素
2.  jQuery each 第一个参数为 索引 第二个参数为 元素 并且可以 遍历伪元素

```js
        var arr = [1,3,4,5,67,7]
        // 第一个参数 元素
        // 第二个参数 索引
        //注意：原生foreach方法只能遍历数组，不能遍历伪数组
        arr.forEach(function(v,i){
            console.log(i,v)
        })
        // 伪数组
        var wei = {0:1,1:3,2:5}
        // 遍历
        // wei.forEach(function(v,i){
        //     console.log(i,v)
        //     // 报错
        // })

        // 1.利用 jQuery的each静态方法遍历数组
        // 第一个参数 索引
        // 第二个参数 元素
        $.each(arr,function(index,value){
            console.log(index,value)
        })
        console.log("-----------")
        // 注意点：jQuery each可以遍历伪数组
        $.each(wei,function(index,value){
            console.log(index,value)
        })
```

### jQuery中map方法和原生map方法的区别

```JS
				// 数组
         var arr = [1,3,4,5,67,7]
         // 伪数组
        var wei = {0:1,1:3,2:5}
        // 1.用原生js的map遍历
        arr.map(function(value,index,array){
            // 参数为 元素 索引 数组
            console.log(index,value,array);
        })
        // map也是不可以遍历伪数组
        // wei.map(function(value,index,array){
        //     // 参数为 元素 索引 数组
        //     console.log(index,value,array);
        // })

        // jQuery调 map
        // 参数 数组 回掉函数（元素 索引）
        // jQuery可以遍历伪数组
        $.map(arr,function(value,index){
            console.log(index,value)
        })
        // 伪数组 可以遍历
        $.map(wei,function(value,index){
            console.log(index,value)
        })

        //jQuery中的map和each
        // 返回值 each返回数组遍历那个数组就返回谁
        // map 返回空数组 可以进行加工返回新的数组

       var ea =  $.each(arr,function(index,value){
            console.log(index,value)
        })
        console.log(ea)

```

### 其他静态方法

#### trim方法

去除字符串两端的空格

```js
var str = "   stali   ";
// 这样打印是有空格的
console.log("---"+str+"---")
// 去掉空格
var res = $.trim(str);
// 没有空格了
console.log("---"+res+"---")
```

#### isWindow方法

$.isWindow() 函数用于判断指定参数是否是一个window窗口。

```
$.isWindow( object )
```

#### isArray方法

$.isArray()函数用于判断指定参数是否是一个数组。

```
$.isArray( object )
```

#### isFunction方法

$.isFunction()函数用于判断指定参数是否是一个函数。

\*\*注意：\*\*jQuery 1.3之后的版本，例如在 Internet Explorer 中，由浏览器提供的函数alert()，以及 DOM 元素方法(比如 getAttribute())将不被认为是函数。

```
$.isFunction( object )
```

## holdReady方法

_$_.holdReady() 函数用于暂停或恢复.ready() 事件的执行。

**注意：**

1.  该方法必须在文档靠前部分被调用，例如，在头部加载完 jQuery 脚本之后，立刻调用该方法。如果在 ready 事件已经被调用后再调用该方法，将不会起作用。
2.  首先调用.holdReady(true)\[调用后ready事件将被锁定\]。当准备好执行ready事件时，调用.holdReady(true)\[调用后ready事件将被锁定\]。当准备好执行ready事件时，调用.holdReady(false)。
3.  可以对 ready 事件添加多个锁定，每个锁定对应一次$.holdReady(false)\[解锁\]调用。ready 事件将在所有的锁定都被解除，并且页面也已经准备好的情况下被触发。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <script>
        // 抑制执行
        $.holdReady(true);
        $(document).ready(function(){
            alert('开始')
        });
    </script>
</head>
<body>
<button>ready事件</button>
<script>
    var ban = document.getElementsByTagName('button')[0];
    ban.onclick = function(){
        // 执行
        $.holdReady(false);
    }
</script>
</body>
</html>
```

[资源下载](https://wwa.lanzous.com/iIN0Tdp3taf)