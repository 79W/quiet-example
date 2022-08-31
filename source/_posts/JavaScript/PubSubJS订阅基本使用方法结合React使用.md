---
title: PubSubJS订阅基本使用方法结合React使用
categories: 技术笔记
tags:
  - PubSubJS
  - React
excerpt: PubSubJS订阅基本使用方法结合React使用
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgwoyasuodfeimglocalerrides.png'
articleLink: 9d3672fc
date: 2020-11-14 18:16:52
---

## PubSubJS

是一款使用JavaScript编写的 具有发布和订阅的功能的库

我们可以通过npm安装

```
npm install pubsub-js
```

### 快速使用

```
// 也就是我们进行导入 可能规则不同 导入的方式也会不同
import PubSub from 'pubsub-js'
```

#### 发布

分别为同步和异步形式进行发布分别为`publish`,`publish`

```
Pubsub.publish(StationStatisticsID, data)
```

`StationStatisticsID`:发布订阅的名称这个可以自己进行定义

`data`:发布订阅传递的参数

#### 订阅

```
pubsub = Pubsub.subscribe(pubsubID, (msg, data) => {
		console.log(msg) // 这里将会输出对应设置的 pubsubID
		console.log(data) // 这里将会输出对应设置的参数
	})
```

`pubsubID`:订阅的名称

`msg`:和`pubsubID`是一个（相等的值）

`data` :接收到发布的值

#### 取消订阅

*取消指定的订阅*

```
Pubsub.unsubscribe(pubsub)
```

`pubsub`:你需要取消订阅的名称

*取消全部订阅*

```
PubSub.clearAllSubscriptions()
```

### React中使用

导入库

```
import PubSub from 'pubsub-js'
```

我们定义一个组件 

```
class App extends Component {
	// 定义个方法
  MYonClick = () => {
  	// 发布订阅
    PubSub.publish('Mydate', { name: '乔越博客' })
  }
  render() {
    return (
      <div>
      	// 我们定义个 按钮然后我们 调用上面的方法
        <button onClick={this.MYonClick}>点我</button>
      </div>
    )
  }
}
```

然后我们第一个子组件进行接收调用

```
class Ceshi Zi Component {
		// 这里放个声明周期方法
    componentDidMount = () => {
    		// 我们这里可以接收这个返回值
    		// 接收订阅传递过来的数据 
    		// 我们需要 接收指定的 订阅名称
        this.pubsub_token = Pubsub.subscribe('Mydate', (msg, data) => {
            console.log(msg) // 这里将会输出对应设置的 pubsubID
            console.log(data) // 这里将会输出对应设置的参数
        })
    }
    render() {
        return (
            <div>
                我是子组件
            </div>
        )
    }
}
```

![PubSubJS](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgfgabudongtai.gif)