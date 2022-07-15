---
title: Angular基础知识之快速入门
categories: 技术笔记
tags:
  - Angular
excerpt: Angular基础知识之快速入门
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201213103202.png'
articleLink: 52382ed6
date: 2020-12-12 17:47:48
---

## 脚手架

学习的第一步会使用脚手架创建项目

```
npm install @angular/cli
```

初始化一个新项目 需要运行 CLI 命令 `ng new` 并提供 `my-app` 名称作为参数

```
ng new my-app
```

`ng new` 命令会提示你提供要把哪些特性包含在初始应用中

按 Enter 或 Return 键可以接受默认值

### 运行

```
cd my-app
ng serve --open
```

`ng serve` 命令会启动开发服务器、监视文件，并在这些文件发生更改时重建应用

`--open`（或者只用 `-o` 缩写）选项会自动打开你的浏览器，并访问 `http://localhost:4200/`。

![angular](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/angularInit0026464.png)

## 组件

组件是 Angular 应用的主要构造块。每个组件包括如下部分：

- 一个 HTML 模板，用于声明页面要渲染的内容
- 一个用于定义行为的 Typescript 类
- 一个 CSS 选择器，用于定义组件在模板中的使用方式
- （可选）要应用在模板上的 CSS 样式

### 创建一个组件

使用 Angular CLI 创建一个组件：

```
ng generate component <component-name> 
```

该命令会创建以下内容：

- 一个以该组件命名的文件夹
- 一个组件文件 `<component-name>.component.ts`
- 一个模板文件 `<component-name>.component.html`
- 一个 CSS 文件， `<component-name>.component.css`
- 测试文件 `<component-name>.component.spec.ts`

其中 `<component-name>` 是组件的名称。

#### 创建目录下的组件

```
ng generate component views/home
```

这个意思就是创建app下单 views文件下单home组件

#### 更换根组件

创建完组件你想去挂载为根组件 不使用app初始化组件的话你需要修改两个地方

第一：我们需要去 `app.module.ts`中导入我们需要设置为根组件的组件

并且声明在 `declarations` 中

然后我们还需要修改 `bootstrap` 为我们导入的名称

第二：我么需要去 根目录下 找到`index.html`

初始化的时候是 

```
<body>
  <app-root></app-root>
</body>
```

我们需要修改成我们组件的名称

这个名称跟我们 `xxxx.component.ts` 中	`selector`需要保持一致

那么我们修改 

```
<app-root></app-root>
```

改为我们新的组件名称

我们就更换了 根组件 （但不太推荐）

## 基础语法

我们接下来写的代码都会在`app`组件去编写

 `app.component.html` ：来写我们的 html结构

`app.component.scss`：我们的css文件

`app.component.ts`：编写我们js的地方

`app.component.spec.ts`：这是我们的测试文件暂时不用管

### 模版语法

这这里面我们就去写html就可以 还可以去写 `angular` 提供的一些模版语法

#### 插值

angular的差值表达式和Vue是一样的哈都是双大括号

```
{{...}}
```

我们可以插入我们在`app.component.ts`

```
export class AppComponent {
  title = 'my-new-app';
  myData = "www.79bk.cn"
}
```

在这里我们定义了两个变量 `title`,`myData`

然后我们用差值表达式渲染到页面上面去

```
<h1>
  我是插值表达式渲染的：{{myData}}
</h1>
```

![插值表达式](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/image-20201213091132014.png)

在插值中我们可以进行运算和调用方法

```
<!-- "The sum of 1 + 1 is not 4" -->
<p>The sum of 1 + 1 is not {{1 + 1 + getVal()}}.</p>
```

Angular 对所有双花括号中的表达式求值，把求值的结果转换成字符串

并把它们跟相邻的字符串字面量连接起来

最后，把这个组合出来的插值结果赋给元素或指令的属性

#### 模版样式

我们给标签属性进行动态赋值的时候可以使用 以下三种方法都是一样的效果

```
<h1 class="{{classStr}}">class1</h1>
<h1 [class]="classStr">class2</h1>
<h1 [attr.class]="classStr">class3</h1>
```

上面说过 如果我们 写多个会合并为一个 

```
<h1 class="myClass" class="{{classStr}}">class1</h1>
```

渲染出的结果会是一个class 两个结果会合并在一个class

```
<!-- 对象模式 -->
<h1 [class]="classObj">class5</h1>
<h1 [class]="{bgBlue:isShow}">class6</h1>
<!-- 数组模式 -->
<h1 [class]="['bgBlue','active','abc']">class7</h1>
<h1 [class]="classArr"></h1>
```

对象模式:我们需要进行一个特定的格式

```
classObj={
	bgBlue:true,
	active:false
}
// 只有为true 才会渲染
```

如果我我们想给 标签加上自定义属性

```
<h1 data-index="{{data}}">class1</h1>
```

这样是不可以的 我们需要 

```
<h1 [attr.data-index]="data">class1</h1>
```

插入html 我们需要使用特定的 要不就会转换为 字符串 渲染到页面上面

```
<h1 [innerHtml]="htmlData">class1</h1>
```

#### 事件绑定

```
<button (click)="onSave()">Save</button>
```

意思为 单击事件去 执行 onSave 方法

#### if判断

```
<h2 *ngIf="isActive"></h2>
```

不要忘了 `ngIf` 前面的星号（`*`）。

当 `isActive` 表达式返回真值时，`NgIf` 会把 `ItemDetailComponent` 添加到 DOM 中。当表达式为假值时，`NgIf` 将从 DOM 中删除 `ItemDetailComponent`，从而销毁该组件及其所有子组件。

#### For循环

```
<div *ngFor="let item of items">{{item.name}}</div>
```

`items` 是我们的数组数据 

我们还可以定义 index 索引

```
<div *ngFor="let item of items; let i=index">{{i + 1}} - {{item.name}}</div>
```

#### Switch指令

NgSwitch 类似于 JavaScript `switch` 语句。它根据切换条件显示几个可能的元素中的一个。Angular 只会将选定的元素放入 DOM。

`NgSwitch` 实际上是三个协作指令的集合： `NgSwitch`，`NgSwitchCase` 和 `NgSwitchDefault`，如以下范例所示。

```
<div [ngSwitch]="currentItem.feature">
  <app-stout-item    *ngSwitchCase="'stout'"    [item]="currentItem"></app-stout-item>
  <app-device-item   *ngSwitchCase="'slim'"     [item]="currentItem"></app-device-item>
  <app-lost-item     *ngSwitchCase="'vintage'"  [item]="currentItem"></app-lost-item>
  <app-best-item     *ngSwitchCase="'bright'"   [item]="currentItem"></app-best-item>
	<!-- . . . -->
  <app-unknown-item  *ngSwitchDefault           [item]="currentItem"></app-unknown-item>
</div>
```

## 数据双向绑定

我们需要先导入模块否则是无法使用的

```
import { FormsModule } from '@angular/forms'
```

导入后我们还需进行声明要不也是无法进行使用的

```
imports: [
    FormsModule
],
```

然后我们在 html中就可以使用双向数据绑定了

```
<input type="text" [(ngModel)]="inputData">
<h2>{{inputData}}</h2>
```

我们在 js 中定义 `inputData` 变量

![双向数据绑定](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/shuangxiangshujubaongding234.gif)

### 临时变量

这个和Vue,react中的Ref 非常相似

我们在标签上面设置一个

```
<input #input1 type="text" >
```

那么我们就可以传递这个变量`input1`

我们在写一个方法

```
<button (click)="getContent(input1,input2)">获取账号密码</button>
```

在`getContent`方法中进行接收这两个变量 

打印出 这两个变量存储的是 Dom 对象我们就可以获取这个input的值等结果

### 管道符

管道用来对字符串、货币金额、日期和其他显示数据进行转换和格式化。

这个和我们Vue中的过滤器很相似

Angular 为典型的数据转换提供了内置的管道：

- `DatePipe`：根据本地环境中的规则格式化日期值。
- `UpperCasePipe`：把文本全部转换成大写。
- `LowerCasePipe`：把文本全部转换成小写。
- `CurrencyPipe`：把数字转换成货币字符串，根据本地环境中的规则进行格式化。
- `DecimalPipe`：把数字转换成带小数点的字符串，根据本地环境中的规则进行格式化。
- `PercentPipe`：把数字转换成百分比字符串，根据本地环境中的规则进行格式化。

```
<p>The hero's birthday is {{ birthday | UpperCasePipe }}</p>
```

管道传递参数需要在管道后面去写

```
<p>The hero's birthday is {{ birthday | UpperCasePipe:'参数' }}</p>
```

当然我们还可以自己进行编写管道

#### 自定义管道

我们需要先进行创建一个管道文件

```
ng g pipe filter/myGuandaoName
```

`filter/myGuandaoName` 自定义的文件路径

创建好后会是这个样子：

```
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'myGuandaoName'
})
export class MyGuandaoNamePipe implements PipeTransform {

  transform(value: unknown, ...args: unknown[]): unknown {
    return null;
  }

}
```

我们需要在 `transform` 中进行定义自己的规则