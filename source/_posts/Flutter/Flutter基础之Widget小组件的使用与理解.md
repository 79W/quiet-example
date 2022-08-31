---
title: Flutter基础之Widget小组件的使用与理解
categories: 技术笔记
tags:
  - Flutter
  - Dart
  - Widget
excerpt: Flutter基础之Widget小组件的使用与理解
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201216155042.png'
articleLink: f15d4dbe
date: 2020-12-16 15:20:21
---

## Hello World

我们需要在手机页面中间显示一个`Hello World` 字样 字体大小为50并且颜色为蓝色

首先我们创建好项目

```
flutter create my_app
```

我们会看到 `lib`文件夹下面有个`main.dart` 文件 这就是我们的主文件 

我们首先将里面的内容全部删除掉 然后从头开始进行编写和理解

```
import 'package:flutter/material.dart';

main() {
  // 打开app
  runApp(Text("Hello World", textDirection: TextDirection.ltr));
}
```

`runApp` 是Flutter内部提供的一个函数，当我们启动一个Flutter应用程序时就是从调用这个函数开始的

```
// 内部代码
void runApp(Widget app) {
  ...省略代码
}
```

该函数让我们传入一个东西：`Widget`   当我们翻译后会发现中文为 小装置 的意思 （简单理解就是小组件）

而在 Flutter 中我们要有一个基本认识 `Flutter中万物皆Widget`

而我们传入的`Text()` 就是内部给我们提供的组件(Widget)

当我们运行后 你会发现页面是全黑 并且 `Hello World`  在左上方

![HelloWorld](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201216153219964.png)

#### 需求

- 我们可能希望文字居中显示，并且可以大一些；
- **居中显示：** 需要使用另外一个Widget，`Center`；
- **文字大一些：** 需要给Text文本设置一些样式；

我们在Text小部件外层包装了一个Center部件，让Text作为其child；

并且，我们给Text组件设置了一个属性：style，对应的值是TextStyle类型；

```
import 'package:flutter/material.dart';

main() {
  runApp(
    Center(
      child: Text(
        "Hello World",
        textDirection: TextDirection.ltr,
        style: TextStyle(fontSize: 36,color: Colors.blue),
      ),
    )
  );
}
```

## 界面结构

目前我们虽然可以显示HelloWorld，但是我们发现最底部的背景是黑色，并且我们的页面并不够结构化。

正常的App页面应该有一定的结构，比如通常都会有`导航栏`，会有一些`背景颜色`等

在开发当中，我们并不需要从零去搭建这种结构化的界面，我们可以使用Material库，直接使用其中的一些封装好的组件来完成一些结构的搭建。

```
import 'package:flutter/material.dart';
main() {
  runApp(
    MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text("Flutter"),
        ),
        body: Center(
          child: Text(
            "Hello World",
            textDirection: TextDirection.ltr,
            style: TextStyle(fontSize: 36),
          ),
        ),
      ),
    )
  );
}
```

![界面](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201216153551586.png)

在最外层包裹一个**MaterialApp**

- 这意味着整个应用我们都会采用MaterialApp风格的一些东西，方便我们对应用的设计，并且目前我们使用了其中两个属性；
- title：这个是定义在Android系统中打开多任务切换窗口时显示的标题；（暂时可以不写）
- home：是该应用启动时显示的页面，我们传入了一个Scaffold；

**Scaffold**是什么呢？

- 翻译过来是`脚手架`，脚手架的作用就是搭建页面的基本结构；
- 所以我们给MaterialApp的home属性传入了一个Scaffold对象，作为启动显示的Widget；
- Scaffold也有一些属性，比如`appBar`和`body`；
- appBar是用于设计导航栏的，我们传入了一个`title属性`；
- body是页面的内容部分，我们传入了之前已经创建好的Center中包裹的一个Text的Widget；

## 代码重构

这样我们会发现`嵌套太多`了 总感觉是别扭 不宜阅读代码

这时候就需要我们自己来写 `Widget`  小组件了

我们可以继承自StatelessWidget或者StatefulWidget来创建自己的Widget类

- **StatelessWidget：** 没有状态改变的Widget，通常这种Widget仅仅是做一些展示工作而已；
- **StatefulWidget：** 需要保存状态，并且可能出现状态改变的Widget；

```
import 'package:flutter/material.dart';
main() {
  // 启动app
  runApp(MaterialApp(
      title: 'qiao',
      home: Scaffold(
          appBar: AppBar(
            title: Text('我是标题'),
          ),
          body: ConWif())));
}

class ConWif extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        "我是内容",
        textDirection: TextDirection.ltr,
        style: TextStyle(fontSize: 50, color: Colors.blue),
      ),
    );
  }
}
```

这样我们就可以 代码会更加的清晰 如果你觉得还可以拆开依然是可以继续拆的