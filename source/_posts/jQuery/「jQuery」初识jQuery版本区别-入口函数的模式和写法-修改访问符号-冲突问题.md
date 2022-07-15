---
title: '「jQuery」初识jQuery版本区别,入口函数的模式和写法,修改访问符号,冲突问题'
categories: 技术笔记
tags:
  - jquery
excerpt: 初识jQuery jQuery是什么？ jQuery 是一个简洁而快速的 JavaScript\_库，可用于简化事件处理，HTML文档遍历
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200615091256861.png'
articleLink: df53ce5304405415
date: 2020-06-15 09:52:54
---

# 初识jQuery

## jQuery是什么？

![「jQuery」初识jQuery版本区别,入口函数的模式和写法,修改访问符号,冲突问题](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200615091256861.png "「jQuery」初识jQuery版本区别,入口函数的模式和写法,修改访问符号,冲突问题")

jQuery 是一个简洁而快速的 JavaScript 库，可用于简化事件处理，HTML文档遍历，Ajax 交互和动画，以便快速开发网站。jQuery 简化了 HTML 的客户端脚本，从而简化了 Web 2.0 应用程序的开发。

## jQuery与JavaScript的区别

从代码上可以看出来 jquery的代码量更少,更加的清晰可以很好的看出来代码所表示的意思。

```js
//js获取元素
//原生js找到相对的元素
window.onload = function(ev){
  var div1 = document.getElementsByTagName("div")[0];
  var div2 = document.getElementsByClassName("jqc1")[0];
  var div3 = document.getElementById("jqi1");
  //原生js 改变背景颜色 css
  div1.style.background = "red";

  console.log(div1);
  console.log(div2);
  console.log(div3);
};
```

```js
//jq获取元素
//jquery找到相对应的元素
//首先要导入jquery库
$(function(){
  var $div1 = $('div');
  var $div2 = $('.jqc1');
  var $div3 = $('#jqi1');
  //jquery 修改css
  $div2.css({
    background:"red",
    width:"200px"
  });
  console.log($div1);
  console.log($div2);
  console.log($div3);
});
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jquery初识</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            border: 1px solid #333333;
        }
    </style>
    <script>
        //原生js找到相对的元素
        window.onload = function(ev){
            var div1 = document.getElementsByTagName("div")[0];
            var div2 = document.getElementsByClassName("jqc1")[0];
            var div3 = document.getElementById("jqi1");
            //原生js 改变背景颜色 css
            div1.style.background = "red";

            console.log(div1);
            console.log(div2);
            console.log(div3);
        };

        //jquery找到相对应的元素
        //首先要导入jquery库
        $(function(){
            var $div1 = $('div');
            var $div2 = $('.jqc1');
            var $div3 = $('#jqi1');
            //jquery 修改css
            $div2.css({
                background:"red",
                width:"200px"
            });
            console.log($div1);
            console.log($div2);
            console.log($div3);
         });
    </script>
</head>
<body>
    <div></div>
    <div class="jqc1"></div>
    <div id="jqi1"></div>
</body>
</html>
```

## jQuery的版本区别

有1.X,2.X,3.X版本  
如果考虑兼容IE 6/7/8 需要bai使用2.X以下du(不包含)  
不同版本之间大体功能区别不大,少部zhi分函数移除或修改dao  
同版本分xx.js和xx.min.js，.js的文件为学习版，体积稍大，.min.js为压缩版/开发版，体积小

## jQuery和js入口函数的区别

### 原生js和jq入口函数的加载模式不一样

1.  原生js会等到dom元素加载完毕 并且图片也加载完毕才执行
2.  jq会等到dom加载完毕，但不会等到图片也加载完毕就会执行。

```js
 //js
 window.onload = function(ev){
   //1.通过js原生入口函数获取图片
   var img = document.getElementsByTagName("img")[0];
   console.log(img);
   //2.通过原生js可以获取图片的宽高
   var w = window.getComputedStyle(img).width;
   console.log(w)
 }
 //  jq
 $(document).ready(function(){
   //1. 通过jq入口函数获取图片
   var $img = $('img');
   console.log($img)
   //2.jq获取图片宽高
   //jq入口函数不可以获取dom元素的 宽高
   var $w = $img.width();
   console.log($w)
})
```

1.  原生js如果编写多个入口函数后面的会覆盖前面的
2.  jq中编写多个入口函数 后面的不会覆盖前面的

```js
//js
window.onload = function(ev){
	alert('hello 1')
};
window.onload = function(ev){
	alert('hello 2')
};
//jq
$(document).ready(function(){
	alert('h1')
})
$(document).ready(function(){
	alert('h2')
})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jq-乔越博客</title>

     <!-- 引入jquery库 -->
     <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>

     <script>
         //js
         window.onload = function(ev){
             //1.通过js原生入口函数获取图片
             var img = document.getElementsByTagName("img")[0];
             console.log(img);
             //2.通过原生js可以获取图片的宽高
             var w = window.getComputedStyle(img).width;
             console.log(w)
         }
        //  jq
         $(document).ready(function(){
            //1. 通过jq入口函数获取图片
             var $img = $('img');
             console.log($img)
             //2.jq获取图片宽高
             //jq入口函数不可以获取dom元素的 宽高
             var $w = $img.width();
             console.log($w)

         })

        /*
        原生js和jq入口函数的加载模式不一样
        原生js会等到dom元素加载完毕 并且图片也加载完毕才执行
        jq会等到dom加载完毕，但不会等到图片也加载完毕就会执行。
        */
       /*
       原生js如果编写多个入口函数后面的会覆盖前面的
       jq中编写多个入口函数 后面的不会覆盖前面的
       */
      //js
      window.onload = function(ev){
          alert('hello 1')
      };
      window.onload = function(ev){
          alert('hello 2')
      };
      //jq
      $(document).ready(function(){
          alert('h1')
      })
      $(document).ready(function(){
          alert('h2')
      })
     </script>
</head>
<body>

    <img src="https://i.loli.net/2020/06/15/6Od7xMFWqs3hw4I.png" alt="">

</body>
</html>
```

## jQuery其他入口函数的写法

```js
 //第一种写法
 $(document).ready(function(){
		alert('hello1')
 })
 //第二种写法
 jQuery(document).ready(function(){
 		alert('hello2')
 })
 //第三种写法（推荐）
 $(function(){
 		alert('hello 3')
 })
 //第四种写法
 jQuery(function(){
 		alert('hello 3')
 })
```

## jQuery冲突问题

```js
//1.释放$的使用权
//注意点 释放操作必须在编写其他jq代码之前
//释放之后不能在使用$用jquery
//jQuery.noConflict()  //自定义一个访问符号
var qy = jQuery.noConflict();
qy(function(){
		alert("新符号")
})
```

[资源下载](https://wwa.lanzous.com/irrPcdom1fa)