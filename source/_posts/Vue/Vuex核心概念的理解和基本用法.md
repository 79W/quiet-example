---
title: Vuex核心概念的理解和基本用法
categories: 技术笔记
tags:
  - Vue
excerpt: Vuex核心概念的理解和基本用法
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201105153540.png'
articleLink: 4e6a7035e5ae3605
date: 2020-11-05 11:38:36
---

## Vuex

实现组件全局状态（数据）管理的一种机制,可以方便的实现组件之间的数据共享。

我们之前的数据传递的方式

- 父向子传递数据： `v-bind`属性绑定
- 子向父传递数据：`v-on`事件绑定
- 兄弟之间传递数据：`$on`接收数据 `$emit`发送数据

![Vuex传递数据](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimgimage-20201105141437241.png)

如果紫色小球想传递数据需要走很多路经（这样就会影响很多的组件）才能到达

而现在我们有了`Vuex`可以直接进行传递数据

![Vuex传递数据](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201105141622192.png)

我们的紫色小球想分享数据 可以直接把数据传递到 `store`里面 然后谁想使用可以直接从`store`里面进行获取即可

### Vuex的好处

❤️能够在vuex中集中管理共享的数据,易于开发和后期的维护

🧡能够高效的实现组件间的数据共从而提高开发效率

💛存储在vuex中的数据都是响应的能够实时保持数据页面的共享同步

### 快速使用

我们需要安装Vuex的包

```
npx i vuex
```

然后我们需要导入到项目中

```
import Vuex from 'vuex'
// 注册
Vue.use(Vuex)
```

我们创建一个`store`对象

```
const store = new Vuex.Store({
		//state中存放的就是全局数据
		state:{
			//我们定义一个数据
			num:0
		}
})
```

我们将`store`对象挂载在Vue实例上面

```
new Vue({
	el:'#app',
	// 将创建的共享数据对象，挂载到Vue实例中
	// 所有的组件就可以直接 store 中获取全局的数据
	store
})
```

### 核心概念

#### State

提供唯一的公共数据源,所有的共享数据都要统一放在Store的State中进行存储

```
//创建Store数据源 提供唯一公共数据
const store = new Vuex.Store({
		state:{ count: 0 }
})
```

我们如何在组件中进行访问呢

我们可以直接用this进行访问（第一种方式）

```
this.$store.state.count
```

我们也可以导入 `mapState`函数

```
import { mapState } = from 'vuex'
```

我们可以通过刚才导入的`mapState`函数 将当前组件需要的全局数据映射为当前组件的 `computed`计算属性

```
computed:{
	...mapState(['count'])
}
```

然后我们就可以在组件中直接使用 `count`变量了

#### Mutation

用于变更Store中的数据

- 只能通过`mutation`进行数据的更改 不可以直接操作 `store` 中的数据
- 通过这种方式虽然会变的稍微复杂一些但是可以集中监控所有数据的变化

首先我们定义一个 `mutation`

```
const store = new Vuex.Store({
		state:{
			count:0
		},
		mutations:{
				// 里面我们定义方法
				// 比如我们给 count 加一的方法
				add(state){
						state.count++
				}
		}
})
```

我么怎么在组件中触发这个加一的方法呢

我们可以直接在自己组件中的事件中调用

```
methods:{
		handle(){
				//触发 mutations
				this.$store.commit('add')
		}
}
```

我们还有别的触发方式吗？当然有

我们可以导入`mapMutations`函数

```
import { mapMutations } from 'vuex'
```

通过导入的函数进行映射为组件的 `methods`方法

```
methods:{
		// 在数组中可以传递多个方法
		...mapMutations(['add','del'])
}
```

我们也是可以传递参数的

##### 传递参数

我们需要在调用的时候传递过去

我们需要在`mutations`中进行接收

```
mutations:{
		// 第一个参数为state
		// 第二个参数为传递的参数
		addN(state,step){
				state.count += step
		}
}
```

第一种方式(我们直接把参数放在第二个参数即可)

```
this.$store.commit('addN',5)
```

第二种方式

```
// 在组件中导入 mapMutations 方法
methods:{
	...mapMutations(['addN']),
	//然后我么就可以直接进行调用了
	// 本质上就是映射为 本组件自己的 方法了
	btnClick(){
		// 我们调用方法 然后传递参数 为10
		this.addN(10)
	}
}
```

#### Action

**我们不能在 mutations函数中去执行异步的操作**

我们需要将异步的操作放在`action`中来处理

你想通过异步的操作修改数据你就必须使用`action`不可使用`mutations`

但是`Action`中还是要通过触发`mutations`来间接的更改数据

```
// 定义action
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
    	// 这里可以写异步处理逻辑
      context.commit('increment')
    }
  }
})
```

触发（我们可以在组件中直接进行调用）

```
this.$store.dispatch('increment')
```

乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？

我们可以在 action 内部执行**异步**操作：

```
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

我么可以导入 `mapActions`函数进行调用

```
import { mapActions } from 'vuex'
```

然后我们可以直接映射为本组件的 `methods`函数

```
methods:{
		...mapActions(['incrementAsync'])
}
```

我们可以传递参数 方式和 `mutations`一样

#### Getter

用于对Store中的数据进行加工处理形成新的数据

- 可以对Store中已经存在的数据进行加工后形成新的数据然后返回，类似计算属性
- Store中的数据发生变化时候 ，Getter会跟着变化

```
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
    	// 这里返回的是处理好的数据
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

我们可以通过this直接在组件中进行访问

```
this.$store.getters.doneTodos
```

我们还可以通过导入`mapGetters`函数进行访问

```
import { mapGetters } from 'vuex'

computed:{
	// 使用对象展开运算符将 getter 混入 computed 对象中
	...mapGetters(['doneTodos'])
}
```

