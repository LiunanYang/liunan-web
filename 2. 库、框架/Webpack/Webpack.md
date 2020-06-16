
<!-- TOC -->

- [1. 入口 entry](#1-入口-entry)
    - [1.1. 单个入口](#11-单个入口)
- [2. 输出 output](#2-输出-output)
    - [2.1. 多个入口起点](#21-多个入口起点)
- [3. loader](#3-loader)
    - [3.1. loader 特性](#31-loader-特性)
    - [3.2. 使用 loader 的方法](#32-使用-loader-的方法)
        - [3.2.1. 配置[Configuration]](#321-配置configuration)
        - [3.2.2. 内联](#322-内联)
        - [3.2.3. CLI命令行](#323-cli命令行)
- [4. 插件 plugins](#4-插件-plugins)

<!-- /TOC -->
# 1. 入口 entry
在 webpack 中配置 `entry`属性，来指定一个入口起点，或者多个入口起点。

## 1.1. 单个入口
```javascript
module.exports={
  entry: {
    app: './src/main.js'
  }
}
```
简写:
```javascript
module.exports={
  entry: './src/main.js'
}
```
# 2. 输出 output
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。

即使可以存在多个入口起点，但只指定一个输出配置。

- filename 用于输出文件的文件名。
- path 目标输出目录，绝对路径。
```javascript
// 将一个单独的 bundle.js 文件输出到 /home/proj/public/assets 目录中。
module.exports = {
    output: {
        filename: 'bundle.js',
        path: '/home/proj/public/assets'
    }
}
```

## 2.1. 多个入口起点
使用占位符确保每个文件具有唯一的名称，`[name]` 是占位符，指的是 entry 的 key 值
```
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}
```

# 3. loader
webpack 自身只理解JavaScript，loader 把所有类型的文件转换成 webpack 能够处理的有效模块。

## 3.1. loader 特性
- loader 支持链式传递。能够对资源使用流水线(pipeline)。一组链式的 loader 将按照相反的顺序执行。loader 链中的第一个 - loader 返回值给下一个 loader。在最后一个 loader，返回 webpack 所预期的 JavaScript。
- loader 可以是同步的，也可以是异步的。
- loader 运行在 Node.js 中，并且能够执行任何可能的操作。
- loader 接收查询参数。用于对 loader 传递配置。
- loader 也能够使用 options 对象进行配置。
- 除了使用 package.json 常见的 main 属性，还可以将普通的 npm 模块导出为 loader，做法是在 package.json 里定义一个 loader 字段。
- 插件(plugin)可以为 loader 带来更多特性。
- loader 能够产生额外的任意文件。

## 3.2. 使用 loader 的方法
### 3.2.1. 配置[Configuration]
例如，可以使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JavaScript。首先：安装相应的`loader`
```javascript
npm install --save-dev css-loader
npm install --save-dev ts-loader
```
然后：进行配置
``` javascript
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
}
```
### 3.2.2. 内联
```javascript
import Styles from 'style-loader!css-loader?modules!./styles.css';
```
- 使用 ! 将资源中的 loader 分开
- 使用 ? 传递查询参数

### 3.2.3. CLI命令行
例如，对 .jade 文件使用 jade-loader，对 .css 文件使用 style-loader 和 css-loader
```
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

# 4. 插件 plugins
插件目的在于解决 loader 无法实现的其他事。