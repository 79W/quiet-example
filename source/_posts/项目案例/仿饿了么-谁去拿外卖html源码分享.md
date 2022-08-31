---
title: 仿饿了么-谁去拿外卖html源码分享!
categories: 项目案例
tags:
  - HTML
  - 资源分享
  - 饿了么
excerpt: 喝酒没有游戏玩懒得下床不想出去那么好这个游戏会满足你!
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c15b91b1b-20200916235148145.jpg
articleLink: ddacbb5a39150705
date: 2018-12-05 18:27:07
---


喝酒 没有游戏玩？

懒得下床 不想出去 那么好 这个游戏会 满足你!

![仿饿了么-谁去拿外卖html源码分享!](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c15b91b1b-20200916235148145.jpg "仿饿了么-谁去拿外卖html源码分享!")![仿饿了么-谁去拿外卖html源码分享!](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c16a30f0c.jpg "仿饿了么-谁去拿外卖html源码分享!")

玩法

每人都选择一个序号  4 个人为例 

 张三选第 ① 

李四选第 ②

王五选第 ③

赵前选第 ④

然后就按 4 下  其中最小的数对应的序号就是他输了就去拿外卖！

备注：在你们几个同时订外卖的时候在用免的被打！

**部分代码原理很简单**

```
(function () {
    var button = document.getElementsByTagName('button')[0];
    var ullist = document.getElementsByTagName('ul')[0];
    var arrList = [];
    var close = document.getElementById('close');
    var mask = document.getElementsByClassName('mask')[0];
    var icobtn = document.getElementsByClassName('ico')[0];
    var min=NaN;
    var index;
    close.onclick = function () {
        mask.style.display = 'none';
        // console.log('ok')
    }
    icobtn.onclick = function () {
        mask.style.display = 'block';
    }
    button.onmousedown = function () {
        this.style.backgroundPosition = '0 ' + (-80) + 'px';
        cteatNumer()
        this.onmouseup = function () {
            this.style.backgroundPosition = '0 ' + (-40) + 'px';
        }
    };
    button.onmouseenter = function () {
        this.style.backgroundPosition = '0 ' + (-40) + 'px';
        this.onmouseleave = function () {
            this.style.backgroundPosition = '0 ' + 0 + 'px';
        }
    };
//鼠标滑过的时候背景变化


    Array.prototype.min = function () {
        var min = this[0];
        var len = this.length;
        for (var i = 1; i < len; i++) {
            if (this[i] < min) {
                min = this[i];
            }
        }
        return min;
    }
    //数组取最小值
    function cteatNumer() {
        var num = Math.floor(Math.random() * 100);
        if(min == num){
            cteatNumer();
            return;
        }     //判断是否有最小值重复   
        arrList.push(num);
        if (arrList.length > 11) {
            if (num > min && index == 0) {
                arrList.splice(1, 1);
            } else {
                arrList.shift();
            }
            console.log(arrList);
        }
        console.log(arrList.min());
       
        min = arrList.min();//最小值
        index = arrList.indexOf(min);//最小值在数组中的索引
        refurbishDom(arrList, index);
    }
    
    function refurbishDom(arr, index) {
        ullist.innerHTML = '';
        var len = arr.length;
        
        for (var i = 0; i < len; i++) {
            
            ullist.innerHTML += '<li>' + '扔出了一个' + arr[i] + '</li>';
        }
        ullist.getElementsByTagName('li')[index].className = 'takeout-list';
    }
})()
```

[资源下载](https://www.lanzous.com/i2jbd6b)

