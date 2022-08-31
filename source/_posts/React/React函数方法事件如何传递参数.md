---
title: React函数方法事件如何传递参数
categories: 技术笔记
tags:
  - React
excerpt: React函数方法事件如何传递参数
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg/imgokreactdianjiclick104154.png'
articleLink: 11280d7a
date: 2020-11-15 10:35:23
---

在React事件中传递参数

我们定义个按钮 并绑定上单击事件

```
 <button onClick={this.delPing()}>删除</button>
```

我们定义好这个方法

```
delPing = (id) => {
	// 我们需要接收到 id
}
```

我们如果直接传递的话

会自动执行这个点击事件

```
onClick={this.delPing(1)}
```

这是因为this的问题我们只需要 给它箭头函数即可

```
<button onClick={() => this.delPing(1)}>删除</button>
```

我们也可以不写方法

```
<button onClick={() => {
	return (id)=>{
			//这里写方法 也是可以的
	}
}
}}>删除</button>
```

