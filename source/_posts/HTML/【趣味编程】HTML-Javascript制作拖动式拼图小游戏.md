---
title: 【趣味编程】HTML+Javascript制作拖动式拼图小游戏
categories: 项目案例
tags:
  - HTML
  - 趣味编程
  - JS
  - 小游戏
  - 拖动
  - 拼图
excerpt: 趣味编程拼图小游戏
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c0914c6d7.jpg'
articleLink: 8f0a908de7a75715
date: 2018-12-15 15:19:57
---


![【趣味编程】HTML+Javascript制作拖动式拼图小游戏](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c0914c6d7.jpg "【趣味编程】HTML+Javascript制作拖动式拼图小游戏")

![【趣味编程】HTML+Javascript制作拖动式拼图小游戏](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5cc2c09add26f.jpg "【趣味编程】HTML+Javascript制作拖动式拼图小游戏")

```
var yinruDiv = document.getElementById('xianshi');
var wendangsuipian;
var laoshuzu = [];
var divshuzu = [];
var newshuzu = [];
var yidong = [];
var zindex = 5;
var fugai ;
/**
 * 
 */

    var chang ;
    var kuan ;
    var ppxx ;



    document.getElementById("ruozhi").onclick = function(){
        var yincang = document.getElementById("yincangtu");
        yincang.style.display = "none";
         chang = 3;
         kuan = 3;
         ppxx = 200;
        
       kaishiyouxianniu();//开始游戏按钮 函数
    }
    

document.getElementById("kaishiyouxi").onclick = function(){
    var yincang = document.getElementById("yincangtu");
    yincang.style.display = "none";
     chang = 4;
     kuan = 4;
     ppxx = 150;
    
   kaishiyouxianniu();//开始游戏按钮 函数
}


document.getElementById("gaoji").onclick = function(){
    var yincang = document.getElementById("yincangtu");
    yincang.style.display = "none";
    
     
     kuan = 8 ;
     chang = 8;
     ppxx = 75;
     kaishiyouxianniu();//开始游戏按钮 函数
     
     return
  }


  document.getElementById("bt").onclick = function(){
    
    var yincang = document.getElementById("yincangtu");
    yincang.style.display = "none";
     
    kuan = 15 ;
    chang = 15;
    ppxx = 40;
    kaishiyouxianniu();//开始游戏按钮 函数
    
    return
 }



function kaishiyouxianniu(){
    console.log(chang);
    console.log(kuan);


/*  开始就生成图的 开始 */
    kaishi(chang,kuan);
    function kaishi(){
        console.log(chang);
        console.log(kuan);
        //给老数组赋值
       for(var i=1 ; i<= chang * kuan; i++) {
        laoshuzu.push(i);
       }
        newshuzu = laoshuzu.slice(0);//赋值新数组
        wendangsuipian = document.createDocumentFragment();//文档碎片
        for (var i = 0 ; i< chang * kuan ; i++){
            var x = -((i% chang ) * ppxx);
            var y = -(Math.floor(i/ chang ) * ppxx );
    
            var div = document.createElement("div");
            div.style.cursor="move";
            div.style.backgroundImage="move";
            /*  
            https://ws3.sinaimg.cn/large/005BYqpgly1fy4389jd0cj30go0go119.jpg
            https://unsplash.it/600/600/?random
            */
            div.style.backgroundImage="url(https://unsplash.it/600/600/?random)";
            div.style.backgroundRepeat="no-repeat";
            div.style.float="left";
            div.style.height= ""+ ppxx +"px" ;
            div.style.width= ""+ ppxx +"px" ;
            div.style.backgroundPosition=""+ x +"px "+ y +"px";
            divshuzu.push(div);
            wendangsuipian.appendChild(div);
        }
        yinruDiv.appendChild(wendangsuipian);
    }
  
  


/*  开始就生成图的 结束 */


    //开始计时
    clearInterval(timer);
    // setInterval() 方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。
    timer=setInterval(function () 
    {
        n++;
        var m=parseInt(n/3600)
        var s=parseInt(n/60%60);
        var M=parseInt(n%60);
        oTxt.value=toDub(m)+"分"+toDub(s)+"秒";
        /*+toDub(M)+"秒"*/
    },1000/60);
//开始结束

    //打乱图片 操作
    
    newshuzu.sort(function(a,b){
        return Math.random() > 0.5 ?1 :-1;
    
    })
    for(var i = 0;i< chang*kuan ;i++){
        yidong[i] = [divshuzu[i].offsetLeft,divshuzu[i].offsetTop];
    }
    
    
    for(var i= 0 ;i<chang*kuan ;i++){
       var xini = newshuzu[i]-1;
       //0-8
       var x = -(xini%chang) * ppxx;
       var y = -Math.floor((xini/chang))*ppxx;

       divshuzu[i].style.left = yidong[i][0] + "px";
       divshuzu[i].style.top = yidong[i][1] + "px";
       divshuzu[i].style.position = "absolute";
       divshuzu[i].style.backgroundPosition=""+ x +"px "+ y +"px";
       divshuzu[i].index = i;//访问下标
       divshuzu[i].onmousedown = shubiao;
        }
    
    
    }
    //检查是否覆盖
    function jiancha_fugai(diandiv3,diandiv4){

        //diandiv3 使我们拖动的div
        //diandiv4 是我们循环的div
        if(diandiv3 == diandiv4){
        return;
    }


    var l1 = diandiv3.offsetLeft;
    var t1 = diandiv3.offsetTop;
    var r1 = diandiv3.offsetWidth + l1;
    var b1 = diandiv3.offsetHeight + t1;

    var l2 = diandiv4.offsetLeft;
    var t2 = diandiv4.offsetTop;
    var r2 = diandiv4.offsetWidth + l2;
    var b2 = diandiv4.offsetHeight + t2;
    //如果判断我拖动 是图是否覆盖!


        if( l1 > r2 || t1 > b2 || r1 < l2 || b1 < t2){
        return false;
    }
        else{
        return  true;
    }
        

    }
    function dis(diandiv3,diandiv4){
        var l1 = diandiv3.offsetLeft - diandiv4.offsetLeft;
        var l2 = diandiv3.offsetTop - diandiv4.offsetTop;

        return Math.sqrt(l1*l1 + l2*l2);

    }



    // 找到 要覆盖的图 ！
    function linjufugai(diandiv2){

        var index_nei = -1;
        var imin = 9999999;
        for(var i=0; i<chang*kuan ; i++){
            divshuzu[i].className = "";
            if(jiancha_fugai(diandiv2,divshuzu[i])){
                var dx = dis(diandiv2,divshuzu[i]);
                
                if(imin > dx){
                    imin =  dx;
                    index_nei = i;
                }


            }
            
        }
        if(index_nei == -1){
            return null; //null 空值
        }
        else{
            return divshuzu[index_nei];
        }

    }

    var dianX,dianY,l,t;
    function shubiao(event){
        
        var diandiv2 = this;

        zindex++
        diandiv2.style.zIndex = zindex;

        //放置鼠标单击的坐标！
        dianX = event.clientX - diandiv2.offsetLeft ;
        dianY = event.clientY - diandiv2.offsetTop ;


        //鼠标点 下去的 坐标图坐标



        document.onmousemove = function (event) {
            l = event.clientX - dianX;
            t = event.clientY - dianY;

            //鼠标拖动图片进行移动！
            fugai = linjufugai(diandiv2);
            if(fugai){
                //如果是一个真实的对象 ！就是 真 不是 则假
                fugai.className = "active"; //border 2px solid red
            }

            diandiv2.style.left = l + "px";
            diandiv2.style.top = t + "px";
            
        }

        document.onmouseup = function(){
            //move 函数代表这图片移动回去
            
            if(fugai){
                fugai.className = "";
                move(diandiv2,yidong[fugai.index][0],yidong[fugai.index][1]);
                move(fugai,yidong[diandiv2.index][0],yidong[diandiv2.index][1]);

                //交换index 

                var temp = 0 ;
                temp = fugai.index;
                fugai.index = diandiv2.index;
                diandiv2.index = temp;


                for(var i = 0 ;  i<9 ; i++){
                    laoshuzu[i] = divshuzu[i].index+1;
                }


                if(pintujieshu()){

                    youxijieshu();

                }


            }
            else{
                move(diandiv2,yidong[diandiv2.index][0],yidong[diandiv2.index][1]);

            }


            //释放
            document.onmousemove = null;
            document.onmouseup = null;

        }

    }

            //拼图结束
            function pintujieshu(){
                
                for(var i=0;i<8;i++){
                    if(newshuzu[i]!=laoshuzu[i]){
                        return false;
                    }
                }

                return true;

            }
            //游戏结束
            function youxijieshu(){

                var pintujieshudiv = document.createElement("div");

                var chuangp = document.createElement("p");

                var t = 4;

                var sj ;
                var yincang = document.getElementById("yincangtu");
                yincang.style.display = "block";
                pintujieshudiv.style.cssText = "position:absolute;z-index:9999999;top:30%;left:22%;width:100%text-align:center;font-size:70px;color:red;";
                
                pintujieshudiv.innerHTML = "很好就用了"+oTxt.value+"就完成了拼图";
                /*alert("很好就用了"+oTxt.value+"就完成了拼图")*/

                pintujieshudiv.appendChild(chuangp);
                
                yinruDiv.appendChild(pintujieshudiv);


                function shijianxiaoshi(){
                    chuangp.innerHTML = t--;
                    if( t <= 0 ){
                        clearInterval(sj);
                        window.location.reload();
                        return;

                    }
                    sj = setTimeout(function(){
                        shijianxiaoshi();
                    },1000);

                }
                shijianxiaoshi();

            }


//var shijian;
function move(diandiv2,left,top){
    // l 和top 是原始数组
    clearInterval(diandiv2.shijian);
    diandiv2.shijian = setInterval(function(){
        var tingindex = false; //停止移动标志
        //移动到 新数组
        var ileft = parseInt(diandiv2.style.left);
        var itop = parseInt(diandiv2.style.top);

        if(left != ileft || top != itop){
            var isudu_left = (left-ileft)/5;
            var isudu_top = (top-itop)/5;
            //
            isudu_left = isudu_left >0 ? Math.ceil(isudu_left): Math.floor(isudu_left);
            isudu_top = isudu_top >0 ? Math.ceil(isudu_top): Math.floor(isudu_top);
            // ceil (数值) 正数？ 小数 部分进1 负数 舍去小数部分

            diandiv2.style.left =  (ileft + isudu_left)+ "PX";
            diandiv2.style.top =  (itop + isudu_top)+ "PX";
        }
        /*
        else if(top != itop){
            var isudu = (top-itop)/5;
            //
            isudu = isudu >0 ? Math.ceil(isudu): Math.floor(isudu);
            // ceil (数值) 正数？ 小数 部分进1 负数 舍去小数部分

            diandiv2.style.top =  (itop + isudu)+ "PX";
        }*/
        else{
            tingindex =  true;
        }
        if(tingindex){
            clearInterval (diandiv2.shijian);

        }
    },10);
}


var oTxt=document.getElementById("mytime");
var oStop=document.getElementById("zanatingshijian");
var oReset=document.getElementById("chongxinkaishi");

var n= 0;
var timer=null;


//暂停并且清空计时器
oStop.onclick= function () {
    clearInterval(timer);
}
//重置
oReset.onclick= function () {
   /* oTxt.value="00:00:00";
    n=0;*/
    location.reload();
}
//补零
function toDub(n){
    return n<10?"0"+n:""+n;
}
```

 

[资源下载](https://www.lanzous.com/i2m4wlg)