---
title: Angular基础知识之指令的自定义创建
categories: 技术笔记
tags:
  - Angular
  - 指令
excerpt: Angular基础知识之指令的自定义创建
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201213115138.png'
articleLink: e0bacd00
date: 2020-12-13 11:06:10
---

### 自定义指令

我们使用CLI命令自动创建

```
ng g directive directive/lcstyle
```

我们执行完这个命令后会给我们创建好文件

```
import { Directive } from '@angular/core';

@Directive({
  selector: '[appLcstyle]'
})
export class LcstyleDirective {
  constructor() { }
}
```

`selector` : 是我们在页面标签上进行调用的

我们在 这个指令文件中进行导入 `input` 因为我们需要接收指令传递过来的数据

```
import { Directive, Input } from '@angular/core';
```

然后我们可以获取到数据

在生命周期中可以进行打印

```
import { Directive, Input } from '@angular/core';

@Directive({
  selector: '[appLcstyle]'
})
export class LcstyleDirective {
  @Input() appLcstyle: any;
  constructor() { }
  // 生命周期
  ngOnChanges() {
    console.log(this.appLcstyle)
  }
}
```

### 获取指令所在的Dom对象

我们需要导入 `ElementRef` 来获取 当前指令所在的dom元素

```
import { Directive, Input, ElementRef } from '@angular/core';
```

但是我们导入后没有办法直接进行使用

我们需要进行依赖注入

我们需要在`constructor` 进行注入

```
import { Directive, Input, ElementRef } from '@angular/core';

@Directive({
  selector: '[appLcstyle]'
})
export class LcstyleDirective {
  @Input() appLcstyle: any;
  constructor(public ref:ElementRef) { 
  	// 这里注入依赖
  }
  // 生命周期
  ngOnChanges() {
    console.log(this.appLcstyle)
    // 注入好了之后我们就可以使用这个 ref 了
    console.log(this.ref)
  }
}
```

`public ref:ElementRef` 意思是`public` 声明一个变量 类型是`ElementRef`

![image-20201213114810569](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201213114810569.png)

然后我们就可以 进行操作这个 节点元素了！