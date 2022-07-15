---
title: Flex基础语法学习与使用详细分析
categories: 技术笔记
tags:
  - Flex
  - 弹性布局
excerpt: Flex基础语法学习与使用详细分析
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922203755.png'
articleLink: dbb1eb1450873810
date: 2020-09-10 10:49:38
---

## flex布局

1. 目前web开发使用最多的布局方案
2. flex布局（flexible布局，弹性布局）
3. 目前移动端用的最多 pc端也越来越多了

### 重要概念

1. 开启flex布局的叫做 flex-container
2. 内部的元素叫做 flex-items

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imge41ed1e2538c2-20200922203200807.png)

### 开启flex布局

外层设置 display

```html
<div class="box">
		<div></div>
		<div></div>
		<div></div>
</div>
```

```css
/*开启flex布局*/
.box{
	display:flex;
	/*flex 块元素 inline-flex为行内元素*/
}
```

没有开启flex布局

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>flex</title>
    <style>

        .box{
            width: 600px;
            height: 600px;
            background: forestgreen;
        }
        .item1{
            width: 100px;
            height: 100px;
            background: rgb(50, 194, 238);
        }
        .item2{
            width: 100px;
            height: 100px;
            background: fuchsia;
        }
        .item3{
            width: 100px;
            height: 100px;
            background: rgb(194, 62, 53);
        }

    </style>
</head>
<body>
    <div class="box">
        <div class="item1"></div>
        <div class="item2"></div>
        <div class="item3"></div>
    </div>
</body>
</html>
```

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img017f068aeed1c.png)

没有开启flex布局的时候 div都是自己独占一行的块元素

开启flex布局

```
.box{
    width: 600px;
    height: 600px;
    background: forestgreen;
    /* 加上 flex */
    display: flex;
}
```

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5d443fc89244f.png)

开启flex就不区分 块元素还是什么元素都是flex说了算

## flex布局模型

**重要**

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgedfeaef26172b.png)

1. main axis 为主轴
   1. main size 主轴大小
   2. main start 为开始的地方
   3. main end 为结束的地方
2. vross axis 交叉轴
   1. vross size 主轴大小
   2. vross start 为开始的地方
   3. vross end 为结束的地方

## flex相关属性

### 应用在 flex-container 上的css属性

1. flex-flow
2. flex-direction 
3. flex-wrap
4. justify-content
5. align-items
6. align-content

#### flex-direction 规定灵活项目的方向

1. flex-items **默认**都是沿着main axis（主轴）从 main start 开始忘 main end 方向排布

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img21ab7c6e97e67.png)

2. direction决定了 main axis的方向

   1. row（默认）

      1. 绝对主轴的位置
      2. 从左往右

   2. row-reverse

      与row相反 是从右往左

      ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img21ab7c6e97e67.png)

   3. column

      1. 主轴从上倒下

         ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img2a6026dbd0f5e.png)

   4. column-reverse

      1. 从下到上

      ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imge5fb7d7258cca.png)

#### justify-content 决定 items在 main 上的对齐方式

1. flex-start（默认）与main start对齐

   ```
   justify-content:flex-start;
   ```

2. flex-end 与 main end对齐

3. center 局中对齐

4. space-between

   1. flex items 之间距离相等
   2. main start ，main end 两端对齐
   3. ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgcc70f4db8113b.png)

5. space-evenly

   1. flex items 之间距离相等
   2. flex items 与 main start ，main end 之间的距离等于 items之间的距离
   3. ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgc1c307cd4ee3a.png)

6. space-around

   1. flex items 之间距离相等
   2. flex items 与 main start ，main end 之间的距离是items之间的距离一半
   3. ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img31d9dc8a0b352.png)

#### align-items 在cross axis的对齐

1. normal:在弹性布局中,效果和 stretch一样

2. stretch:当 flex items在cross axis方向的size为auto时,会自动拉伸至填充 flex container

   1. 在没有给 高度的时候就会拉伸长度

   <img src="https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img61f787d45f2ab.png" alt="image.png" style="zoom:50%;" />

3. flex-start:与 cross start对齐 （顶部）

4. flex-end:与 cross end对齐 （底部）

5. center居中对齐

6. baseline:与基准线对齐

   1. 如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效
   2. 是与第一个最后一行的为标准
   3. ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img33e323d5db81d-20200922203448368.png)

#### flex-wrap 换行显示

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img7117f5406c8c2.png)

1. nowrap (默认) 单行显示

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img498842f330161-20200922203521280.png)

2. wrap 多行显示

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img02390e5fe2ed7-20200922203543336.png)

3. wrap-reverse ：多行

   对比wrap cross-start 与 cross-end 相反

   在交叉轴上进行一个反转

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img59f651650a596.png)

#### flex-flow 

flex-flow 属性是 flex-direction 和 flex-wrap 属性的复合属性。

用于设置或检索弹性盒模型对象的子元素排列方式。

flex-direction 属性规定灵活项目的方向。

flex-wrap 属性规定灵活项目是否拆行或拆列。

简单说就是同时设置 并减少了代码

#### align-content 决定多行flex-items 在交叉轴上的对齐方式

align-content-决定了多行 flex items在 cross axis上的对齐方式,用法与 justify-content-类似

1. stretch(默认值):与 align-items-的 stretch类似

2. flex- start:与 cross start对齐

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img67b99097d6713.png)

3. flex-end:与 cross end对齐

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img89aa6be57ef78.png)

4. center:居中对齐

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgc52ba84c5006d.png)

5. space-between:

   1. flex items之间的距离相等
   2. 与 cross start、 cross end两端对齐

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgae99987789677.png)

6. space-around:

   1. flex items之间的距离相等
   2.  flex items与 cross start、 cross end之间的距离是 flex items之间距离的一半

   ![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img7ec1b085b5a8e.png)

7. space-evenly

   1. flex items之间的距离相等
   2. flex items与 cross start、 cross end之间的距离等于 flex items之间的距离

### 应用在 flex-items上的css属性

1. flex
2. flex-grow
3. flex-basis
4. flex-shrink
5. order
6. align-self

#### order 排序

觉得items 排列的顺序

1. 可以设置任何整数 0-> 值越小越排在前面
2. 默认为0

```
<div class="box">
        <div class="item1"></div>
        <div class="item2"></div>
        <div class="item3"></div>
    </div>
```

```
.box{
            width: 600px;
            height: 600px;
            background: forestgreen;
            display: flex;

        }
```

```
.item1{
            width: 100px;
            height: 100px;
            background: rgb(50, 194, 238);
            order: 1;
        }
        .item2{
            width: 100px;
            height: 100px;
            background: fuchsia;
            order: 99;
        }
        .item3{
            width: 100px;
            height: 100px;
            background: rgb(194, 62, 53);
            order: 2;
        }
```

![image.png](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgd26f5b9f84685.png)

#### align-self

items可以通过align-self 覆盖flex container 设置的align-items

1. auto 默认值
2. stretch flex-start flex-end center baseline 效果跟align-items一样的

#### flex-grow 分份 增加

主轴分出来然后在分给每个

##### flex items 如何拓展

1. 可以设置任何非负数 （正小树 正整数 0） 默认值是0
2. 当 flex-container 在main axis 方向上有剩余的时候才会生效

##### 如果flex items 的flex-grow总和sum超过1 每个flexitem 拓展的size为

flex contiainer 剩余size * flex-grow/sum

##### 如果flex items 的flex-grow总和sum不超过1 每个flexitem 拓展的size为

flex container 剩余 size * flex-grow

#### flex-shrink 减少

flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。

1. flex -shrink-决定了 flex items如何收缩
   1. 可以设置任意非负数字(正小数、正整数、0),默认值是1
   2. 当 flex items在 main axis方向上超过了 flex container的size,flex -shrink-属性才会有效
2. 如果所有 flex items的flex- shrink总和超过1,每个 flex item收缩的size为
   1.  flex items超出 flex container的size*收缩比例/所有 flex items的收缩比例之和
3. *如果所有 flex items的flex- -shrink总和sum不超过1,每个 flex item收缩的size为
   1.  flex items超出 flex container的size*sum*收缩比例/所有 flex items的收缩比例之和
   2. 收缩比例=flex -shrink-* flex item的 base size
   3. base size就是 flex item放入 flex container之前的size
4. flex items收缩后的最终size不能小于min -width-\min-height

#### flex-basis 用于设置或检索弹性盒伸缩基准值

设置默认大小

优先级：

	1. max-width \ max-height \ min-width \ min-height
 	2. Flex-basis
 	3. Width\height
 	4. 内容本身的size

#### flex

flex是flex -grow- flex-shrink- flex--basi的简写flex属性可以指定1个,2个或3个值。

##### 单值语法:值必须为以下其中之一

1. 个无单位数():它会被当作的值。
2. 一个有效的宽度(width)值:它会被当作的值
3. 关键字none,auto或 initial.

##### 双值语法:第一个值必须为一个无单位数,并且它会被当作的值。

1. 第二个值必须为以下之一:
2. 一个无单位数:它会被当作的值。
3. 一个有效的宽度值:它会被当作的值。

##### 三值语法:

1. 第一个值必须为一个无单位数,并且它会被当作的值
2. 第二个值必须为一个无单位数,并且它会被当作的值。
3. 第三个值必须为一个有效的宽度值,并且它会被当作的值。