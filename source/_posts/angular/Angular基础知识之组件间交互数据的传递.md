---
title: Angular基础知识之组件间交互数据的传递
categories: 技术笔记
tags:
  - Angular
  - 组件
excerpt: Angular基础知识之组件间交互数据的传递
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201213115118.png'
articleLink: 335bdee6
date: 2020-12-13 10:36:30
---

## 组件传递

我们在父组件中倒入子组件 直接写上子组件的名称即可

```
<!-- 渲染子组件 -->
<app-my-name></app-my-name>
```

#### 父组件向子组件传递数据

父组件中 定义数据然后进行传递

```
<app-my-name [itemData]="data"></app-my-name>
```

这样直接传递是不可以的

我们需要在子组件中 使用 `@input`装饰器 进行先行定义然后才可以进行接收

```
// 子组件中导入 input
import { Component, OnInit ,Input} from '@angular/core';
```

然后定义接收的变量名称

```
export class MyNameComponent implements OnInit {
  // 在这里进行定义 
  // 因为是个装饰器 所以需要加上个 @ 
  @Input() item: any;
  
  constructor() { }
  ngOnInit(): void {
  }
}
```

然后我们可以直接在子组件页面中进行渲染

```
<p>my-name works!</p>
<p>父组件传递：{{item}}</p>
```

#### 子组件向父组件传递数据

##### 子组件

我们需要在子组件中引入自定义事件`Output` `EventEmitter`

```
import { Component, OnInit ,Output,EventEmitter} from '@angular/core';
```

然后我们实例化一个事件对象

```
@Output() childMsg = new EventEmitter()
// childMsg 是我们自定义的
```

我们在子组件中定义一个按钮并给个事件

```
<button (click)="clickBtn()">子向父传递数据</button>
```

然后我们在这个`clickBtn`事件中调用我们创建的自定义事件

```
clickBtn() {
		// 传递参数对象
    this.childMsg.emit({
      url: '79bk.cn'
    })
  }
```

##### 父组件

我们需要在父组件中进行监听这个自定义事件来接收数据

```
<app-my-name (childMsg)="getData($event)"></app-my-name>
```

`childMsg` : 子组件中自定义的事件

`getData` ：需要我们在父组件中进行定义的事件来接收传递过来的变量

```
getData(event: any) {
		console.log(event)
}
```

