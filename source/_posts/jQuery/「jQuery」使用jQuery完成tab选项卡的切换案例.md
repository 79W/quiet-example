---
title: 「jQuery」使用jQuery完成tab选项卡的切换案例
categories: 项目案例
tags:
  - HTML
  - 个人笔记
  - jquery
  - tab
  - 选项卡
excerpt: tab选项卡 需要监听它的移入 这里使用 mouseenter ()方法监听鼠标的移入操作 鼠标移入后 addClass() 使用此方法添加类
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916133515.png'
articleLink: 87e38b45f41e1716
date: 2020-06-16 18:44:17
---

## tab选项卡

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200916133515.png)

1.  需要监听它的移入
    1.  这里使用 mouseenter ()方法监听鼠标的移入操作
2.  鼠标移入后 addClass() 使用此方法添加类 class
3.  **siblings()** 
    1.  使用此方法找到同级别的其他元素
    2.  并使用 removeClass()方法删除
    3.  取得一个包含匹配的元素集合中每一个元素的所有唯一同辈元素的元素集合。
    4.  可以用可选的表达式进行筛选。
4.  index() 获取当前元素的索引
5.  eq() 匹配一个给定索引值的元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>tab选项卡-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>

    <style>

        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 440px;
            height: 298px;
            border: 1px solid #ccc;
            margin: 50px auto;
        }
        .nav>li{
            list-style: none;
            width: 110px;
            height: 50px;
            background: orchid;
            text-align: center;
            line-height: 50px;
            float: left;
        }
        .nav .curr{
            background: #ccc;
        }
        /* --------- */
        .content>li{
            list-style: none;
            display: none;
        }
        .content>.show{
            display: block;
        }
    </style>

    <script>

        $(function(){
            // 笨方法
            // $(".nav>li").mouseenter(function(){
            //     $(this).addClass("curr");
            //     var index = $(this).index();
            //     var $li = $('.content>li').eq(index);
            //     $li.addClass('show')
            // })
            // $(".nav>li").mouseleave(function(){
            //     $(this).removeClass("curr");
            //     var index = $(this).index();
            //     var $li = $('.content>li').eq(index);
            //     $li.removeClass('show')
            // })
            
            // 优秀方法
            // 监听移入方法
            $(".nav>li").mouseenter(function(){
                // 给当前移入的 加上class 
                $(this).addClass("curr");
                //查找同级别其他元素并删除 class
                // 重点 siblings()方法
                $(this).siblings().removeClass("curr");
                // 获取当前元素的索引
                // 重点 index()
                var index = $(this).index();
                // 获取当前索引的图片元素
                // 重点eq()
                var $li = $('.content>li').eq(index);
                // 给图片li加入class
                $li.addClass('show')
                //查找同级别其他元素并删除 class
                // 重点 siblings()方法
                $li.siblings().removeClass("show");
            })


        })

    </script>
</head>
<body>

    <div class="box">
        <ul class="nav">
            <li class="curr">第一个</li>
            <li>第二个</li>
            <li>第三个</li>
            <li>第四个</li>
        </ul>
        <ul class="content">
            <li class="show"><img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt=""></li>
            <li><img src="https://img9.doubanio.com/view/photo/s_ratio_poster/public/p2565068596.jpg" alt=""></li>
            <li><img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2522880251.jpg" alt=""></li>
            <li><img src="https://img1.doubanio.com/view/photo/s_ratio_poster/public/p792776858.jpg" alt=""></li>
        </ul>
    </div>
    
</body>
</html>
```

[资源下载](https://wwa.lanzous.com/iaoVedqd9ja)

