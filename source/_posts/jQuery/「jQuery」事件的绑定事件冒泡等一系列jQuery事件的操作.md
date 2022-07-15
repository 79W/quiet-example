---
title: 「jQuery」事件的绑定事件冒泡等一系列jQuery事件的操作
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - jquery
  - 事件
  - 事件冒泡
  - 事件绑定
excerpt: 事件绑定 eventName(fn) 编码效率高部分事件jquery没有的不可以使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916133230.png'
articleLink: 1d3d0541f4343816
date: 2020-06-16 15:29:38
---

## 事件绑定

1.  eventName(fn) 编码效率高部分事件jquery没有的不可以使用
    
    ```js
    $('button').click(function(){
    		alert("点击")
    })
    ```
    
2.  on(eventName,fn) 编码效率低 所有js事件都可以添加
    
    ```js
    $("button").on('click',function(){
    		alert("点击2")
    })
    ```
    

可以添加多个相同或者不同的事件，不会被覆盖。

## 事件解绑

在选择元素上移除一个或多个事件的事件处理函数。

```js
$('button').off();//不传递参数 全部移除
$('button').off('click');//移除指定的事件
$('button').off('click',方法名);//移除指定的事件，指定类型
```

## 事件冒泡

1.  当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window 。（注意这里传递的仅仅是事件 并不传递所绑定的事件函数。所以如果父级没有绑定事件函数，就算传递了事件 也不会有什么表现 但事件确实传递了。）
2.  简单的说就是 父亲和儿子
    1.  当点击的是父亲那么就会父亲一个方法响应
    2.  当点击的是儿子那么就会连带这父亲一起响应
3.  阻止事件冒泡
    1.  可以在儿子当中使用 return false
    2.  也可以使用 event对象中的方法
        1.  event.stopPropagation();

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件冒泡-乔越博客</title>
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

        }
        .son{
            width: 100px;
            height: 100px;
            background: blue;
        }
    </style>

    <script>
        $(function(){
            $(".son").click(function(event){
                alert("son");
                //阻止冒泡
                // return false;
                event.stopPropagation();
            })
            $(".father").click(function(){
                alert('father')
            })
        })

    </script>
</head>
<body>
    <div class="father">
        <div class="son"></div>
    </div>
    <a href="https://www.79bk.cn">乔越博客</a>
    <form action="https://me.csdn.net/imabug">
        <input type="text">
        <input type="submit">
    </form>
</body>
</html>
```

## 默认行为

1.  默认行为就是一些 a标签和表单 默认事件
2.  我没有给a标签设置跳转到浏览器 而它自己就跳转了
3.  阻止默认行为
    1.  第一个方法就是使用 return false
    2.  event对象中的
        1.  event.preventDefault();

```js
$(function(){
      $('a').click(function(event){
            alert('tan');
            // 阻止默认行为
            // return false;
            event.preventDefault();
      })
})
```

## 自动事件

1.  trigger()
    
    1.  会触发事件冒泡
    
    ```js
    $("dom元素").trigger('事件名称');
    ```
    
2.  triggerHandler()
    
    1.  不会触发事件冒泡
    
    ```js
    $("dom元素").triggerHandler('事件名称');
    ```
    

#### 面试题

1.  用trigger（）自动执行a标签不会跳转
2.  需要用trigger执行span标签才会跳转  
    ![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916133230.png)

```
<a href="https://www.79bk.cn">乔越博客</a>
<a href="https://www.79bk.cn"><span>乔越博客</span></a>
```

## 自定义事件

1.  事件必须通过on（）来绑定
2.  事件必须通过 trigger来执行  
    ![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916133246.png)

## 事件命名空间

1.  需要使用 on来绑定
2.  在事件名称后面加上 .名字 就可以
3.  调用的时候就是 事件.名字

```Js
$('.son').on("click.qy",function(){
		console.log("这个是 qy 的方法 命名空间")
})
$('.son').trigger("click.qy");
```

## 事件委托

delegate()来解决事件委托的问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>事件委托-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <script>

        $(function(){
            $("button").click(function(){
                $("ul").append("<li>我是第4个li</li>");
            })
            
            // $("ul>li").click(function(){
            //     // 这个方法 当新增li就不会给新增的li加上click事件
            //     console.log($(this).html())
            // })

            // 事件委托
            $('ul').delegate("li","click",function(){
                // 监听的是ul 不是li
                console.log($(this).html())
            })

        })

    </script>
</head>
<body>
    <ul>
        <li>我是第1个li</li>
        <li>我是第2个li</li>
        <li>我是第3个li</li>
    </ul>
    <button>新增一个li</button>
</body>
</html>
```

## 移入移出事件

### mouseout(\[\[data\],fn\])

当鼠标指针从元素上移开时，发生 mouseout 事件。

```js
$("p").mouseout(function(){
  $("p").css("background-color","#E9E9E4");
});
```

### mouseover(\[\[data\],fn\])

当鼠标指针位于元素上方时，会发生 mouseover 事件。

```js
$("p").mouseover(function(){
  $("p").css("background-color","yellow");
});
```

### mouseleave(\[\[data\],fn\])

当鼠标指针离开元素时，会发生 mouseleave 事件。

```js
$("p").mouseleave(function(){
  $("p").css("background-color","#E9E9E4");
});
```

### mouseenter(\[\[data\],fn\])

当鼠标指针穿过元素时，会发生 mouseenter 事件。

```js
$("p").mouseenter(function(){
  $("p").css("background-color","yellow");
});
```

### hover(\[over,\]out)

over:鼠标移到元素上要触发的函数

out:鼠标移出元素要触发的函数

一个模仿悬停事件（鼠标移动到一个对象上面及移出这个对象）的方法。这是一个自定义的方法，它为频繁使用的任务提供了一种“保持在其中”的状态。

```js
$("td").hover(
  function () {
    $(this).addClass("hover");
  },
  function () {
    $(this).removeClass("hover");
  }
);
```

[资源下载](https://wwa.lanzous.com/iRQmJdq6n0d)