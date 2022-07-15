---
title: 「jQuery」使用jQuery来实现电影排行榜的效果
categories: 项目案例
tags:
  - HTML
  - 个人笔记
  - jquery
  - 电影排行榜
  - 鼠标移入移出
excerpt: 电影排行榜 使用监听移入和移除两个方法 mouseenter() 当鼠标移入的时候 给当前元素 添加class 并显示出来
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgJun-16-2020-18-30-31.gif'
articleLink: 7350e7a929293816
date: 2020-06-16 18:33:38
---

## 电影排行榜

![「jQuery」使用jQuery来实现电影排行榜的效果](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgJun-16-2020-18-30-31.gif "「jQuery」使用jQuery来实现电影排行榜的效果")

1.  使用监听移入和移除两个方法
    1.  mouseenter()
        
        当鼠标移入的时候 给当前元素 添加class 并显示出来 简介
        
        addClass() 此方法为 为每个匹配的元素添加指定的类名。
        
    2.  mouseleave()
        
        当鼠标移出的时候 去除掉当前 class 并隐藏
        
        removeClass() 此方法为 从所有匹配的元素中删除全部或者指定的类。
    
2.  第二种方法就是使用 hover()方法 监控两种操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>电影排行榜-乔越博客</title>
    <!-- 引入jquery库 -->
    <script src="https://www.jq22.com/jquery/jquery-3.3.1.js"></script>


    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 300px;
            height: 450px;
            margin: 50px auto;
            border: 1px solid #000;
        }
        .box>h1{
            font-size: 20px;
            line-height: 35px;
            color: deeppink;
            padding-left: 10px;
            border-bottom: 1px dashed #ccc ;
        }
        ul>li{
            list-style: none;
            padding: 5px 10px;
            border-bottom: 1px dashed #ccc ;

        }
        ul>li:nth-child(-n+3) span{
            background: deeppink;
        }
        ul>li>span{
            display: inline-block;
            width: 20px;
            height: 20px;
            margin-right: 10px;
            background: #ccc;
            text-align: center;
            line-height: 20px;

        }
        .content{
            overflow: hidden;
            display: none;
        }
        .content>img{
            width: 80px;
            height: 120px;
            float: left;
            margin-top:5px ;
        }
        .content>p{
            width: 180px;
            height: 120px;
            float: right;
            font-size: 15px;
        }
        .curren .content{
            display: block;
        }
    </style>
    <script>
        // 编写jquery代码
        $(function(){
            // 监听移入鼠标给他li 添加一个class
            // $("li").mouseenter(function(){
            //     $(this).addClass("curren");
            // })
            // 监听移除鼠标 给li去掉一个类
            // $("li").mouseleave(function(){
            //     $(this).removeClass("curren");
            // })
            // 用hover可以同样监听两个操作
            $("li").hover(function(){
                $(this).addClass("curren");
            },function(){
                $(this).removeClass("curren");
            })
        })

    </script>
</head>
<body>
    <div class="box">
        <h1>电影排行榜-乔越博客</h1>
        <ul>
            <li><span>1</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div>
            </li>
            <li><span>2</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
            <li><span>3</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
            <li><span>4</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
            <li><span>5</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
            <li><span>6</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
            <li><span>7</span>电影名称
                <div class="content">
                    <img src="https://img3.doubanio.com/view/photo/s_ratio_poster/public/p2577340942.jpg" alt="">
                    <p>兰斯与沃尔特，前者是超级炫酷又迷人的间谍，后者负责发明兰斯使用的各种炫酷小道具。危难当头，他们必须团结一致才能拯救世界。</p>
                </div></li>
        </ul>
    </div>
</body>
</html>
```

[资源下载](https://wwa.lanzous.com/iA3QZdqd0if)

