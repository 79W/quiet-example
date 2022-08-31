---
title: JavaScript面向对象思想实现贪吃蛇小游戏
categories: 技术笔记
tags:
  - 贪吃蛇
  - 面向对象
  - JavaScript
excerpt: JavaScript面向对象思想实现贪吃蛇小游戏
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200922212404.png'
articleLink: f63c4e227c502921
date: 2020-09-21 15:43:29
---

## 贪吃蛇游戏

![](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20200921205254.png)

可以将这个小游戏分为几个模块：

### Map地图类

#### clearData 清空数据

如果蛇吃到食物了那么这个食物就要消失并且重新生成一个

还有就是蛇进行移动的时候也是 清除数据在进行生成

#### setData 设置数据  

就是将食物和蛇的位置等数据进行设置成功

#### include 是否包含这个数据

也就是说 生成的食物不能是在蛇的内部的 

也就是说蛇所占的位置将不在生成这个食物 

```
include(data) {
   return this.data.some(item => (item.x == data.x && item.y == data.y));
}
```

#### render 渲染数据

将数据放在html 里面进行显示

```
            render() {
                this.el.innerHTML = this.data.map(item => {
                    return `
                <span style="width:${this.rect}px;height:${this.rect}px;position:absolute;left:${this.rect * item.x}px;top:${this.rect * item.y}px;background:${item.color};"></span>
            `;
                }).join("");
            }
```

### foot食物类

#### create 创建食物

进行随机生成x和y 的数值然后经过 include 方法进行判断如果包含那么就重新生成一个 如果不包含那么就渲染出来

```
create() {
                let data = {};
                data.x = Math.floor(Math.random() * this.map.cells);
                data.y = Math.floor(Math.random() * this.map.rows);
                if (this.map.include(data)) {
                    this.create();
                } else {
                    data.color = this.color[Math.floor(Math.random() * this.color.length)];
                    this.map.setData(data);
                    this.data = data;
                }
            }
```

### Snake蛇类

#### move 移动蛇

判断移动的方向然后 进行加以

第一次蛇的位置为：

```
{ x: 6, y: 2, color: "blue" },
{ x: 5, y: 2, color: "white" },
{ x: 4, y: 2, color: "white" },
{ x: 3, y: 2, color: "white" },
{ x: 2, y: 2, color: "white" }
```

那么向右移动一个就会变成

```
{ x: 7, y: 2, color: "blue" },
{ x: 6, y: 2, color: "white" },
{ x: 5, y: 2, color: "white" },
{ x: 4, y: 2, color: "white" },
{ x: 3, y: 2, color: "white" }
```

也就是说 后面一个等于前面那个的位置

```
move() {
    var i = this.data.length - 1;
    this.LastData = {
        x: this.data[i].x,
        y: this.data[i].y,
        color: this.data[i].color
    }
    for (; i > 0; i--) {
        this.data[i].x = this.data[i - 1].x;
        this.data[i].y = this.data[i - 1].y;
    }
    switch (this.direction) {
        case "left": this.data[0].x -= 1; break;
        case "right": this.data[0].x += 1; break;
        case "top": this.data[0].y -= 1; break;
        case "bottom": this.data[0].y += 1; break;
    }
}
```

#### eatfoot 吃食物

判断蛇头是不是和这个食物的位置相同 如果相同就表示吃到了 那么就给蛇后面加个小方块

```
eatfoot(foot) {
    if (foot.data.x == this.data[0].x
    && foot.data.y == this.data[0].y) {
        this.data.push(this.LastData);
        return true;
    }
		return false;
}

```

### Game游戏类

#### move 移动控制

判断这个是否

```
move() {
    this.stop();
    this.timer = setInterval(() => {
        this.shake.move();
        this.map.clearData();
        if (this.shake.eatfoot(this.foot)) {
            this.foot.create();
            this.grade++;
            this.gradeChange && this.gradeChange();
            if (this.interval > 10) {
                this.stop();
                this.interval -= 5;
                this.move();
            }
        }
        this.map.setData(this.foot.data);
        this.map.setData(this.shake.data);
        if (this.isOver()) {
            this.stop();
            alert("game over");
            return;
        }
        this.map.render();
    }, this.interval)
}
```



#### isEat 是否吃到东西

#### isOver 是否结束

检测这个蛇是否到了边上了 或者是这个蛇头是否撞到了自己的身体那么就返回true 然后

在move里面进行判断 最后调用结束方法

```
isOver() {
    // 检测蛇和自己相撞
    for (let i = 1; i < this.shake.data.length; i++) {
        if (this.shake.data[i].x == this.shake.data[0].x
        && this.shake.data[i].y == this.shake.data[0].y) {
        		return true;
        }
    }
    // 检测蛇超出
    if (this.shake.data[0].x < 0
        || this.shake.data[0].x >= this.map.cells
        || this.shake.data[0].y < 0
        || this.shake.data[0].y >= this.map.rows) {
    		return true;
    }
    return false;
}
```

#### over 结束

#### control 控制器

就是控制蛇上下左右的方向的

第一种：

```
control() {
  window.addEventListener("keydown", ({ keyCode }) => {
    if (this.shake.direction == "top" || this.shake.direction == "bottom") {
    this.shake.direction = keyCode == 37 ? "left" : (keyCode == 39 ? "right" : this.shake.direction);
    } else if (this.shake.direction == "left" || this.shake.direction == "right") {
    this.shake.direction = keyCode == 38 ? "top" : (keyCode == 40 ? "bottom" : this.shake.direction);
    }
  });
}
```

第二种：

```
keyDown({ keyCode }) {
    console.log(keyCode)
    switch (keyCode) {
        case 37:
            this.snake.changeDir("left")
            break
        case 38:
            this.snake.changeDir("top")
            break
        case 39:
            this.snake.changeDir("right")
            break
        case 40:
            this.snake.changeDir("bottom")
            break
    }
}
 //控制器
 control() {
 		window.addEventListener("keydown", this.keyDown)
 }
```

## 完整代码一

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        #map {
            position: relative;
            width: 400px;
            height: 400px;
            background: #000;
        }
    </style>
</head>
<body>
    <div id="map"></div>
    <script>
        class Map {
            constructor(el, rect) {
                this.el = el;
                this.data = [];
                this.rect = rect;
                this.rows = Math.ceil(Map.getStyle(this.el, "height") / rect);
                this.cells = Math.ceil(Map.getStyle(this.el, "width") / rect);
                Map.setStyle(this.el, "width", this.cells * rect);
                Map.setStyle(this.el, "height", this.rows * rect);
            }
            static getStyle(el, attr) {
                return parseFloat(getComputedStyle(el)[attr]);
            }
            static setStyle(el, attr, val) {
                el.style[attr] = val + "px";
            }
            clearData() {
                this.data.length = 0;
            }
            setData(data) {
                this.data = this.data.concat(data);
            }
            include(data) {
                return this.data.some(item => (item.x == data.x && item.y == data.y));
            }
            render() {
                this.el.innerHTML = this.data.map(item => {
                    return `
                <span style="width:${this.rect}px;height:${this.rect}px;position:absolute;left:${this.rect * item.x}px;top:${this.rect * item.y}px;background:${item.color};"></span>
            `;
                }).join("");
            }
        }
        class foot {
            constructor(map) {
                this.map = map;
                this.color = ["red", "green", "pink", "yellow"];
                this.data = null;
            }
            create() {
                let data = {};
                data.x = Math.floor(Math.random() * this.map.cells);
                data.y = Math.floor(Math.random() * this.map.rows);
                if (this.map.include(data)) {
                    this.create();
                } else {
                    data.color = this.color[Math.floor(Math.random() * this.color.length)];
                    this.map.setData(data);
                    this.data = data;
                }
            }
        }
        class Shake {
            constructor() {
                this.data = [
                    { x: 6, y: 2, color: "blue" },
                    { x: 5, y: 2, color: "white" },
                    { x: 4, y: 2, color: "white" },
                    { x: 3, y: 2, color: "white" },
                    { x: 2, y: 2, color: "white" }
                ];
                this.direction = "right";
            }
            move() {
                var i = this.data.length - 1;
                this.LastData = {
                    x: this.data[i].x,
                    y: this.data[i].y,
                    color: this.data[i].color
                }
                for (; i > 0; i--) {
                    this.data[i].x = this.data[i - 1].x;
                    this.data[i].y = this.data[i - 1].y;
                }
                switch (this.direction) {
                    case "left": this.data[0].x -= 1; break;
                    case "right": this.data[0].x += 1; break;
                    case "top": this.data[0].y -= 1; break;
                    case "bottom": this.data[0].y += 1; break;
                }
            }
            eatfoot(foot) {
                if (foot.data.x == this.data[0].x
                    && foot.data.y == this.data[0].y) {
                    this.data.push(this.LastData);
                    return true;
                }
                return false;
            }

        }
        class Game {
            constructor(el, rect) {
                this.map = new Map(map, rect);
                this.foot = new foot(this.map);
                this.shake = new Shake();
                this.timer = 0;
                this.grade = 0;
                this.interval = 200;
                this.control();
            }
            start() {
                this.foot.create();
                this.move();
            }
            stop() {
                clearInterval(this.timer);
            }
            move() {
                this.stop();
                this.timer = setInterval(() => {
                    this.shake.move();
                    this.map.clearData();
                    if (this.shake.eatfoot(this.foot)) {
                        this.foot.create();
                        this.grade++;
                        this.gradeChange && this.gradeChange();
                        if (this.interval > 10) {
                            this.stop();
                            this.interval -= 5;
                            this.move();
                        }
                    }
                    this.map.setData(this.foot.data);
                    this.map.setData(this.shake.data);
                    if (this.isOver()) {
                        this.stop();
                        alert("game over");
                        return;
                    }
                    this.map.render();
                }, this.interval)
            }
            isOver() {
                // 检测蛇和自己相撞
                for (let i = 1; i < this.shake.data.length; i++) {
                    if (this.shake.data[i].x == this.shake.data[0].x
                        && this.shake.data[i].y == this.shake.data[0].y) {
                        return true;
                    }
                }
                // 检测蛇超出
                if (this.shake.data[0].x < 0
                    || this.shake.data[0].x >= this.map.cells
                    || this.shake.data[0].y < 0
                    || this.shake.data[0].y >= this.map.rows) {
                    return true;
                }
                return false;
            }
            control() {
                window.addEventListener("keydown", ({ keyCode }) => {
                    if (this.shake.direction == "top" || this.shake.direction == "bottom") {
                        this.shake.direction = keyCode == 37 ? "left" : (keyCode == 39 ? "right" : this.shake.direction);
                    } else if (this.shake.direction == "left" || this.shake.direction == "right") {
                        this.shake.direction = keyCode == 38 ? "top" : (keyCode == 40 ? "bottom" : this.shake.direction);
                    }
                });
            }
        }
        {
            let map = document.querySelector("#map");
            let game = new Game(map, 10);
            game.start();
            game.gradeChange = function () {
                console.log(game.grade);
            };
        }
    </script>
</body>

</html>
```

## 完整代码二

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>贪吃蛇</title>
    <style>
        #map {
            position: relative;
            /* left: 50%; */
            width: 400px;
            height: 400px;
            background: #000;
        }
    </style>
</head>

<body>
    <h1 id="grade">0</h1>
    <div id="map"></div>

    <script>
        //地图类
        class Map {
            constructor(el, rect) {
                this.el = el
                this.rect = rect
                // 数据
                this.data = [
                    // {
                    //     x: 0,
                    //     y: 0,
                    //     color: 'red'
                    // }
                ]
                // 向上取整
                this.rows = Math.ceil(Map.getStyle(el, "height") / rect)
                this.cells = Math.ceil(Map.getStyle(el, "width") / rect)
                Map.setStyle(el, "height", this.rows * rect)
                Map.setStyle(el, "width", this.cells * rect)
            }
            // 获取多少份
            static getStyle(el, attr) {
                // 解析一个字符串，并返回一个浮点数。
                return parseFloat(getComputedStyle(el)[attr])
            }
            // 设置地图的长宽
            static setStyle(el, attr, val) {
                el.style[attr] = val + "px"
            }
            // 设置数据
            setData(newData) {
                this.data = this.data.concat(newData)
            }
            // 清空数据
            clearData() {
                this.data.length = 0
            }
            //
            include({ x, y }) {
                // some方法用于检测数组中的元素是否满足指定条件（函数提供）
                return this.data.some(item => (item.x == x && item.y == y))
            }
            //将数据渲染在地图上
            render() {
                this.el.innerHTML = this.data.map(item => {
                    return `<span style="position: absolute;left:${item.x * this.rect}px ;top:${item.y * this.rect}px ;width: ${this.rect}px ;height:${this.rect}px  ;background: ${item.color} ;"></span>`
                }).join("")
            }
        }

        class foot {
            constructor(map) {
                this.map = map
                this.data = null
                this.create()
            }
            create() {
                // 0-39
                // 获取x
                let x = Math.floor(Math.random() * this.map.cells)
                // 获取y
                let y = Math.floor(Math.random() * this.map.rows)
                let color = 'red'
                console.log(x, y, color)
                //将data设置好
                this.data = { x, y, color }
                // 判断是否包含了这个点
                if (this.map.include(this.data)) {
                    // 包含了就重新弄下
                    this.create()
                }
                this.map.setData(this.data)
            }
        }
        class Snake {
            constructor(map, foot) {
                this.data = [
                    { x: 6, y: 2, color: 'blue' },
                    { x: 5, y: 2, color: 'white' },
                    { x: 4, y: 2, color: 'white' },
                    { x: 3, y: 2, color: 'white' },
                    { x: 2, y: 2, color: 'white' },
                ]
                this.map = map
                this.foot = foot
                this.direction = "right"
                this.map.setData(this.data)

            }
            move() {
                let i = this.data.length - 1
                this.lastData = {
                    x: this.data[i].x,
                    y: this.data[i].y,
                    color: this.data[i].color
                }
                /*
                让后每一个走到前面的位置
                */
                for (let i = this.data.length - 1; i > 0; i--) {
                    this.data[i].x = this.data[i - 1].x
                    this.data[i].y = this.data[i - 1].y
                }
                //
                switch (this.direction) {
                    case "left":
                        this.data[0].x--
                        break
                    case "right":
                        this.data[0].x++
                        break
                    case "top":
                        this.data[0].y--
                        break
                    case "bottom":
                        this.data[0].y++
                        break
                }
            }
            //方向改变 
            // 现在本身是上下移动 就只能左右
            changeDir(dir) {
                if (this.direction === "left" || this.direction === "right") {
                    if (dir === "left" || dir === "right") {
                        return false //不能修改方向
                    }
                    this.direction = dir
                } else {
                    if (dir === "top" || dir === "bottom") {
                        return false //不能修改方向
                    }
                    this.direction = dir
                }
            }
            // 吃食物
            eatfoot() {
                this.data.push(this.lastData)
            }

        }

        class Game {
            constructor(el, rect) {
                this.map = new Map(el, rect)
                this.foot = new foot(this.map)
                this.snake = new Snake(this.map)
                //初始化
                this.map.render()

                this.timer = 0
                this.interval = 200
                this.keyDown = this.keyDown.bind(this)
                this.grade = 0
                this.control();
            }

            // 开始游戏
            start() {
                this.move()
            }
            //暂停游戏
            stop() {
                clearInterval(this.timer)
            }
            // 控制移动
            move() {
                this.stop()
                this.timer = setInterval(() => {
                    this.snake.move()
                    this.map.clearData()
                    if (this.isEat()) {
                        console.log("吃到食物")
                        this.grade++;
                        this.snake.eatfoot()
                        this.foot.create()
                        this.changeGrade(this.grade)
                        this.interval *= .9
                        this.stop()
                        this.start()
                    }
                    if (this.isOver()) {
                        this.over()
                        return
                    }
                    this.map.setData(this.snake.data)
                    this.map.setData(this.foot.data)
                    this.map.render()
                }, this.interval);
            }
            //是否吃到东西
            isEat() {
                return this.snake.data[0].x === this.foot.data.x && this.snake.data[0].y === this.foot.data.y
            }
            //是否结束
            isOver() {
                //判断蛇出了地图
                if (this.snake.data[0].x < 0 || this.snake.data[0].x >= this.map.cells || this.snake.data[0].y < 0 || this.snake.data[0].y >= this.map.cells) {
                    return true
                }
                for (let i = 1; i < this.snake.data.length; i++) {
                    if (this.snake.data[0].x == this.snake.data[i].x && this.snake.data[0].y == this.snake.data[i].y) {
                        return true
                    }
                }
                return false

            }
            //结束
            over() {
                this.stop()
            }
            //改变分数
            changeGrade(grade) {
                console.log("分数：" + grade)

            }
            keyDown({ keyCode }) {
                console.log(keyCode)
                switch (keyCode) {
                    case 37:
                        this.snake.changeDir("left")
                        break
                    case 38:
                        this.snake.changeDir("top")
                        break
                    case 39:
                        this.snake.changeDir("right")
                        break
                    case 40:
                        this.snake.changeDir("bottom")
                        break
                }
            }
            //控制器
            control() {
                window.addEventListener("keydown", this.keyDown)
            }
        }

        {
            let map = document.querySelector('#map');
            let game = new Game(map, 10)
            game.start()
            // gameMap.render();
            // setInterval(() => {
            //     gameSnake.move()
            //     gameMap.clearData()
            //     gameMap.setData(gameSnake.data)
            //     gameMap.render()
            // }, 100);
            // setInterval(() => {
            //     gameSnake.changeDir('bottom')
            //     gameSnake.eatfoot();
            // }, 500);
        }



    </script>
</body>

</html>
```

## 总结

要考虑单一原则 ：要进行拆分类 将每个都单一化 （一个类应该是单一完整的东西）

低耦合：也就是说两个类直接尽量减少联系

高内聚：也就是说写蛇那么就在蛇的类里面把需要的都写齐了

