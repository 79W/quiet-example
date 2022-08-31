---
title: TypeScript环境搭建基本配置项
categories: 技术笔记
tags:
  - TypeScript
excerpt: TypeScript环境搭建基本配置项的简介和基本的用法说明
cover: 'https://cdn.jsdelivr.net/gh/duogongneng/MyBlogImg@master/img20201106114813.png'
articleLink: d1b1d53e
date: 2020-11-06 09:37:33
---

### 环境搭建

`TypeScript` 编写的程序并不能直接通过浏览器运行，我们需要先通过 `TypeScript` 编译器把 `TypeScript` 代码编译成 `JavaScript` 代码

`TypeScript` 的编译器是基于 `Node.js` 的，所以我们需要先安装 `Node.js`

#### 安装node

我们可以前往[node](https://nodejs.org/)官方下载对应的版本然后安装即可

安装完成后我们可以运行以下命令来查看是否安装成功

```
# 查看当前 node 版本
node -v
```

#### 安装`TypeScript` 编译器

通过 `NPM` 包管理工具安装 `TypeScript` 编译器

```
npm i -g typescript
```

安装完成以后，我们可以通过命令 `tsc` 来调用编译器

```shell
# 查看当前 tsc 编译器版本
tsc -v
```

### 代码编写

`vsCode` 和 `TypeScript`都是微软的产品，`vsCode` 本身就是基于 `TypeScript` 进行开发的，`vsCode` 对 `TypeScript` 有着天然友好的支持

默认情况下，`TypeScript` 的文件的后缀为 `.ts`

`TypeScript` 代码

```
let src: string = "qiaoBlog"
```

### 编译执行

我们编写的`ts`文件是不可以直接运行的需要编译成`js`文件

我们编写完后可以使用我们安装好的`tsc`进行编译

```
tsc 路经/文件名.ts
```

然后会生成同名的`js`文件

### 编译选项

编译命令 `tsc` 还支持许多编译选项，这里我先来了解几个比较常用的

#### --outDir

指定编译文件输出目录

```
tsc --outDir 输出目录 输入目录/文件名.ts
```

我们上面简单的编译是将编译好的`ts`文件放在了同目录下 

我们可以用这个命令选项进行设置编译好的文件放在哪里

#### --target

指定编译的代码版本目标，默认为 `ES3`

```
tsc --outDir ./dist --target ES6 ./src/helloKaiKeBa.ts
```

#### --watch

在监听模式下运行，当文件发生改变的时候自动编译

```
tsc --outDir ./dist --target ES6 --watch ./src/helloKaiKeBa.ts
```

通过上面的配置我们不可能每次都去配置所有`typescript`提供了配置文件

### 编译配置文件

我们可以把编译的一些选项保存在一个指定的 `json` 文件中，默认情况下 `tsc` 命令运行的时候会自动去加载运行命令所在的目录下的 `tsconfig.json` 文件，配置文件格式如下

```
{
    "compilerOptions": {
        //输出文件
        "outDir": "./dist",
        // 编译模式
        "target": "ES2015",
        // 是否自动监听
        "watch": true,
    },
    // ** : 所有目录（包括子目录）
    // * : 所有文件，也可以指定类型 *.ts
    "include": [
        "./src/**/*"
    ]
}
```

有了单独的配置文件，我们就可以直接运行

```
tsc
```

#### 指定加载的配置文件

使用 `--project` 或 `-p` 指定配置文件目录，会默认加载该目录下的 `tsconfig.json` 文件

```shell
tsc -p ./configs
```

也可以指定某个具体的配置文件

```
tsc -p ./configs/ts.json
```

