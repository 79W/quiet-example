---
title: ReactNative路由导航与Tabbar基本设置
categories: 技术笔记
tags:
  - ReactNative
  - Tabbar
  - Router
excerpt: ReactNative路由导航与Tabbar基本设置
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/20201215102642.png'
articleLink: 07c19982
date: 2020-12-15 09:41:40
---

**react-navigation结合react-native-tab-navigator进行路由跳转**

在写ReactNative混合App开发的时候 顶部头和底部 Tabbar 基本上是必须的

### react-navigation

我们先配置 `react-navigation` 也就是路由的一些操作

如果你是崭新的一个项目需要安装很多的依赖才可以使用

首先我们需要安装 `react-navigation`

```
npm install @react-navigation/native
```

下面这些依赖 才能正常的去使用

```
npm install react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

当我们都安装好了后  我们就可以创建一个 放置路由的 文件了 我们叫他 `router` 里面创建个 `index.js`

我们需要在安装个 配置路由堆的依赖

```
npm install @react-navigation/stack
```

这回我们可以真正的去配置路由了

```
import * as React from 'react';
// 导入包
import { NavigationContainer } from '@react-navigation/native';
// 创建路由表使用
import { createStackNavigator } from '@react-navigation/stack';
// 必须
const Stack = createStackNavigator();
function Router() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
     		// 在这里我们写上路由 name 也就相当于我们路由路径 HomeScreen 是需要我们导入进来的
        <Stack.Screen name="Home" component={HomeScreen} />
        // 这里可以写多个 当然放在最上面的会第一个显示
      </Stack.Navigator>
    </NavigationContainer>
  );
}
export default Router;
```

我们配置好路由表后我们就可以在 ` app.js` 中进行引入使用了

![react-navigation](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg/image-20201215094750337.png)

这时候 你就会发现我们的顶部导航就可以显示了 

接下来我们来配置底部`Tabbar`

### react-native-tab-navigator

首先我们还是安装 依赖

```
npm i @hanzc/react-native-tab-navigator
```

然后我们创建个文件 就叫他 `Tabbar` 

然后我们导入 依赖

```
import TabNavigator from 'react-native-tab-navigator';
```

我们来写个 组件的基本格式

```
import React from 'react';
import {View, Text, Image} from 'react-native';
import TabNavigator from 'react-native-tab-navigator';
class Tabbar extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <View style={{flex: 1}}>
					// 这里放Tabbar
      </View>
    );
  }
}
export default Tabbar;

```

然后我们需要 参考官方文档给的格式

```
<TabNavigator>
  <TabNavigator.Item
    selected={this.state.selectedTab === 'home'}
    title="Home"
    renderIcon={() => <Image source={...} />}
    renderSelectedIcon={() => <Image source={...} />}
    badgeText="1"
    onPress={() => this.setState({ selectedTab: 'home' })}>
    {homeView}
  </TabNavigator.Item>
  <TabNavigator.Item
    selected={this.state.selectedTab === 'profile'}
    title="Profile"
    renderIcon={() => <Image source={...} />}
    renderSelectedIcon={() => <Image source={...} />}
    onPress={() => this.setState({ selectedTab: 'profile' })}>
    {profileView}
  </TabNavigator.Item>
</TabNavigator>
```

- renderIcon ：默认显示图标
- renderSelectedIcon ：选中后展示的图标
- badgeText ：图表上提示的数字

homeView -- homeView 是放我们组件的地方

然后我们在路由表中进行引入这个 `tabbar `的组件并放在最上方

```
<NavigationContainer>
  <Stack.Navigator>
    {/* 导入Tabbar 底部导航来进行 */}
    <Stack.Screen name="首页" component={Tabbar} />
    {/* 下面放其他路由组件 */}
  </Stack.Navigator>
</NavigationContainer>
```

然后我们会看到配置成功 并且`tabbar` 页面可以正常的跳转

![react-native-tab-navigator](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg/image-20201215094758227.png)

### 跳转路由

但是当我们想从`tabbar` 页面跳转到不是 `tabbar` 页面的时候 

在 `react-navigation` 的官方文档 个人理解的意思

当我们在 在`Stack` 中写入 的路由当跳转到这个页面的时候会默认传入 `navigation` 对象 进行操作路由跳转

![image-20201215101306162](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg/image-20201215101306162.png)

但是我们第一个写的是 `Tbabar`组件并非 `Home`组件 

这时候就需要我们手动的将 的进行传递

我们需要在 `Tabbar`组件中进行传递 也就是我们 在 `homeView` 哪个地方写组件的时候就进行传递

```
<Home navigation={this.props.navigation} />
```

这时候我们的`Home`组件就可以接收到 `navigation` 对象了 就可以操作路由跳转了

```
this.props.navigation.navigate('TeamDetails', {
   teamId: teamId,
});
```

`TeamDetails` : 为路由名称

第二个参数为 携带数据 然后我们的项目的基本路由配置就完成了

![navigation](https://cdn.jsdelivr.net/gh/duogongneng/OneMyBlogImg@master/team-min.gif)