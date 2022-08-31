---
title: QiaoBug用Css画个小虫子并用动画动起来
categories: 技术笔记
tags:
  - css
  - bug
  - 动画
excerpt: QiaoBug用Css画个小虫子并用动画动起来
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5b5ad93fe5e8f.png'
articleLink: 3ee9026f15525725
date: 2020-09-25 21:47:57
---

### 场景

总想买个域名但是又不知道买什么域名

突然有个灵感 用我的姓氏Qiao然后我是搞前端开发的 最怕的就是Bug

那么就是Qiao最怕Bug

最后就形成了 [QiaoBug.com](https://www.QiaoBug.com/)

既然域名都买了那就将我长期使用[Github](https://github.com/QiaoBug)和[Gitee](https://gitee.com/QiaoBug)一并改了吧！

### Bug起源

然后我就查看[维基百科](https://zh.wikipedia.org/wiki/bug)关于Bug的相关介绍

在1947年9月9日，葛丽丝·霍普（Grace Hopper）发现了第一个电脑上的bug。当在 Mark II 计算机上工作时，整个团队都搞不清楚为什么电脑不能正常运作了。经过大家的深度挖掘，发现原来是一只飞蛾意外飞入了一台电脑内部而引起的故障（如图所示）。这个团队把错误解除了，并在日志本中记录下了这一事件。也因此，人们逐渐开始用“Bug”（原意为“虫子”）来称呼计算机中的隐错。现在在华盛顿的美国国家历史博物馆中还可以看到这个遗稿。

总结：bug就是一个虫子飞进电脑内部导致短路

### 需求

得是一只虫子 得会飞

### 开始

然用css画个虫子吧 

那么这个虫子应该找什么虫子的 维基百科说是飞蛾（感觉。。。）然后我试着在Google图片搜索`bug`那么出现了很多 七星瓢虫 我靠这个简单呀！

那么好我就将虫子定义为 七星瓢虫

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200925220915.png)

这是我自己抽象出来的样子那么好 既然我有这么好的画工就按这个画吧 

#### 分解

头那么不就是个椭圆吗

```
width: 30px;
height: 20px;
border-radius: 50%;
```

那么长和宽一样的话在加上`border-radius`为百分之五十的时候是个圆

那么椭圆就把 长度弄小点小于宽度就可以了 

身子也是个椭圆只是 这次宽度小于长度就ok 了 在去调整细节即可

画两个 触角 就是两个长方形

但是这里面用到 `transform`的`rotate`

翅膀也是用到了这个 

#### transform-rotate

定义了一种将元素围绕一个定点旋转而不变形的转换

而这个定点是由`transform-origin`属性指定

指定 `rotate()` 的旋转程度。

参数为正时，顺时针旋转；

参数为负时，逆时针旋转。

180° 旋转称为*点反演*。

```
<div>Normal</div>
<div class="rotated">Rotated</div>
```

```
div {
  width: 80px;
  height: 80px;
  background-color: skyblue;
}

.rotated {
  transform: rotate(45deg); /* Equal to rotateZ(45deg) */
  background-color: pink;
}
```

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200925221745.png)

#### transform-origin

让你更改一个元素变形的原点。

可以使用一个，两个或三个值来指定，其中每个值都表示一个偏移量。 没有明确定义的偏移将重置为其对应的初始值。

总结说就是换个原点 围绕着这个点进行旋转

#### 虫子画完

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200925222305.png)

有人会问了这和上面的手绘版是一个虫子吗？

我很严肃的告诉你 没错这就是同一个

### 翅膀动起来

虫子的样子画完了那么我就需要让这个翅膀 扑翅起来吧 营造一种飞的感觉

我用的是 css的`animation`动画

我先定义一个动画

```
animation-name: left-move;
```

这是什么意思呢 就是告诉浏览器 我要弄个动画 这个动画名字叫 吧啦吧啦...

我在告诉你这个动画执行的周期时间是多少你得在这时间内给我执行完

```
animation-duration: 10s;
```

不能扑哧一次就不飞了吧

那么你给我一直扑哧

```
animation-iteration-count: infinite;
```

#### 执行动画

下面的方式来定义上面的动画 名字要相对应

```
 @keyframes left-move {}
```

这里面可以 定义为百分比 什么意思呢？就是 你不是 10秒吗 那么可以分为 0% 25% 50% 75% 100%分为着5块那么就是 `10/5=2s` 每块待2秒

```
 @keyframes left-move {
   0%{}
   25%{}
   50%{}
   75%{}
   100%{}
 }
```

0%的时候干啥呢？你想干啥都行然后就在 0%那个大括号里面写上你想干的事

而我就偷懒了 这个还有个方法就是不用百分比的方式分割 而是直接定义开始和结束

```
@keyframes left-move {
    from {}
    to {}
}
```

效果其实和你只定义0%-100%是一样的

那么开始翅膀是什么样子 

结束的时候是什么样子

这时候就需要再次使用 `transform-origin` 和 `transform: rotate` 进行重新定义 原点和旋转的角度了

在加上动画时间执行的较快就形成了飞的感觉！！！

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgfeile.gif)

### 移动

现在只是在原地扑哧翅膀 这是我们需要 我们鼠标点哪儿 你就飞哪里去

下面是JavaScript了这里我用了jQuery

**讲讲思路**

首先获取鼠标点击位置的坐标

那么在进行移动到这个坐标即可

```js
$('html').click(function (e) {
            var xx = e.originalEvent.x || e.originalEvent.layerX || 0;
            var yy = e.originalEvent.y || e.originalEvent.layerY || 0;
            if ($(window).width() > "700") {
                $(".bug").animate({ left: xx, top: yy });
                $(".bug").attr({ 'transform': 'rotate(110deg)' })
            }
        })
```

这里又来知识点了哈

#### animate() 

animate() 方法执行 CSS 属性集的自定义动画。

该方法通过CSS样式将元素从一个状态改变为另一个状态。CSS属性值是逐渐改变的，这样就可以创建动画效果。

只有数字值可创建动画（比如 "margin:30px"）。字符串值无法创建动画（比如 "background-color:red"）。

那么我哪里用的是绝对定位 那么只需要更改 left top的值就可以了

这时候还是有些生硬 这个虫子 不转头呀 ！！！

#### Math.Atan2

可返回从 x 轴到点 (x,y) 之间的角度。

-PI 到 PI 之间的值，是从 X 轴正向逆时针旋转到点 (x,y) 时经过的角度。

计算到这个角度后 然后用js给这个虫子整体加上 `transform-rotate`的属性参数就是计算出来的角度

