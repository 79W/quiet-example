---
title: 'Vue组件化,组件间数据的交互,组件插槽的使用'
categories: 技术笔记
tags:
  - Vue
  - 组件
  - 插槽
excerpt: 'Vue组件化,组件间数据的交互,组件插槽的使用'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg/imgVuezujianhuazz.png'
articleLink: 3dedfeeb478c3602
date: 2020-11-02 09:11:36
---

## 组件基础

通常一个应用会以一棵嵌套的组件树的形式来组织：

![Component Tree](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgcomponents-20201101193746207.png)

例如，你可能会有页头、侧边栏、内容区等组件，每个组件又包含了其它的像导航链接、博文之类的组件。

#### 全局注册

我们使用 `Vue.component( id, [definition\] )` 注册或获取全局组件。

我们需要传递一个名称（也就是你这个组件的名称）我们始终需要给它一个名字

强烈推荐遵循 W3C 规范中的自定义组件名(字母全小写且必须包含一个连字符)

第二个参数为对象

```
Vue.component('my-component-name', { /* ... */ })
```

该组件名就是 `Vue.component` 的第一个参数。

**调用**

```
<div id="app">
  <my-component-name></my-component-name>
  <my-component-name></my-component-name>
  <my-component-name></my-component-name>
</div>
```

如这段代码所示我们可以多次调用并且互不影响

上面我们说了全局注册方法 接下来说局部注册

#### 局部注册

先声明一个方法对象 （也就是全局注册的第二个参数）

```
var ComponentA = { /* ... */ }
```

然后我们注册

```
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
  }
})
```

#### 小案例

我们创建一个组件 一个按钮上面的字进行变化

```
Vue.component('but-cou',{
			data:function(){
				return {
					count:0
				}
			},
			template:'<button @click="handle">点击{{count}}次</button>',
			methods:{
				handle:function(){
					this.count += 1
				}
			}
		})
```

我们引入

```
<div id="app">
		<but-cou></but-cou>
</div>
```

#### `data` 必须是一个函数

从上面的小案例中我们可以看出来 我们第二个参数中 有date 还有 methods

然而我们的 data 必须是一个 函数

你不可以是一个对象

```
data: {
  count: 0
}
```

**一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝：

```
data: function () {
  return {
    count: 0
  }
}
```

## 组件间数据交互

#### 父组件向子组件传值

我们需要通过 Prop 向子组件传递数据 

一个 prop 被注册之后，你就可以像这样把数据作为一个自定义 attribute 传递进来：

```
<blog-post title="My journey with Vue"></blog-post>
```

然后我们子组件可以接收

```
Vue.component('blog-post', {
	// 可以接收多个
  props: ['title'],
  data:function(){
  	return {
  		num:0
  	}
  },
  template: '<h3>{{ title }}</h3>'
})
```

`props`类型：字符串String 数值Number 布尔值Boolean 数组 Array 对象Object

`props`传递原则：单项数据流

#### 子组件向父组件传递值

我们需要在子组件上面去定义一个事件名称

可以通过调用内建的 `$emit` 方法 并传入事件名称来触发一个事件：

```
<button v-on:click="$emit('enlarge-text')">
  Enlarge text
</button>
```

有了这个 `v-on:enlarge-text="postFontSize += 0.1"` 监听器，父级组件就会接收该事件并更新 `postFontSize` 的值。

##### 小案例

我们需要子组件来改变父组件的值

我们先创建一个子组件

```
Vue.component('but-size',{
    // 我们子组件定义一个按钮 并且 这个按钮传递事件名称 add-size
    template:`<button v-on:click="$emit('add-size')">子组件</button>`
})
```

然后我们 父组件进行调用并接收方法名

```
<div id="app">
    <p>{{size}}</p>
    <but-size v-on:add-size="size+=1"></but-size>
</div>
```

```
let vm = new Vue({
			el:"#app",
			data:{
				size:0
			},
		})
```

然后效果就是 子组件可以改变 父组件的 `size` 值

![addsizezujian](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgaddsizezujian.gif)

上面一些列我们发现我们并没有传递数据

其实 我们可以把数据放在 $emit 的第二个参数中

```
Vue.component('but-size',{
    // 我们子组件定义一个按钮 并且 这个按钮传递事件名称 add-size
    template:`<button v-on:click="$emit('add-size',5)">子组件</button>`
})
```

然后我们在父组件中 用$event接收

```
<div id="app">
    <p>{{size}}</p>
    <but-size v-on:add-size="size+=$event"></but-size>
</div>
```

#### 兄弟组件间传递值

我们需要使用事件中心的处理方式进行传递

![事件中心](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101203322416.png)

我们需要有一个 Vue 对象 作为事件中心

```
var eventHub = new Vue()
```

监听事件与销毁事件

```
eventHub.$on('add-todo',addTodo)
eventHub.$off('add-todo',addTodo)
```

触发事件

```
eventHub.$emit('add-todo',id)
```

##### 小案例

首先我们定义 事件中心

```
var zvue = new Vue()
```

然后我们定义第一个组件A

```
// 第一个子组件
		Vue.component('test-a',{
			data:function(){
				return {
					num:0
				}
			},
			template:`
				<div>
				<p>第一个组件：{{num}}</p>
				<button @click="heandel"> 点击第一个组件 </button>
				</div>
			`,
			methods:{
				heandel:function(){
					// 调用B组件的方法
					zvue.$emit('tom-b',10)
				}
			},
			mounted:function(){
				zvue.$on('tom-a',(val)=>{
					this.num += val
				})
			}
		})
```

定义组件B

```
// 第二个子组件
		Vue.component('test-b',{
			data:function(){
				return {
					num:0
				}
			},
			template:`
				<div>
				<p>第二个组件：{{num}}</p>
				<button @click="heandel"> 点击第二个组件 </button>
				</div>
			`,
			methods:{
				heandel:function(){
					// 调用A组件的方法
					zvue.$emit('tom-a',3)
				}
			},
			mounted:function(){
				zvue.$on('tom-b',(val)=>{
					this.num += val
				})
			}
		})
```

讲解：我们在 `methods` 里面创建触发事件

我们在生命周期函数中 `mounted` 监听事件 （实例被挂载后调用）

#### 组件插槽

我们在父组件调用的时候两个标签中没有写内容的

```
<alert-box></alert-box>
```

那我们在中间写数据又是如何接收的呢？

```
<alert-box>
  Something bad happened.
</alert-box>
```

幸好，Vue 自定义的 `<slot>` 元素让这变得非常简单：

```
Vue.component('alert-box', {
  template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
```

最后我们的数据会放在 `<slot>` 标签里面 我们在子组件的  `<slot>`  中自定义一些默认的东西 会被替换掉

如果父组件调用子组件没有传递 会使用你默认写的东西

#### 具名插槽

有时我们需要多个插槽。例如对于一个带有如下模板的 `<base-layout>` 组件

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

我们定义了 三个插槽 有两个是携带名字`name`的  还有一个是不携带名字的

在向具名插槽提供内容的时候，我们可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称：

```
<base-layout>

  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>
  
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>
  
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
  
</base-layout>
```

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。

任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

#### 作用域插槽

`自 2.6.0 起有所更新。已废弃的使用 slot-scope`

有时让插槽内容能够访问子组件中才有的数据是很有用的

父组件对子组件的内容进行加工处理

我们写个案例来演示一下

我们编写根组件

```
let vm = new Vue({
			el:"#app",
			data:{
				list:[
					{
						id:1,
						title:"西瓜",
					},
					{
						id:2,
						title:"香蕉",
					},
					{
						id:3,
						title:"苹果",
					}
				]
			}
		})
```

然后我们写子组件

```
// 创建子组件
		Vue.component('zi-text',{
			// 接收父组件传递过来的数据
			props:['list'],
			// 对接收过来的数据进行处理
			// 重点是 slot 中我以 info为名字将 item传递给了父组件
			template:`
				<div>
					<li v-for="(item,index) in list">
						<slot :info="item">
							{{item.title}}
						</slot>
					</li>
				</div>
			`
		})
```

然后我们在根组件渲染子组件

```
<div id="app">
			<zi-text :list="list">
				<!-- 父组件 slot-scope="slotProps" 来接收数据 -->
				<template slot-scope="slotProps">
					<h1 v-if="slotProps.info.id == 2">
						<!-- 我们可以对数据进行操作了 -->
						{{slotProps.info.title}}
					</h1>
					<h4 v-else>
						{{slotProps.info.title}}
					</h4>
				</template>
			</zi-text>
		</div>
```

#### 新 v-slot用法

在2.6.0中,我们为具名插槽和作用域插槽引入了一个新的统一的语法(即v-slot指令)。

它取代了 slot和slot -scope-这两个目前已被废弃但未被移除且仍在文档中的特性。

##### 插槽一共就三大类

- 匿名插槽(也叫默认插槽):没有命名,有且只有一个
- 具名插槽:相对匿名插槽组件slot标签带name命名的
- 作用域插槽:子组件内数据可以被父页面拿到(解决了数据只能从父页面传递给子组件)

##### 作用域插槽

重点是slotProps接取子组件里:user="user" :test="test"类似属性的数据

```
// 父组件
<todo-list>
  <template v-slot:todo="slotProps" >
      {{slotProps.user.firstName}}
  </template> 
</todo-list> 
//slotProps 可以随意命名
//slotProps 接取的是子组件标签slot上属性数据的集合所有v-bind:user="user"
```

```
//子组件
 <slot name="todo" :user="user" :test="test">
        {{ user.lastName }}
  </slot> 
data() {
        return {
            user:{
                lastName:"Zhang",
                firstName:"yue"
            },
            test:[1,2,3,4]
        }
    },
// {{ user.lastName }}是默认数据   v-slot:todo 当父页面没有(="slotProps")
// 时显示 Zhang


## 显示 ##
// yue
```

总：

就是子组件将数据渲染后我们可以将整个元素进行传递到 父组件 然后父组件也可以进行数据的处理

在用上v-slot之后 只需要考虑好

- 是否需要命名(匿名插槽,具名插槽)
- 父页面是否需要取存在子页面的数据(作用域插槽)

[购物车案例](https://wwa.lanzous.com/irwMKhz8zyj)

