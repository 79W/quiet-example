---
title: HTML用JavaScript写简单计算器教程
categories: 项目案例
tags:
  - HTML
  - JS
  - 计算器
excerpt: 一个比较基础！的js计算器！希望能够帮助到你
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921214913.png'
articleLink: b5d41c41a9f32526
date: 2018-11-26 12:17:25
---


一个比较基础！的js计算器！希望能够帮助到你！

![HTML用JavaScript写简单计算器教程](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c1bf85b5d.jpg "HTML用JavaScript写简单计算器教程")

```
function anshu(shuzi){
            /*VALUE 这个的意思是 将字符串转换为数字*/
            var shuchu = document.getElementById('shu').value;
                document.getElementById('shu').value += shuzi;
        }
    
        function dengyu(){

            var shuchu = document.getElementById('shu').value;
            var jieguo = eval(shuchu);
            /*EVAL 这是 js 提供 计算表达式 1+1  一个函数*/
                document.getElementById('shu').value +='='+ jieguo;

        }
        function qingkong(){
            var shuchu = document.getElementById('shu').value;
                document.getElementById('shu').value = '';

        }
        function tui(){
            var tg = document.getElementById('shu');
            tg.value=tg.value.substring(0,tg.value.length-1);
        }
        function zfh(){
            var zfh = document.getElementById('shu').value;
            if(zfh.charCodeAt(0) == 45){
                document.getElementById('shu').value = zfh.substr(1,zfh.length);
            }
            else{
                document.getElementById('shu').value = '-' + zfh;
            }

        }
        
```

源代码在下面下载!

在 哔哩哔哩上  搜索html计算器  就可以去观看视频教程!

[资源下载](https://www.lanzous.com/i2gllmh)