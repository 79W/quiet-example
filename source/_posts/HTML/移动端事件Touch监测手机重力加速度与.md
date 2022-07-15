---
title: 移动端事件Touch监测手机重力加速度与
categories: 技术笔记
tags:
  - 移动端
  - Touch
  - 函数节流
  - 函数防抖
excerpt: 移动端事件Touch监测手机重力加速度与
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201028171710.png'
articleLink: e1a227ca24383628
date: 2020-10-28 10:24:36
---

## 移动端事件

描述手指在触摸平面（触摸屏、触摸板等）的状态变化的事件

这类事件用于描述一个或多个触点，使开发者可以检测触点的移动，触点的增加和减少，等等

通俗的说就是你在手机屏幕上的手指的操作和数量

我们定义一个div然后我们对其进行操作

```
<div id="box"></div>
```

### touchstart

当用户在触摸平面上放置了一个触点时触发。事件的目标 `element` 将是触点位置上的那个目标 `element`

```
box.addEventListener("touchstart",function(){
        console.log("手指触碰");//手指在 box 上摁下
});
```

### touchmove

当一个触点被用户从触摸平面上移除（即用户的一个手指或手写笔离开触摸平面）时触发。当触点移出触摸平面的边界时也将触发。例如用户将手指划出屏幕边缘。

事件的目标`element` 与触发 `touchstart` 事件的目标`element` 相同，即使 `touchend` 事件触发时，触点已经移出了该`element` 。

已经被从触摸平面上移除的触点，可以在 `changedTouches` 属性定义的 `TouchList`中找到。

```
box.addEventListener("touchmove",function(){
        console.log("手指移动");// 手指在元素上摁下之后，在屏幕中移动
});
```

### touchend

当用户在触摸平面上移动触点时触发。事件的目标`element` 和触发 `touchstart` 事件的目标`element` 相同，即使当 `touchmove` 事件触发时，触点已经移出了该`element` 。

当触点的半径、旋转角度以及压力大小发生变化时，也将触发此事件。

```
box.addEventListener("touchend",function(){
        console.log("手指抬起",this);//手指在 box 上摁下,在屏幕中抬起
});
```

### touch和mouse的区别

1⃣️ `mousedown`只能在元素内进行移动才是有效的出了元素就没有用了 （不需要按下）

2⃣️ `touchmove` 当你接触到元素只要在屏幕内移动可以的 出了元素也是可以的（必须按下才能执行）

3⃣️ `touchend` 可以在元素外抬起 而`mouseup` 只能在当前元素上进行抬起

### 事件点透

当我们只点击了一次却下层的事件也执行了

```
<style>
  #box {
    position: absolute;
    left: 0;
    top: 0;
    opacity: .5;
    width: 260px;
    height: 260px;
    background: red;
    font: 28px/2 "宋体";
    color: #fff;
  }
</style>
```

```
<a href="https://www.baidu.com">百度</a>
<div id="box">DIV</div>
```

```
let box = document.querySelector("#box");
box.addEventListener("touchend", function (e) {
		// 点击抬起 进行隐藏
		box.style.display = "none";
});
```

![事件点透](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgdianjiaasf.gif)

点击时上层元素消失，触发到下边被遮挡元素的事件;

这就是 事件点透 但是是什么原因造成这样的后果呢？

*触碰元素之后，会立即执行 touch 事件,然后记录手指触碰的坐标点，延迟300ms（300ms 并不是精确值）, 在该坐标去找元素，查看该元素上是否有鼠标事件，有的话就执行。* 

a的href就是一个鼠标点击事件

**解决办法：**

🍁 给 touch 操作添加延迟

```
let box = document.querySelector("#box");
box.addEventListener("touchend", function (e) {
		// 添加延时
    setTimeout(()=>{
   		 box.style.display = "none";
    },300);
});
```

🍂 在移动端不在使用鼠标事件（包括a 的 href）

🍃 在 touch 事件中阻止默认事件

```
let box = document.querySelector("#box");
box.addEventListener("touchend", function (e) {
    box.style.display = "none";
    // 阻止默认行为
    e.preventDefault();
});
```

**但是阻止了默认 会有很多新的问题**

- 不允许双指缩放页面
- 阻止完默认事件之后，该元素及其子元素鼠标事件都会失效
- 输入框不能获得焦点，不能输入
- 长按不能启动系统菜单
- 禁止滚动条滚动

### touchEvent对象

#### touches

会显示屏幕上所有手指的个数

#### targetTouches

点击元素上的手指个数

#### changedTouches

涉及当前(引发)事件的触摸点的列表

```
let box = document.querySelector("#box");
    box.addEventListener("touchmove",function(e){
        this.innerHTML = `
            当前屏幕上的手指个数:${e.touches.length}<br/>
            当前元素上的手指个数:${e.targetTouches.length}<br/>
            触发当前事件的手指个数:${e.changedTouches.length}<br/>
        `;
    });
```

![touchEvent](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201028150108244.png)

### orientationchange

监听横竖屏切换

我们监听 `orientationchange` 事件

```
window.addEventListener("orientationchange",function(){
   // 我们当转换换了屏幕这里可以干一些事情
});  
```

### window.orientation

检测手机横竖屏

横屏分别为 90和-90 竖屏分别为初始的0和180

```
alert(window.orientation);
```

我们可以在上面进行 使用 比如我们监听手机是否反转屏幕了 

我们可以知道是 竖着的还是横着的

```
window.addEventListener("orientationchange",function(){
    alert(window.orientation);
});  
```

### devicemotion

监听手机加速度发生变化 里面有几个方法 

#### acceleration

手机加速度检测 经典的案例 是微信的摇一摇

```
window.addEventListener("devicemotion",(e)=>{
    let {acceleration} = e;
    let {x,y,z} = acceleration; //手机目前的移动速度 -- 加速度
    //console.log(x,y,z);
    box.innerHTML = `
        x:${x.toFixed(5)}<br/>
        y:${y.toFixed(5)}<br/>
        z:${z.toFixed(5)}<br/>
    `;
});
```

是一个包括三轴（x、y、z）加速度信息的对象，每个轴都有自己的属性

📱X：表示x轴（西到东）上的加速度

⌚️Y：表示y轴（南到北）上的加速度

💻Z：表示z轴（下到上）上的加速度

#### accelerationIncludingGravity

手机重力加速度检测 案例是体感游戏和飞机大战

```
window.addEventListener("devicemotion",(e)=>{
    let {accelerationIncludingGravity} = e;
    let {x,y,z} = accelerationIncludingGravity;
    /*检测手机的移动速度 和 现在每个方向受到的重力加速度*/
    box.innerHTML = `
        x:${x.toFixed(0)}<br/>
        y:${y.toFixed(0)}<br/>
        z:${z.toFixed(0)}<br/>
    `;
});
```

### 检测是否是IOS

```
function getIos() {
    var u = window.navigator.userAgent;
    return !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
}
```

## 拓展

[lodash](https://www.lodashjs.com/docs/latest) 

### 函数防抖

多次调用一个函数，最终只执行一次

```
// 调用时，立即执行这个函数
function debounce(fn,delay=200,isStart = false){
    if(typeof fn !== "function"){
        return console.error("请传入一个函数");
    }
    let timer = 0;
    let isFirst = true; // 第一次触发
    // 返回处理过防抖的新函数
    return function(...arg){
        let _this = this;
        if(isFirst&&isStart){
            fn.apply(_this,arg);
            isFirst = false;
        }
        clearTimeout(timer);
        timer = setTimeout(() => {
            (!isStart)&&(fn.apply(_this,arg));  
            isFirst = true; 
        }, delay);
    }
}
```

### 函数节流

多次执行一个函数，但是我们希望能把函数的执行限定在一个可以接受的频率内

```
function throttle(fn,delay=200,start = true){
    if(typeof fn !== "function"){
        return console.error("请传入一个函数");
    }
    let timer = 0;
    // 返回处理过防抖的新函数
    return function(...arg){
        let _this = this;
        if(timer){
            return ;
        }
        start&&fn.apply(_this,arg); 
        timer = setTimeout(() => {
            (!start)&&fn.apply(_this,arg); 
            timer = 0;
        }, delay);
    }
}
```





