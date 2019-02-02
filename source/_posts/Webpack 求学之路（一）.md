---
title: Webpack 求学之路（一）初见
tags:
  - HTTP
date: 2018-07-05
---

### 一、JS 、CSS 模块化的前世今生
想要学 webpack ，我们应该先了解 webpack 解决了什么问题。

webpack 主要解决模块的打包和依赖问题，这是我的拙见。看一下 JS 模块化的发展：

- 命名空间

- CommonJS
> 一个文件为一个模块
> 通过module.exports暴露模块接口
> 通过require引入模块
> 同步执行
<!-- more -->
- AMD
> Async Module Definition 异步模块定义
> 试用 define 定义模块
> 通过require引入模块
> RequireJS 库
> 依赖前置，提前执行

- CMD
> Common Module Definition 通用模块定义
> 一个文件为一个模块
> 使用 define 来定义一个模块
> 使用 require 来加载一个模块
> SeaJS
> 尽可能懒执行

- UMD
> Universal Module Definition 通用模块定义
> 三个步骤：判断是否支持AMD、判断是否支持 CommonJS、如果都不支持，使用全局变量

- ES Module
> 一个文件为一个模块
> export / import

CSS 其实也是有模块化的：

常见的有 BEM（Block/Element/Modifier），尴尬，就了解一个。

### 二、 核心概念
- Entry
> 代码的入口、打包的入口、单个或多个入口

- Output
> 打包成的文件（bundle）、一个或多个、自定义规则、配合CDN  

- Loaders
> 处理文件、转化为模块、
> 编译相关：babel-loader、ts-loader
> 样式相关：style-loader、css-loader、less-loader、postcss-loader
> 文件相关：file-loader、url-loader

- Plugins
> 参与整个打包过程
> 打包优化和压缩
> 配置编译时的变量
> 优化相关：UglifyJsPlugin、CommonsChunkPlugin
> 功能相关：ExtractTextWebpackPlugin、HtmlWebpackPlugin、HotModuleReplacementPlugin、CopyWebpackPlugin

```js
const webpack = require('webpack')
module.exports = {
    entry: {
        index: ['index.js','app.js'],
        vendor: 'vendor.js'
    },
    output: {
        filename: '[name].min.[hash:5].js'//name对应上面 entry 对象的每个key
    },
    rules: [
        {
            test: /\.css$/,
            use: 'css-loader'
        }
    ],
    plugins: [
        new webpack.optimize.UglifyJsPlugin()//引用插件来混淆压缩代码
    ]
}
```

### 三、各种方式打包模块
webpack打包可以直接在命令行里面通过 
> webpack '要打包的文件' 

或者，通过配置 webpack.config.js 来打包，分别来看一下

首先创建入口文件 app.js

```js
//esModule 语法
import sum from './sum.js' 
sum(4, 5);

// CommonJS语法
let sum = require('./sum')
sum(4, 5);

// Amd 语法
require(['./sum'],function(sum){
  sum(4, 5)
})
```

在创建 sum.js

```js
// esModule 语法
export default function(a, b) { 
  console.log(`${a}+${b}=${a+b}`)
}

// CommonJS 语法
module.exports = function(a, b){
  console.log(`${a}+${b}=${a+b}`)
}

// Amd 语法
define(function(require, factory) {
  'use strict';
  return function(a, b){
    console.log(`${a}+${b}=${a+b}`)
  }
});
```

或者直接配置 webpakc.config.js 文件，必须使用 CommonJS 规范

```js
module.exports = {
    entry: {
        app: 'app.js',
    }
    output: {
        filename: '[name].min.[hash:5].js'//name对应上面 entry 对象的每个key
}
```

然后终端运行 webpack，就可以打包了，如果配置文件的名称不是 webpack.config.js ，那么终端对应的命令为：
> webpack --config webpack.conf.js


### 四、编译打包 ES6
在 webpack 中可以借助 loader 来编译 ES6 的代码

```js
```



 


