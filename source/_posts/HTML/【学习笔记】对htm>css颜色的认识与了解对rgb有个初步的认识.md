---
title: 【学习笔记】对htm>css颜色的认识与了解对rgb有个初步的认识
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - 基础知识
  - 学习笔记
  - 颜色
excerpt: 对htm>css颜色的认识与了解对rgb有个初步的认识
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921220447.png'
articleLink: 4589175460a30905
date: 2020-01-05 15:26:09
---

## 颜色的概念

1. 色调：也就是大家常说的颜色（大家理解为颜色就可以了）
2. 饱和度：灰色的含量（饱和度最大时，颜色中灰色的含量为零；饱和度最小时，颜色基本就是灰色。也就是说，饱和度与灰色的占比是成反比的。）
3. 亮度：黑色的含量（亮度最大时，颜色中黑色的含量为零。亮度最小时，颜色会变得非常暗。也就是说，亮度与黑色的占比是成反比的。）
4. 对比度：前景色与背景色的对比
5. web安全色：

**提示**：关于 Web 安全色的具体颜色以及值，可以参考 https://www.bootcss.com/p/websafecolors/。

### 颜色值的类型

- 色彩关键字（也就是颜色的单词如：**red**是红色 **transparent**是完全透明的）
- RGB 色彩模式
- HSL 色彩模式

#### rgb模式

**RGB**是一个简称，全称为 **Red-Green-Blue**，即**红-绿-蓝**。

十六进制符号 `#RRGGBB` 和 `#RGB`

- `#RRGGBB`：是 `#` 符号后面编写 6 个十六进制字符（0-9，A-F）
- `#RGB`：是 `#` 符号后面编写 3 个十六进制字符（0-9，A_F）

函数符 `rgb(R, G, B)`

- 这里的 R、G、B 的值可以使用 0 ~ 255 之间的值
- 这里的 R、G、B 的值也可以使用 0% ~ 100% 之间的值

```css
#p1 {
	background-color: #FFCC33;
}
#p2 {
	background-color: #FC3;
}
#p3 {
	background-color: rgb(255, 204, 51);
}
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaIXY4Jn6ryjePZc-20200921220223111.png)

#### hsl模式

**HSL**是一个简称，全称为 **Hue-Saturation-Lightness**，即 **色调-饱和度-亮度**。HSL 色彩模式是一种将 RGB 色彩模型中的点在圆柱坐标系中的表示法。

- **H **表示色调，其值范围为 0 ~ 360 之间的一个角度。
- **S** 表示饱和度，其值范围为 0% ~ 100% 之间的百分值。
- **L** 表示亮度，其值使用百分值表示。0%表示黑色，50%表示标准色，100%表示白色。

```
p {
	background-color: hsl(50, 33%, 25%);
}
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img2ej14DGftlK3SRy-20200921220252290.png)

### 透明度

在 CSS3 版本中新增了 opacity 属性用来设置 HTML 元素的透明度，该属性的值范围介于 0 ~ 1 之间。具体情况如下所示：

- 如果值为 0 或 0.0 则表示完全透明
- 如果值为 0.5 则表示半透明
- 如果值为 1 或 1.0 则表示不透明

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>透明度</title>
  <style>
    div {
      background-color: lightcoral;
    }

    .light {
      opacity: 0.2;
    }

    .medium {
      opacity: 0.5;
    }

    .heavy {
      opacity: 0.9;
    }
  </style>
</head>

<body>
  <div class="light">这是一个测试内容.</div>
  <div class="medium">这又是一个测试内容.</div>
  <div class="heavy">这还是一个测试内容.</div>
</body>

</html>
```

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imglcHoD5WBqJsm1T7.png)

- RGB 模式增加了 `rgba(R,G,B,A)` 函数，其中 A 为 alpha 表示透明度。
- HSL 模式增加了 `hsl(H,S,L,A)` 函数，其中 A 为 alpha 表示透明度。

## 单位

- 绝对长度单位
- 相对长度单位

### 绝对长度单位

这种的长度是固定、不变化的。（px）

| 单位 | 名称         | 等价换算            |
| :--- | :----------- | :------------------ |
| `cm` | 厘米         | 1cm = 96px/2.54     |
| `mm` | 毫米         | 1mm = 1/10th of 1cm |
| `Q`  | 四分之一毫米 | 1Q = 1/40th of 1cm  |
| `in` | 英寸         | 1in = 2.54cm = 96px |
| `pc` | 十二点活字   | 1pc = 1/16th of 1in |
| `pt` | 点           | 1pt = 1/72th of 1in |
| `px` | 像素         | 1px = 1/96th of 1in |

### 相对长度单位

**相对长度单位**所描述的长度一般会具有一个明确的参考物，例如父级元素、根元素或视口大小等。使用**相对长度单位**相对绝对长度单位更适用于现在越发复杂的终端设备的屏幕输出。

| 单位   | 相对于            |
| :----- | :---------------- |
| `em`   | 父元素的字体大小  |
| `ex`   | 字符“x”的高度     |
| `ch`   | 数字“0”的宽度     |
| `rem`  | 根元素的字体大小  |
| `lh`   | 元素的line-height |
| `vw`   | 视窗宽度的1%      |
| `vh`   | 视窗高度的1%      |
| `vmin` | 视窗较小尺寸的1%  |
| `vmax` | 视图大尺寸的1%    |

