---
title: AntDesign-React使用和样式的按需引入的高级配置
categories: 技术笔记
tags:
  - React
  - AntDesign
excerpt: AntDesign-React使用和样式的按需引入的高级配置
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201026171351.png'
articleLink: 7ec10d1c56cd5526
date: 2020-10-26 15:57:55
---

### AntDesign

`antd` 是基于 Ant Design 设计体系的 React UI 组件库，主要用于研发企业级中后台产品。

#### ✨ 特性

- 🌈 提炼自企业级中后台产品的交互语言和视觉风格。
- 📦 开箱即用的高质量 React 组件。
- 🛡 使用 TypeScript 开发，提供完整的类型定义文件。
- ⚙️ 全链路开发和设计工具体系。
- 🌍 数十个国际化语言支持。
- 🎨 深入每个细节的主题定制能力。

### 构建项目

这是 create-react-app 生成的默认目录结构

```
├── README.md
├── package.json
├── public
│   ├── favicon.ico
│   └── index.html
├── src
│   ├── App.css
│   ├── App.js
│   ├── App.test.js
│   ├── index.css
│   ├── index.js
│   └── logo.svg
└── yarn.lock
```

### 快速使用

现在从 yarn 或 npm 安装并引入 antd。

```
$ yarn add antd
```

```
$ npm i antd
```

修改 `src/App.js`，引入 antd 的按钮组件。

```
import React from 'react';
import { Button } from 'antd';
import './App.css';

const App = () => (
  <div className="App">
    <Button type="primary">Button</Button>
  </div>
);

export default App;
```

修改 `src/App.css`，在文件顶部引入 `antd/dist/antd.css`。

```
@import '~antd/dist/antd.css';
```

好了，现在你应该能看到页面上已经有了 antd 的蓝色按钮组件

### 高级配置

这个例子在实际开发中还有一些优化的空间，比如无法进行主题配置。

也就是按需要导入 你的项目里面就使用了一个 按钮 那么你只需要按钮的样式就可以了 其他的样式你不需要

react-app-rewired (一个对 create-react-app 进行自定义配置的社区解决方案)

```
npm i react-app-rewired customize-cra
```

这是安装了两个 插件

安装完成后我们修改 根目录的 `package.json` 文件的 `scripts` 

```
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
  },
```

改成这个样子后我们继续安装 `babel-plugin-import`

```
npm i babel-plugin-import
```

安装好后我们在根目录下创建一个 `config-overrides.js` 文件   (用来修改 默认的配置)

```
const { override, fixBabelImports } = require('customize-cra');
module.exports = override(
    fixBabelImports('import', {
        libraryName: 'antd',
        libraryDirectory: 'es',
        style: 'css'
    }),
);
```

然后我们删掉 App 当中引入的css文件 在看看是否能够显示正确的样式

> 如果这样配置不上 请看上一篇文章的 完整项目搭建 里面写了 antd 按需导入的配置

### 错误

我们会发现控制台当中有一个错误 

```
Warning: findDOMNode is deprecated in StrictMode. findDOMNode was passed an
instance of Wave which is inside StrictMode. Instead, add a ref directly to the 
element you want to reference. Learn more about using refs safely here:
```

![image-20201026162858008](https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/imgimage-20201026162858008.png)

是因为 react 中的严格模式: `StrictMode`

在`index.js`中挂载 App 的外面有这样一个标签 只要把这个标签删除掉就可以了

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

修改成

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);   
```