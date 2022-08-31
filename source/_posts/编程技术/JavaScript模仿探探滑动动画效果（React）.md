---
title: JavaScript模仿探探滑动动画效果（React）
categories: 技术笔记
tags:
  - 探探
  - React
excerpt: 使用JavaScript,React,Vue模仿探探动画
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/cool.cn_community_01e0b25d6721bca801211f9e194369.png@2o.png&refer=http___img.zcool.png'
articleLink: 72d23e8e
date: 2021-06-10 17:27:20
---

模仿一下探探的动画（右滑喜欢左滑不喜欢）这里只做基础动画 可以自己进行进一步的功能增加

我这里使用的是React进行编写的总体思路不变

-------------------------

我们首先思考所需要什么

1. 跟随手指滑动（onTouchMove,onTouchStart,onTouchEnd）
2. 松手自动离开或者复原（动画 transform ）

![](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/1540713-cb62427e7fde2680.gif)

### 移动onTouch

我们只需要实时进行改变它的位置即可 

我们需要三个监听事件分别为 手指按下 手指移动 手指移开

```
<View className="div1">
      <View className="div2"
          onTouchEnd={(e)=>{
          	// 手指移开
            touchEnd(e)
          }}
          onTouchMove={(e)=>{
          	// 手指移动
            touchmove(e)
          }}
          onTouchStart={(e)=>{
          	// 手指按下
            touchStart(e)
          }}
      ></View>
</View>
```

#### 手指按下onTouchStart

手指按下那一刻我们需要做的事情比较少我们只需要记录当前位置即可

```
let touchStart = (event)=>{
    let {changedTouches} = event
    let {clientX,clientY} = changedTouches[0]
    setStartx(clientX)
    setStarty(clientY)
    setEndx(clientX)
    setEndy(clientY)
}
```

我们记录下当前的 X，Y的坐标后就是开始移动了这里我们需要做的东西较多

#### 手指移动onTouchMove

我们需要实时更新手指移动的坐标并且更新到div元素上面

```
let [poswidth,setPoswidth] = useState(0)
let [posheight,setPosheight] = useState(0)
let touchmove = (event)=>{
    let {changedTouches} = event
    let {clientX,clientY} = changedTouches[0]
    setEndx(clientX)
    setEndy(clientY)
    setPoswidth(endx-startx)
    setPosheight(endy-starty)
 }
```

我们记录了手指移动的坐标并且实时的更新变量了 但是你会发现我们并没有把参数更新到div元素上面

我们编写一个方法实时计算css transform 并赋值到div元素上面

```
let transformIndex =  (color) => {
    // 处理3D效果
    let style = {};
    style['transform'] = 'translate3D(' + poswidth + 'px' + ',' + posheight + 'px' + ',0px)'+ 'rotate(' + dxangle + 'deg)';
    style['opacity'] = 1;
    style['zIndex'] = 10;
    style['box-shadow'] = '0px 1px 20px 0px rgba(0,0,0,0.1)';
    style['background'] = color;
    // 这里是 是否启动动画效果
    if(animation){
      style['transitionTimingFunction'] = 'ease';
      style['transitionDuration'] = 400 + 'ms';
    }
    console.log('style1', style);
    return style;
  }
```

然后我们div就应该变成

```
<View className="div1">
      <View className="div2"
          onTouchEnd={(e)=>{
            touchEnd(e)
          }}
          onTouchMove={(e)=>{
            touchmove(e)
          }}
          onTouchStart={(e)=>{
            touchStart(e)
          }}
          style={transformIndex()}
      ></View>
</View>
```

然后就可以跟随手指进行变动了

最后一个就是当我们手指松开的时候

#### 手指移开onTouchEnd

我们可以在这里做很多操作 当他松开的的时候我们是将div元素动画移动走还是回到原来的位置

这里你可以有自己的想法

```
let touchEnd = (event)=>{
    if (Math.abs(poswidth) >= 79) {
      let ratio = Math.abs(posheight / poswidth);
      setPoswidth(poswidth >= 0 ? poswidth + 500 : poswidth - 500)
      setPosheight(posheight >= 0 ? Math.abs(poswidth * ratio) : -Math.abs(poswidth * ratio))
    }else{
      setPoswidth(0)
      setPosheight(0)
    }
  }
```

### 总结

这里只是一个基础的结构 基本的思路

但是万变不离其宗啊～ 

在探探中其实他还有个角度这个需要大家发挥思路自行解决一下

思考：

1. 为什么手指点上的时候div 没有让左上角移动到我手指的地方
2. animation 启动动画哪里是做什么用的自行完善下
3. 如何让多个叠加起来？

