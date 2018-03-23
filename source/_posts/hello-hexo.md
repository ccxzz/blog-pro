---
title: hello hexo
date: 2018-03-22 14:25:06
tags:
---
###相关资源
***
+ 中文文档：[前往学习](https://doc.webpack-china.org/api/)
+ 文章demo: [前往作者的github](https://github.com/ccxzz/webpack-demo.git)

###项目依赖
***
+ webpack: `npm install webpack --save-dev`
+ path: `npm install path --save-dev`

###项目结构
***
![](http://upload-images.jianshu.io/upload_images/6215665-3059114380d71f6f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ page/index.js: 项目入口文件
+ webpack-config.js: webpack配置文件

###入口配置（entry）
***
> 一个应用程序的起点，应用程序从这里启动执行。每个html页面都对应一个入口起点。单页面应用只有一个入口起点，多页面应用有多个入口起点。
+ 单入口（单页面应用）
```
// webpack-config.js
const webpack = require('webpack')
const path = require('path')
module.exports = {
	entry: {
        // 配置入口文件
		index: './page/index.js'
	},
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: '[name].js',
		publicPath: "../"
	},
}
// 如果 entry: './page/index.js'   则会输出的文件名  默认为 main.js
// entry: {'a':'./a.js'} ------> a.js
// entry: {'b':'./a.js'} ------> b.js
// entry: './a.js' ------> main.js
```
+ 动态入口
```
// webpack-config.js
const webpack = require('webpack')
const path = require('path')

module.exports = {
    // entry接收function返回的对象
	entry: () => ({index: './page/index.js'}), // new Promise((resolve) => resolve({index: './page/index.js'}))
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: '[name].js',
		publicPath: "../"
	},
}
```
或者
```
// webpack-config.js
const webpack = require('webpack')
const path = require('path')

let entryFnc = () => {
	/*
	 * 实现入口文件逻辑
	 * ......
	 */
	return {index: './page/index.js'}
}

module.exports = {
	// 入口配置
	entry: entryFnc(),
	output: {
		path: path.resolve(__dirname, './dist'),
		filename: '[name].js',
		publicPath: "../"
	},
}
```
###输出配置（output）
***
>指示 webpack 如何去输出、以及在哪里输出你的「bundle、asset 和其他你所打包或使用 webpack 载入的任何内容」。
+ output.path
```
output: {
    // 配置输出目录
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].js',
    publicPath: "../"
}
// __dirname是nodejs的一个全局变量，输出为当前目录路径
// dist 是设置文件输出的目录，编译后，文件将输出到 '/dist' 目录下
```
![](http://upload-images.jianshu.io/upload_images/6215665-0a17488f546656e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ output.filename
```
//静态名称
output: {
    // 配置输出目录
    path: path.resolve(__dirname, 'dist'),
    // 配置输出文件名 -- 输出为a.js
    filename: 'a.js',
    publicPath: "../"
},

// 使用与入口文件名相同的名称
filename: "[name].js"

// 使用内部 chunk id
filename: "[id].bundle.js"

// 为输出文件名使用hash值
filename: "[name].[hash].bundle.js"

// 使用基于每个 chunk 内容的 hash
filename: "[chunkhash].bundle.js"
```
+ output.publicPath
```
// CDN（总是 HTTPS 协议）
publicPath: "https://cdn.example.com/assets/",
// 输出：
<script type="text/javascript" src="https://cdn.example.com/assets/index.js"></script>
```
```
// CDN (协议相同，取当前网站地址协议)
publicPath: "//cdn.example.com/assets/",
// 输出：
<script type="text/javascript" src="//cdn.example.com/assets/index.js"></script>
```
```
// 相对于服务(server-relative)
// 比如：http://localhost:63342/webpack-demo/dist/index.html
// js：http://localhost:63342/assert/index.js
publicPath: "/assets/",
// 输出：
<script type="text/javascript" src="/assert/index.js"></script></body>
```
```
// 相对于 HTML 页面
publicPath: "assets/",
publicPath: "../assets/",
// 输出：
<script type="text/javascript" src="assert/index.js"></script></body>
<script type="text/javascript" src="../assert/index.js"></script></body>
```
+ output.chunkFilename
```
//此选项决定了非入口(non-entry) chunk 文件的名称。
chunkFilename: 'js/[id].[chunkhash].js'
```
> 非入口chunk： 类似于使用require.ensure[[查看文档](https://doc.webpack-china.org/api/module-methods/#require-ensure)]去加载的模块才会出现这样的配置

###模块（module）
***
+ module.noParse
```
noParse: /jquery|lodash/

// 从 webpack 3.0.0 开始
noParse: function(content) {
  return /jquery|lodash/.test(content);
}
// 配置后webpack将不会扫描jquery lodash 模块， 可以提高webpack构建效率
// 值为正则表达式 或 function
```