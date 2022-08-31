---
title: jQuery编写锅打灰太狼小游戏
categories: 项目案例
tags:
  - jQuery
  - 小游戏
excerpt: jQuery编写锅打灰太狼小游戏
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922111108.png'
articleLink: bda4da0f47665924
date: 2020-06-24 19:40:59
---

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922110649.png)

### 思路分析

1. 要有开始和结束的方法
2. 计算比分
3. 时间的进度条
4. 小灰灰和灰太狼显示
   1. 随机位置显示
   2. 打小灰灰减掉10分
   3. 打到灰太狼加上10分

### 计时器

首先规定这个进度条的长度然后开启定时器 在这宽度上进行递减

这是两层最下面的一层为 灰色 

上面一层为带颜色的可以看出来正在走

时间走完后删除狼等图片 

将重新开始的进行显示出来

```
function progressH(){
        // 规定进度条宽度
        $(".progress").css({
            width:180
        })
        //开启定时器
        var time = setInterval(function(){
            //拿到进度条的宽度
            var progH = $(".progress").width();
            // 进度条递减
            progH -= 1;
            // 赋值回去
            $(".progress").css({
                width:progH
            })
            // 监听进度条没了 开启重新开始界面
            if(progH <= 0){
                // 已经走完了进度条
                //关闭定时器
                clearInterval(time);
                // 删除狼的图片
                stopl();
                // 开启冲重新开始界面
                $(".mask").stop().fadeIn(100);
                // 结束后显示得到的分数
                $(".mask>h3>span").text($(".score").text());
            }
        },100)
    }
```

### 随机显示狼

用一个随机显示

是选择灰太狼还是小灰灰 

选中后在随机选择个位置进行 慢慢进行添加到页面上

```
    // 随机图片灰太狼的方法
    function foridanim(){
        // 灰太狼
       var lang_1 = ['./images/h0.png',
                     './images/h1.png',
                     './images/h2.png',
                     './images/h3.png',
                     './images/h4.png',
                     './images/h5.png',
                     './images/h6.png',
                     './images/h7.png',
                     './images/h8.png',
                     './images/h9.png'
                    ];
        // 小灰灰
       var lang_2 = ['./images/x0.png',
                     './images/x1.png',
                     './images/x2.png',
                     './images/x3.png',
                     './images/x4.png',
                     './images/x5.png',
                     './images/x6.png',
                     './images/x7.png',
                     './images/x8.png',
                     './images/x9.png'
                    ];
        // 位置
        var arrPos = [
            {left:"100px",top:"115px"},
            {left:"20px",top:"160px"},
            {left:"190px",top:"142px"},
            {left:"105px",top:"193px"},
            {left:"19px",top:"221px"},
            {left:"202px",top:"212px"},
            {left:"120px",top:"275px"},
            {left:"30px",top:"295px"},
            {left:"209px",top:"297px"},
        ];
    
        // 获取随机位置
        var mart = Math.round(Math.random()*8);
        // 创建一个图片
        $wolfow = $("<img src='' class= 'limg'>");
        console.log(arrPos[mart].left,arrPos[mart].top)
        // 显示位置
        $wolfow.css({
            position:"absolute",
            left:arrPos[mart].left,
            top:arrPos[mart].top
        })
        // 获取随机狼
        var lmart = Math.round(Math.random()) == 0?lang_1:lang_2;
        
        // 设置动画
        window.lindex = 0;
        window.lindexend = 5;
        // 设置定时器 更好图片
        ltime = setInterval(function(){
            // 第五张以后不要了
            if(lindex > lindexend){
                // 删除图片
                $wolfow.remove();
                // 关闭定时器
                clearInterval(ltime);
                foridanim();
                lindex = 0
            }
            // 设置图片内容
            $wolfow.attr("src",lmart[lindex]);
            lindex ++;
        },150);
        //  添加到界面上
        $(".centent").append($wolfow);
        // 点击图片的游戏规则
        gameRule($wolfow);

    };
```

### 游戏规则

主要是进行分数的计算等操作

```
    // 处理游戏规则
    function gameRule($wolfow){
       $($wolfow).one("click",function(){
        //    修改索引
        window.lindex = 5;
        window.lindexend = 9;
        //   拿到当前图片的地址
           var $src = $(this).attr("src");
        //    根据图片地址查看是灰太狼还是小
            var flas = $src.indexOf("h") >= 0;
            // 根据点击的图片来增减分数
            if(flas){
                // 点击的是灰太狼就给加10分
                var scmun = parseInt($(".score").text()) + 10;
                $(".score").text(scmun)
            }else{
                // 点击的是小灰灰就给-10分
                var scmun = parseInt($(".score").text()) - 10;
                $(".score").text(scmun)
            }
       })
    };
```

[资源下载](https://wwa.lanzous.com/iZMSEgtvxhg)