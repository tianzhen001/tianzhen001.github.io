---
title: webpack配置指南
tags: 
- webpack
- 打包工具
categories: 打包工具
summary: 前端大型项目打包上线必备
abbrlink: dfd4
date: 2020-05-08 22:13:35
---
## webpack概述


> *webpack* 是一个现代 JavaScript 应用程序的*静态模块打包器(module bundler* )

[webpack中文网](https://www.webpackjs.com/)

[webpack官网](https://webpack.js.org/)

## webpack做了什么

+ 语法转换
  + less/sass转换成css
  + ES6转换成ES5
  + typescript转换成js
+ html/css/js代码的压缩与合并（打包）
+ webpack可以在开发期间提供一个开发环境
  + 自动开启浏览器
  + 自动监视文件变化
  + 自动刷新浏览器
+ 项目一般都需要经过webpack打包之后才上线。

## webpack模块说明

webpack会把所有的资源都当成模块

- css
- js
- 图片
- 字体图标

webpack给前端开发提供了模块化的开发环境

+ 对于js文件，webpack中支持AMD、CMD、commonJS、ES6模块化等语法
+ 有了webpack，我们可以在前端代码中使用任意的模块化语法
+ 可以在浏览器中使用nodejs的模块化语法`const $ = require('jquery')`

## webpack基本使用

+ 创建一个文件夹`webpack-demo`
+ 初始化项目 生成`package.json`

```bash
yarn init -y
```

+ 安装webpack的依赖包

```bash
yarn add webpack webpack-cli -D
```

+ 新建文件`src`和`dist`文件夹，，src用于提供源码，，dist用于存放打包后的文件
+ 在src下新建了`index.js`文件，目的：对`src/index.js`文件进行打包

+ 在package.json文件配置了打包的脚本

```js
  "scripts": {
    "build": "webpack --config webpack.config.js"
  }
```


+ 在项目的根目录，创建一个文件`webpack.config.js`
+ 执行打包命令

```js
yarn build
```

## 配置webpack的打包入口

+ 在`webpack.config.js`文件中

```js
// 这是webpack的配置文件 
// webpack是运行在node环境中，webpack可以执行任意的node代码，包括可以使用node中模块。
module.exports = {
  // 默认： ./src/index.js
  entry: './src/app.js'
}
```



## 配置webpack的打包出口

> 配置webpack最终打包的文件的出口

```js
  // 配置webpack打包出口
  output: {
    // path： 打包出口的目录,默认 dist, 必须指定绝对路径
    path: path.join(__dirname, 'lib'),
    // filename: 打包出口的文件名字  默认 main.js
    filename: 'bundle.js'
  }
```

==如果要配置path，记得是绝对路径==

## 配置webpack的打包模式

```js
  // 打包模式  development|production
  // development: 打包不会对进行压缩   打包快
  // production: 打包会对代码进行压缩  上线
  mode: 'development'
```

## 配置html-webpack-plugin插件

> html-webpack-plugin插件能够帮助我们自动在dist中生成一个html文件，并且会自动帮我们引入打包后的文件。

+ 安装html-webpack-plugin插件

```bash
yarn add html-webpack-plugin -D
```

+ 在`webpack.config.js`中配置

```js
//1. 导入html-webpack-plugin插件
const HtmlWebpackPlugin = require('html-webpack-plugin')

// 2.配置webpack的插件，是一个数组
plugins: [new HtmlWebpackPlugin({
  // 生成html的模板
  template: './src/index.html'
})]
```

## 配置css-loader处理css文件

> webpack天生只能处理js文件，如果需要处理其他类型的文件，需要配置loader

+ 安装依赖包

```js
yarn add css-loader style-loader -D
```

+ 配置loader

```js
module: {
  rules: [
    // 配置css-loader的规则
    {
      // 匹配所有.css结尾的文件
      test: /\.css$/,
      // 使用css-loader 和 style-loader处理
      use: ['style-loader', 'css-loader']
    }
  ]
}
```

==注意：loader加载顺序从右往左==

## 配置less-loader处理less文件

+ 安装依赖包

```bash
yarn add less-loader less -D
```

+ 配置less-loader

```js
// 配置less-loader的规则
{
  // 匹配所有.less结尾的文件
  test: /\.less$/,
    // 使用css-loader 和 style-loader处理
    use: ['style-loader', 'css-loader', 'less-loader']
}
```

## 配置file-loader处理图片

+ 安装依赖包

```
yarn add file-loader -D
```

+ 配置file-loader

```js
// file-loader配置
{
  test: /\.(png|jpg|gif)$/,
  use: 'file-loader'
}
```

## 配置url-loader处理图片

+ 安装

```js
yarn add url-loader file-loader -D
```

+ 配置url-loader

```js
{
  test: /\.(png|jpg|gif)$/,
  use: {
    loader: 'url-loader',
    options: {
      limit: 20 * 1024
    }
  }
}
```

## 配置字体图标和音视频

```js
// 字体图标
{
  test: /\.(eot|svg|ttf|woff)$/,
  use: {
    loader: 'url-loader',
    options: {
      limit: 20 * 1024
    }
  }
},
{
  test: /\.(mp3|mp4|ogg)$/,
  use: {
    loader: 'url-loader',
    options: {
      limit: 20 * 1024
    }
  }
}
```

## 配置babel-loader

> babel可以把高版本的js语法转成低版本的js语法，保证运行的效果一样。能够兼容更多的浏览器。

+ 安装依赖包

```js
yarn add  babel-loader @babel/core @babel/preset-env -D
```

+ 配置babel



```js
{
  test: /\.m?js$/,
  exclude: /(node_modules|bower_components)/,
  use: {
    loader: 'babel-loader',
    options: {
      presets: ['@babel/preset-env']
    }
  }
}
```

## 提取css到单独的文件中

+ 安装插件

```js
yarn add mini-css-extract-plugin -D
```

+ 配置插件

```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

// 配置插件
plugins: [
  new MiniCssExtractPlugin({
    // 指定生成的css文件名和路径
    filename: './index.css',
  })
],
```

+ 配置css和less的loader

```js
{
  test: /\.css$/,
  // css-loader只能够让webpack能够处理css文件
  // style-loader： 能够把处理好的css代码添加到页面中
  // MiniCssExtractPlugin.loader ; 把css提取到单独的css文件中
  use: [MiniCssExtractPlugin.loader, 'css-loader']
},
{
  test: /\.less$/,
  use: [MiniCssExtractPlugin.loader, 'css-loader', 'less-loader']
},
```

## webpack-dev-server的使用

> webpack-dev-server不是用来打包的，而是用于启动一个服务器，，，，，当我们代码发生了改变，webpack-dev-server会重新打包（内存）并且会刷新浏览器，实时看到效果
>
> 最新版本的webpack5还不支持，需要降级处理

![image-20201108181347423](images/image-20201108181347423.png)

+ 安装包

```bash
yarn add webpack-dev-server -D

## 注意：如果需要使用webpack-dev-server  就不能使用最新的webpack5版本， 应该使用webpack 4
```

+ 配置一个脚本

```js

  "scripts": {
    "build": "webpack --config webpack.config.js",
    "serve": "webpack-dev-server --config webpack.config.js"
  },
```

+ 使用dev脚本

```
yarn serve
```

+ 常见配置

```js
  // devServer的配置
  devServer: {
    // 自定义端口
    port: 9090,
    // 自动打开浏览器
    open: true
  }
```

## webpack处理vue文件

+ 新建了一个`App.vue`文件

```js
<template>
  <div class="app">我是根组件 ---{{msg}}  --<demo></demo> </div>
</template>

<script>
export default {
  data() {
    return {
      msg: 'hello'
    }
  },
}
</script>

<style>
.app {
  background-color: red;
}
</style>
```

+ 在main.js中导入`App.vue`根组件，并且渲染成为根组件

```js
import Vue from 'vue'
import App from './App.vue'

const vm = new Vue({
  el: '##app',
  // 把App组件渲染成根组件
  render: c => c(App),
  // 把app渲染成为根组件
  // render: function(createElement) {
  //   return createElement(App)
  // }
})
```

+ 报错，因为webpack处理不了vue文件

+ 安装依赖包

```js
yarn add vue-loader@15.9.0 vue-template-compiler -D    
```

+ 在webpack.config.js中配置vue-loader

```js
const VueLoaderPlugin = require('vue-loader/lib/plugin')

  plugins: [
    new VueLoaderPlugin()
  ],
    
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader'
      }
    ]
```

12