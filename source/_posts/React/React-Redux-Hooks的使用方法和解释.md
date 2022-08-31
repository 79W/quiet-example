---
title: React-Redux-Hooks的使用方法和解释
categories: 技术笔记
tags:
  - React
  - Redux
  - Hooks
excerpt: React-Redux-Hooks的使用方法和解释
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201025175107.png'
articleLink: 31aef27c40642426
date: 2020-10-26 11:14:24
---

## Hooks

在`"react-redux": "^7.1.0",` 以后的版本新增了Hooks 这样可以更加简化我们的 `connect`

我们首先定义一个 `reducer` 里面有一个数据 `nub` 然后有一个方法是进行添加的方法

```
function reducer(state = {
    nub: 1
}, action) {
    switch (action.type) {
        case "ADD":
            let { nub } = state;
            nub++;
            return {
                nub
            }
    }
    return state;
}
```

总共有三那个Hooks

我们 先进行引入

```
import { useDispatch, useStore, useSelector } from "react-redux";
```

### useSelector(fn)

接收一个 函数方法 然后返回一个数据我们看下

```
const { nub } = useSelector((state) => {
    console.log(state)
    return state;
});
```

`state` 就是我们在 `reducer` 内定义

![Hooks](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026112333707.png)

通俗的说就是获取 `reducer` 的变量 

这里的 `return` 我们其实是可以返回任何数据 我么也可以 先对 `state` 数据进行处理然后我们在 返回也是可以的

### useDispatch()

看名字我们就猜到差不多了 这是获取`dispatch` 方法

我们打印下看看

```
const dispatch = useDispatch();
console.log(dispatch)
```

![dispatch](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026112743996.png)

我们可以直接调用

```
dispatch({
	type: "ADD"
})
```

```
<button onClick={() => {
  dispatch({
  		type: "ADD"
  })
}}>增加</button>
```

### useStore()

这个也很好理解就是获取我们的`Store` 对象

```
console.log(useStore());
```

![image-20201026113050466](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026113050466.png)

