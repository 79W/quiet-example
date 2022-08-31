---
title: Flutter基础之StatefulWidget基本使用
categories: 技术笔记
tags:
  - Flutter
  - Dart
  - Widget
  - StatefulWidget
excerpt: Flutter基础之StatefulWidget基本使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201216195510.png'
articleLink: 3080a4fe
date: 2020-12-16 19:36:35
---

我们在创建项目的时候 默认给我们携带的程序中是一个按钮点后页面的数字会随着改变

这是我们使用 `StatelessWidget` 不可以实现的 因为在我们继承了`StatelessWidget` 后所有的变量都要设置成 `final` 类型 而这个类型是不可变属性

```
@immutable
abstract class Widget extends DiagnosticableTree {
	// ...省略代码
}
```

![immutable](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201216194027195.png)

**结论：** 定义到Widget中的数据一定是不可变的，需要使用final来修饰

当我们要实现动态数据改变的时候需要继承 `StatefulWidget`

### StatefulWidget

我们来实现一个 和初始化项目一样 的小案例

- 因为当点击按钮时，数字会发生变化，所以我们需要使用一个StatefulWidget，所以我们需要创建两个类；
- MyCounterWidget继承自StatefulWidget，里面需要实现createState方法；
- MyCounterState继承自State，里面实现build方法，并且可以定义一些成员变量；

```
class MyCounterWidget extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    // 将创建的State返回
    return MyCounterState();
  }
}
```

下面这个类才是真正的去操作数据的类

```
class MyCounterState extends State<MyCounterWidget> {
  int counter = 0;
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text("当前计数：$counter", style: TextStyle(fontSize: 30),),
    );
  }
}
```

接下来我们来实现 一个按钮 我们可以使用内置 的 按钮小组件 

```
RaisedButton(
	// 按钮文字
  child: Text('加'),
  // 按钮颜色
  color: Colors.blue,
  // 按钮字的颜色
  textColor: Colors.white,
  // 点击事件
  onPressed: () => {
    print('点击了')
	}
),
```

这个是我们`flutter ` 内部提供的一个按钮小组件 但是大家会发现 跟初始化那个加号小按钮不一样

当然我们默认案例中使用的是 `FloatingActionButton` 按钮组件 用什么都是可以 的

#### 状态改变

接下来就是我们点击给数字加一就可以了 我们第一反应是在 点击事件中直接++

```
// 点击事件
onPressed: () => {
		counter++
}
```

然而这样是不会更新我们的页面但是数据会被更新 只是页面没有发生更新

我们需要使用和 `React` 差不多的一个方法来更新页面

```
 setState(() {})
```

是不是和 `React` 的很相似 但是也有不一样的地方这里是执行的一个匿名函数

我们可以将数据状态改变放在外面也可以放在内部都是可以的这个 `setState` 主要作用是更新页面状态

```
onPressed: () => {
  setState(() {
 		 myNum++;
  })
}),
```

然后我们就完成了点击按钮从而更新页面数据

大家可以实现一下 添加一个点击减掉一的按钮事件

#### 完整代码

```
import 'package:flutter/material.dart';

main(List<String> args) {
  runApp(myApp());
}

class myApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "名称",
      home: myStateone(),
    );
  }
}

class myStateone extends StatefulWidget {
  @override
  myState createState() => myState();
}

class myState extends State {
  int myNum = 0;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("标题")),
      body: Center(
        child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              RaisedButton(
                  child: Text('加'),
                  color: Colors.blue,
                  textColor: Colors.white,
                  onPressed: () => {
                        setState(() {
                          myNum++;
                        })
                      }),
              Text(
                "当前数字：$myNum",
                style: TextStyle(fontSize: 30, color: Colors.red),
              ),
            ]),
      ),
    );
  }
}

```

