---
title: 「jQuery」动画&动效学习和练习延时执行淡入淡出动画
categories: 技术笔记
tags:
  - jquery
excerpt: 'jQuery动画 show([speed,[easing],[fn]]) 显示隐藏的匹配元素。'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img6.gif'
articleLink: 3ba71669edc21718
date: 2020-06-18 10:08:17
---

## jQuery动画

### show(\[speed,\[easing\],\[fn\]\])

显示隐藏的匹配元素。

### hide(\[speed,\[easing\],\[fn\]\])

隐藏显示的元素

![「jQuery」动画&动效学习和练习延时执行淡入淡出动画](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img1.gif "「jQuery」动画&动效学习和练习延时执行淡入淡出动画")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jq显示隐藏-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>

    <style>
        *{
            margin: 0;
            padding: 0;
        }

        div{
            width: 200px;
            height: 200px;
            background: red;
            display: none;
        }
    </style>

    <script>

        $(function(){

            $("button").eq(0).click(function(){
                // $("div").css("display","block");
                $("div").show(1000);
            })
            $("button").eq(1).click(function(){
                // $("div").css("display","none");
                $("div").hide(1000);
            })
            $("button").eq(2).click(function(){
                $("div").toggle(1000);
            })  

        })
    </script>
</head>
<body>
    <button>显示</button>
    <button>隐藏</button>
    <button>切换</button>
    <div></div>
</body>
</html>
```

### scroll(\[\[data\],fn\]) 监听网页滚轮

当用户滚动指定的元素时，会发生 scroll 事件。

scroll 事件适用于所有可滚动的元素和 window 对象（浏览器窗口）。

```js
$(window).scroll( function() { /* ...do something... */ } );
```

![「jQuery」动画&动效学习和练习延时执行淡入淡出动画](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img3.gif "「jQuery」动画&动效学习和练习延时执行淡入淡出动画")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>对联广告-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <style>

        *{
            margin: 0;
            padding: 0;
        }
        img{
            width: 250px;
            height: 400px;
            display: none;
        }
        .left{
            float: left;
            position: fixed;
            left: 0;
            top: 200px;
        }
        
        .right{
            float: right;
            position: fixed;
            right: 0;
            top: 200px;
        }
    </style>
    <script>
        $(function(){
            $(window).scroll(function(){
               var offset =  $("html,body").scrollTop();
               if(offset >= 500){
                    $("img").show(1000);
               }else{
                    $("img").hide(1000);
               }
            })
        })
    </script>
</head>
<body>
    <img class="left" src="https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1592379021&di=288d03442624777f9bef0bab14fbe62e&src=http://d.hiphotos.baidu.com/zhidao/pic/item/738b4710b912c8fc40cc855efc039245d6882161.jpg" alt="">
    <img class="right" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1592389104341&di=84f707ab996b60cfbda20ee4da048669&imgtype=0&src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fitem%2F201305%2F12%2F20130512095153_WSu5Q.jpeg" alt="">
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
    <p>乔越博客</p>
</body>
</html>
```

### slideDown(\[speed\],\[easing\],\[fn\])

通过高度变化（向下增大）来动态地显示所有匹配的元素，在显示完成后可选地触发一个回调函数。

这个动画效果只调整元素的高度，可以使匹配的元素以“滑动”的方式显示出来。在jQuery 1.3中，上下的padding和margin也会有动画，效果更流畅。

```js
$(".btn2").click(function(){
  $("p").slideDown("时间｜方法");
});
```

### slideUp(\[speed,\[easing\],\[fn\]\])

通过高度变化（向上减小）来动态地隐藏所有匹配的元素，在隐藏完成后可选地触发一个回调函数。

这个动画效果只调整元素的高度，可以使匹配的元素以“滑动”的方式隐藏起来。

```js
$("p").slideUp("时间｜方法");
```

### slideToggle(\[speed\],\[easing\],\[fn\])

通过高度变化来切换所有匹配元素的可见性，并在切换完成后可选地触发一个回调函数。

```js
$("p").slideToggle("时间｜方法");
```

![「jQuery」动画&动效学习和练习延时执行淡入淡出动画](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img3.gif "「jQuery」动画&动效学习和练习延时执行淡入淡出动画")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>展开收起-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }

        div{
            height: 400px;
            width: 100px;
            background: red;
        }
    </style>

    <script>

        $(function(){
            $("button").eq(0).click(function(){
                $("div").slideDown(1000)
            })
            $("button").eq(1).click(function(){
                $("div").slideUp(1000)
            })
            $("button").eq(2).click(function(){
                $("div").slideToggle(1000)
            })

        })
    </script>
</head>
<body>

    <button>展开</button>
    <button>收起</button>
    <button>切换</button>

    <div></div>

    
</body>
</html>
```

### stop(\[clearQueue\],\[jumpToEnd\])

停止所有在指定元素上正在运行的动画。

如果队列中有等待执行的动画(并且clearQueue没有设为true)，他们将被马上执行。

```
$("#stop").click(function(){
  $("#box").stop();
});
```

在jQuery中如果需要执行动画,建议在执行动画之前先调用 stop 方法 然后在执行动画.

## 淡入淡出

### fadeIn(\[speed\],\[easing\],\[fn\])

淡入

通过不透明度的变化来实现所有匹配元素的淡入效果，并在动画完成后可选地触发一个回调函数。

```js
$("元素").fadeIn("时间"); //淡入
```

### fadeOut(\[speed\],\[easing\],\[fn\])

淡出

```js
$("元素").fadeOut("时间");//淡出
```

### fadeToggle(\[speed,\[easing\],\[fn\]\])

淡入或淡出

```js
$("元素").fadeToggle("时间","效果");
```

### fadeTo(\[\[speed\],opacity,\[easing\],\[fn\]\])

调整透明度

```js
$("元素").fadeTo("slow", 0.66);
// 时间 透明度
```

![「jQuery」动画&动效学习和练习延时执行淡入淡出动画](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5.gif "「jQuery」动画&动效学习和练习延时执行淡入淡出动画")

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>淡入淡出-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>
        
    <style>
        div{
            width: 200px;
            height: 400px;
            background: red;

        }
    </style>
    <script>

        $(function(){

            $("button").eq(0).click(function(){
                // 淡入
                $("div").fadeIn(1000);
            })
            $("button").eq(1).click(function(){
                // 淡出
                $("div").fadeOut(1000);
            })
            $("button").eq(2).click(function(){
                // 调整透明度
                $("div").fadeTo(1000,0.5,function(){
                });
            })
            $("button").eq(3).click(function(){
                // 切换
                $("div").fadeToggle(1000);
            })
            
        })


    </script>
</head>
<body>
    <button>淡入</button>
    <button>淡出</button>
    <button>切换</button>
    <button>淡入到</button>

    <div>

    </div>
</body>
</html>
```

## 自定义动画

### animate(params,\[speed\],\[easing\],\[fn\])

#### 参数

1.  params：一组包含作为动画属性和终值的样式属性和及其值的集合
2.  speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)
3.  easing：要使用的擦除效果的名称(需要插件支持).默认jQuery提供”linear” 和 “swing”.
4.  fn：在动画完成时执行的函数，每个元素执行一次。

![「jQuery」动画&动效学习和练习延时执行淡入淡出动画](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img6.gif "「jQuery」动画&动效学习和练习延时执行淡入淡出动画")

```html
<!--html-->
<button id="go"> Run</button>
<div id="block">Hello!</div>
```

```js
//jquery
// 在一个动画中同时应用三种类型的效果
$("#go").click(function(){
  $("#block").animate({ 
    width: "90%",
    height: "100%", 
    fontSize: "10em", 
    borderWidth: 10
  }, 1000 );
});
```

### delay(时间,_\[队列\]_)

设置一个延时来推迟执行队列中之后的项目。

```js
$('元素').slideUp(300).delay(800).fadeIn(400);
//1. 当slideUp动画执行完毕后
//2. 等待800毫秒后   （简单的说就是前面的执行完需要等会在执行下一个）
//3. 执行fadeIn动画
```

[资源下载](https://wwa.lanzous.com/iyzFods5e3i)