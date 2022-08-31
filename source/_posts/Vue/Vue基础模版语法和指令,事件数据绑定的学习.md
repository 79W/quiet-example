---
title: 'Vue基础模版语法和指令,事件数据绑定的学习'
categories: 技术笔记
tags:
  - Vue
  - 数据绑定
excerpt: 'Vue基础模版语法和指令,事件数据绑定的学习'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201101114031.png'
articleLink: 55177fe4b8873601
date: 2020-11-01 08:20:36
---

## Vue

渐进式JavaScript框架

声明式渲染->组件系统->客户端路由->集中式状态管理->项目构建

- 易用：熟悉`HTML` `CSS` `JavaScript`知识后可以快速上手Vue
- 灵活：在一个库和一套完整的框架之间自由伸缩
- 高效：20kb运行大小, 超快虚拟DOM

### 基本使用

首先我们导入包（官网可以下载）

```
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

确定一个Dom

```
<body>
		<div id="app">
			{{msg}}
		</div>
	</body>
```

声明vue

```
	<script type="text/javascript">
		var vm = new Vue({
			el:'#app',
			data:{
				msg:'Hello Vue'
			}
		})
	</script>
```

当我们刷新页面就会发现页面上会呈现出 `Hello Vue`

**实例参数分析**

- el ：元素的挂载位置（值可以是css选择对象或者DOM元素）
- data：模型数据（值是一个对象）

**插值表达式用法**

- 将数据填充到HTML标签中
- 插值表达式支持基本的计算操作

**Vue运行原理的分析**

- Vue语法->原生语法
- ![VUE运行](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101084054006.png)

### Vue模版语法

Vue.js 使用了基于 HTML 的模板语法

允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。

结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

#### 文本

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

就我们上面所使用的方法

```
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` property 的值。

无论何时，绑定的数据对象上 `msg` property 发生了改变，插值处的内容都会更新。

通过使用 `v-once 指令`，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```
<span v-once>这个将不会改变: {{ msg }}</span>
```

接下来我们来学习指令

#### 指令

本质就是自定义属性 格式是以v-开始如（v-cloak）

#### v-cloak

这个指令保持在元素上直到关联实例结束编译。

和 CSS 规则如 `[v-cloak] { display: none }` 一起用时

这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

通俗的说就是 插值表达式存在闪动问题 

```
[v-cloak] {
  display: none;
}
```

```
<div v-cloak>
  {{ message }}
</div>
```

原理：先通过样式隐藏内容，然后在内存中进行值得替换，替换好了之后再显示最终的内容

#### v-text

更新元素的 `textContent`。

如果要更新部分的 `textContent`，需要使用 `{{ Mustache }}` 插值。

```
<span v-text="msg"></span>
<!-- 和下面的一样 -->
<span>{{msg}}</span>
```

#### v-html

更新元素的 `innerHTML`。

**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**

如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代。

> 在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 XSS 攻击。只在可信内容上使用 `v-html`，**永不**用在用户提交的内容上。

```
<div v-html="html"></div>
// 数据为 html：'<p>我是html</p>'
```

#### v-pre

跳过这个元素和它的子元素的编译过程。

可以用来显示原始 Mustache 标签。

跳过大量没有指令的节点会加快编译。

```
<span v-pre>{{ msg }}</span>
```

在页面上就会显示 `{{ msg }}` 没有编译的结果

### 数据响应式

数据变动改变页面内容的变化

这样说还是比较的抽象 我们在浏览器当中看下

我们在控制台中输入我们的 `vm` 是我们声明Vue时候定义的名

![image-20201101092741980](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101092741980.png)

 我们可以看出这里给我们打印出来这个变量 `msg`

我们还可以获取到这个变量 

![image-20201101093000140](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101093000140.png)

并且我们可以给他改值

![gaibianshujukk](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/gaibianshujukk.gif)

### 双向数据绑定

![image-20201101093325030](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101093325030.png)

```
<input type="text" v-model="msg" />
```

当我input输入框内值的变化 会联动着我们 `v-model` 所绑定的数据的变化

**MVVM设计思想**

M(model)  V(view)  VM(view-model)

![MVVM](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101094636938.png)

### 事件绑定

#### v-on

我们给按钮添加一个点击事件 `doThis` 方法

```
<button v-on:click="doThis()"></button>
```

我们定义这个方法

```
var vm = new Vue({
    el:'#app',
    data:{
      msg:'Hello Vue',
      num :0
    },
    methods:{
      doThis:function(){
      	this.num ++
      }
    }
})
```

当我们点击的时候就会出发我们的方法 然后给我们的 `num` 加一

我们还有缩写 `@`

```
<button @click="doThis"></button>
```

也是一样的去执行这个函数

我们看到了我们定义的方法中使用了 `this ` 我们要知道 这个指向的是 我们定义的`vm`

##### 传递事件对象

```
<button v-on:click="doThis($event)"></button>
```

接收

```
doThis:function(event){
     console.log(event)
}
```

如果你是下面这种方式调用 事件对象就会默认传递第一个参数

```
<button v-on:click="doThis"></button>
```

##### 事件修饰符

- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

**调用方法**

```
<button v-on:click.stop="doThis"></button>
```

```
<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>
```

```
<!-- 键修饰符，键别名 -->
<input @keyup.enter="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">
```

### 属性绑定

#### v-bind

动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

在绑定 `class` 或 `style` attribute 时，支持其它类型的值，如数组或对象。

在绑定 prop 时，prop 必须在子组件中声明。

可以用修饰符指定不同的绑定类型。

没有参数时，可以绑定到一个包含键值对的对象。

注意此时 `class` 和 `style` 绑定不支持数组和对象。

```
<!-- 绑定一个 attribute -->
<img v-bind:src="imageSrc">
```

我们可以缩写

```
<!-- 绑定一个 attribute -->
<img :src="imageSrc">
```

**样式**

```
<!-- class 绑定 -->
<div :class="{ red: isRed }"></div>
```

我们需要定义 `isRed` 变量 为`true`和`false`

```
<div :class="[classA, classB]"></div>
<div :class="[classA, { classB: isB, classC: isC }]">
```

数组的形式我们需要定义变量 `classA:这里写class对应的名字`

我们可以定义对象和数组类型然后之间传递也是可以的

##### 修饰符

- `.prop` - 作为一个 DOM property 绑定而不是作为 attribute 绑定。
- `.camel` - (2.1.0+) 将 kebab-case attribute 名转换为 camelCase。(从 2.1.0 开始支持)
- `.sync` (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 `v-on` 侦听器。

### 分支循环结构

#### v-if

我们`javascript` 中的if是什么样子的 `if(){}else if(){}else{}`

那么我们在Vue 当中也是可以这样的

```
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else>
  Not A/B/C
</div>
```

#### v-for

基于源数据多次渲染元素或模板块。

此指令之值，必须使用特定语法 `alias in expression`，为当前遍历的元素提供别名：

```
<div v-for="item in items">
  {{ item.text }}
</div>
```

另外也可以为数组索引指定别名 (或者用于对象的键)：

```
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>
```

`v-for` 的默认行为会尝试原地修改元素而不是移动它们。

要强制其重新排序元素，你需要用特殊 attribute `key` 来提供一个排序提示：

”给自己看：这里的key 和 `react` 当中的循环便利给个key是一样的（提高效率）“

```
<div v-for="item in items" :key="item.id">
  {{ item.text }}
</div>
```

> 当和 `v-if` 一起使用时，`v-for` 的优先级比 `v-if` 更高。
>
> 注意我们**不**推荐在同一元素上使用 `v-if` 和 `v-for`。

当它们处于同一节点，`v-for` 的优先级比 `v-if` 更高，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中。当你只想为*部分*项渲染节点时，这种优先级的机制会十分有用，如下：

```
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

上面的代码将只渲染未完成的 todo。

而如果你的目的是有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 (或 [`) 上。如：

```
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```



[案例下载](https://wwa.lanzous.com/icGSXhxnbla)

