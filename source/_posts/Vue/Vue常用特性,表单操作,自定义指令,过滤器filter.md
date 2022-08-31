---
title: 'Vue常用特性,表单操作,自定义指令,过滤器filter'
categories: 技术笔记
tags:
  - Vue
  - 表单操作
  - 指令
  - filter
excerpt: 'Vue常用特性,表单操作,自定义指令,过滤器filter'
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgchangyongtexing.png'
articleLink: 143dc32673183601
date: 2020-11-01 13:04:36
---

## 表单操作

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。

它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

>`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。

#### 文本

```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```

#### 多行文本

```
<p style="white-space: pre-line;">{{ message }}</p>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```

> 在文本区域插值 (`<textarea>{{text}}</textarea>`) 并不会生效，应用 `v-model` 来代替。

#### 复选框

单个复选框，绑定到布尔值：

```
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

多个复选框，绑定到同一个数组：

```
<input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>
<input type="checkbox" id="john" value="John" v-model="checkedNames">
<label for="john">John</label>
<input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
<br>
<span>Checked names: {{ checkedNames }}</span>
```

```
new Vue({
  el: '...',
  data: {
    checkedNames: []
  }
})
```

![image-20201101152956728](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101152956728.png)

#### 单选按钮

```
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```

```
new Vue({
  el: '#example-4',
  data: {
    picked: 'One'
  }
})
```

![image-20201101153037452](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101153037452.png)

#### 选择框

单选时：

```
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

```
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

![image-20201101153204205](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101153204205.png)

多选时 (绑定到一个数组)：

```
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```

```
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
```

### 修饰符

#### .lazy

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 。

你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步。

也就是说只有在失去焦点的时候 才会执行去同步

```
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

#### .number

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```
<input v-model.number="age" type="number">
```

#### .trim

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```
<input v-model.trim="msg">
```

## 自定义指令

我们在全局注册一个自定义的指令

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})
```

如果你想在局部定义一个 可以在组件中进行编写

组件中也接受一个 `directives` 的选项：

```
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

然后你可以在模板中任何元素上使用新的 `v-focus` property，如下：

```
<input v-focus>
```

#### inserted

被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

#### 参数

`el`：指令所绑定的元素，可以用来直接操作 DOM。

`binding`：一个对象，包含名称 参数等

![binding](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101160304827.png)

### computed 

计算属性的结果会被缓存，除非依赖的响应式 property 变化才会重新计算。

注意，如果某个依赖 (比如非响应式 property) 在该实例范畴之外，则计算属性是**不会**被更新的。

### watch

一个对象，键是需要观察的表达式，值是对应回调函数。

值也可以是方法名，或者包含选项的对象。

Vue 实例将会在实例化时调用 `$watch()`，遍历 watch 对象的每一个 property。

![image-20201101162505584](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101162505584.png)



简单的说就是监听一个数据的变化然后做一些事情

### 我们用 computed 和 watch 分别写下面的案例

需求：我们需要 当我们单独输入姓名或者名的时候 拼接成一个名字显示在下面

![image-20201101162827881](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201101162827881.png)

先使用我们的 `computed` 实现一下

```
<div id="app">
  <input type="text" v-model="xing" placeholder="姓氏" /> <br/>
  <input type="text" v-model="ming" placeholder="名" />
  <p>名字：{{fullname}}</p>
</div>
```

然后我们定义组件

```
let vm = new Vue({
			el:'#app',
			data:{
				xing:'',
				ming:''
			},
			computed:{
				fullname:function(){
					return this.xing + ' ' + this.ming
				}
			},
		})
```

我们在用 `watch` 实现一下

```
<div id="app">
    <input type="text" v-model="xing" placeholder="姓氏" /> <br/>
    <input type="text" v-model="ming" placeholder="名" />
    <p>名字：{{xingming}}</p>
</div>
```

```
		let vm = new Vue({
			el:'#app',
			data:{
				xing:'',
				ming:'',
				xingming:''
			},
			watch:{
				// 监听 xing 这个变量的改变然后触发
				xing:function(val){
					this.xingming = val + ' ' + this.ming
				},
				ming:function(val){
					this.xingming = this.xing + ' ' + val
				}
			}
		})
```

从这个案例上来看 用我们的 `computed` 会更加简洁

而数据监听用的最多的场景为 数据变化时候执行异步或开销比较大的时候进行使用

watch 案例可以用在 用户检验是否注册了

## 过滤器filter

自定义过滤器

```
Vue.filter('mingzi',function(val){})
```

使用

```
<!-- 在双花括号中 -->
{{ message | mingzi }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | mingzi"></div>
```

你可以在一个组件的选项中定义本地的过滤器：

```

filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

过滤器可以串联：

```
{{ message | filterA | filterB }}
```

在这个例子中，`filterA` 被定义为接收单个参数的过滤器函数，表达式 `message` 的值将作为参数传入到函数中。然后继续调用同样被定义为接收单个参数的过滤器函数 `filterB`，将 `filterA` 的结果传递到 `filterB` 中。 

### 携带参数

接收参数是从第二个参数开始的

```
{{msg | mingzi('这是数据')}}
```

上面我们传递了一个参数

```
Vue.filter('mingzi',function(val,text){
		return val + text
})
```

在这里我们需要定义两个行参去接受

val：我们接收的是 msg 的值

text：才是接收我们传递的 "这是数据" 的值 

## 生命周期

![Vue 实例生命周期](https://cn.vuejs.org/images/lifecycle.png)