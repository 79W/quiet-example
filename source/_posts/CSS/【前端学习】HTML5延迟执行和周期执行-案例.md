---
title: '【前端学习】HTML5延迟执行和周期执行,案例'
categories: 技术笔记
tags:
  - HTML
  - 个人笔记
  - time
  - 周期操作
  - 延迟操作
excerpt: 定时器是什么 延迟执行：指的是指定程序代码在指定时间后被执行，而不是立即执行。 周期执行：指的是指定程序代码在指定时间为间隔，重复被执行。
cover: >-
  https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgc4c9019ee634867da087d76ba1583efd.gif
articleLink: 415795f18bd13814
date: 2020-05-14 08:07:38
---

## 定时器是什么

1. 延迟执行：指的是指定程序代码在指定时间后被执行，而不是立即执行。
2. 周期执行：指的是指定程序代码在指定时间为间隔，重复被执行。

### 延迟执行

　setTiomeout()方法设置一个定时器 在定时器定时后执行一个函数或一段指定的代码 。

```
var timeoutID = scope.setTimeout(function/code[,delay])
```

| 参数  | 描述                                             |
| ----- | ------------------------------------------------ |
| code  | 必需。要调用的函数后要执行的 JavaScript 代码串。 |
| delay | 必需。在执行代码前需等待的毫秒数。               |

 

### 周期执行

setInterval()方法可按照指定的周期（以毫秒计）来调用函数或计算表达式。

setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。

由 setInterval() 返回的 ID 值可用作   clearInterval() 方法的参数。

```
setInterval(code/milisec[,'lang'])
```

| 参数     | 描述                                                   |
| -------- | ------------------------------------------------------ |
| code     | 必需。要调用的函数或要执行的代码串。                   |
| millisec | 必须。周期性执行或调用 code 之间的时间间隔，以毫秒计。 |

#### 返回值

一个可以传递给 Window.clearInterval() 从而取消对 code 的周期性执行的值

### 动画

requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行

```
window.requestAnimationFrame(callback);
```

#### 参数

- `callback`

  下一次重绘之前更新动画帧所调用的函数(即上面所说的回调函数)。该回调函数会被传入`DOMHighResTimeStamp`参数，该参数与`performance.now()`的返回值相同，它表示`requestAnimationFrame()` 开始去执行回调函数的时刻。

#### 返回值

一个 `long` 整数，请求 ID ，是回调列表中唯一的标识。是个非零值，没别的意义。你可以传这个值给 `window.cancelAnimationFrame()` 以取消回调函数。

## **案例：开始与结束**

 

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgc4c9019ee634867da087d76ba1583efd.gif)

```
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Document</title>

</head>

<body>

    <button id="start">开始时间</button>

    <button id="stop">停止时间</button>

    <h2 id="shouwtime"></h2>

    <script>

        // 全局 timeID

        var t;

        // 获取按钮

        var start = document.getElementById('start')

        var stop2 = document.getElementById('stop')

        // 开始按钮的执行事件

        start.addEventListener('click',stime);

        // 结束按钮事件

        stop2.addEventListener('click',function(){

            // 清除延迟事件

            clearTimeout(t);

            // 恢复按钮

            start.removeAttribute('disabled')

        });

        // 开始显示的事件 获取时间

        function stime(){

            // 禁用按钮

            start.setAttribute('disabled','disabled')

            // 获取时间

            var date = new Date();

            var hour = date.getHours();

            var minute = date.getMinutes();

            var second = date.getSeconds();

            // 格式化时间

            var time = hour + ":" + minute + ":" +second;

            // 显示时间

            var showtime = document.getElementById("shouwtime");

            showtime.textContent = time;

            // 设置延迟事件

            t = setTimeout(stime,1000);

        }

    </script>

</body>

</html>
```

## **案例：小方块移动**

 

![img](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img5f1caa64c4dbb33a31f0ad26ad3635f2.gif)

```
<!DOCTYPE html>

<html lang="en">

<head>

	<meta charset="UTF-8">

	<title>方块移动</title>

	<style>

		body {

			margin: 0;

		}

		#box {

			width: 50px;

			height: 50px;

			background-color: coral;

			position: absolute;

			top: 200px;

			left: 100px;

		}

	</style>

</head>

<body>

	<div id="box"></div>

	<script>

		var box = document.getElementById('box')

		var t

		//开关,false表示没有被点击过,应开始移动.

		var flag = false

		box.addEventListener('click', function () {

			if (!flag) {

				t = setInterval(function () {

					//1. 获取当前方块的left

					var style = window.getComputedStyle(box)

					var left = parseFloat(style.left)

					//2. 增加left样式属性值

					left++

					//3. 利用内联样式覆盖外联样式

					box.style.left = left + 'px'

				}, 10)

				flag = true

			} else {

				clearInterval(t)

				flag = false

			}

		})

	</script>

</body>

</html>
```

