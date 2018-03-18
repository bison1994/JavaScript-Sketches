### What
> webpack is a module bundler. Its main purpose is to bundle JavaScript files for usage in a browser, yet it is also capable of transforming, bundling, or packaging just about any resource or asset.

### Origin
webpack 的创始人叫 Tobias Koppers，他创造 webpack 的初衷就是希望给一个叫 modules-webmake 的 js 打包工具加上 code spliting 的功能，[看这里](https://github.com/medikoo/modules-webmake/issues/7)。webpack 的创造也得益于从 browserify 和 require.js 中获得灵感。

### 核心概念
- webpack 根据**依赖关系图 dependency graph**将**模块 module**打包为**js bundle**
- 所有的文件，无论何种类型，都被视为模块，模块间通过下列方式建立依赖关系
  + ES2015 import 语句
  + CommonJS require() 语句
  + AMD define 和 require 语句
  + css/sass/less 文件中的 @import 语句。
  + 样式(url(...))或 HTML 文件(&lt;img src=...>
- 通过 `webpack-bundle-analyzer` 可以生成[依赖关系图](https://medium.com/webpack/webpack-bits-getting-the-most-out-of-the-commonschunkplugin-ab389e5f318) 
- webpack 本身只处理 js 类型的模块，其它类型的模块只要能找到对应的 loader，就能被处理为 webpack 模块
- 模块[不可以出现循环引用](https://m.aliyun.com/yunqi/jsarticle/15709)，特别是 js 模块
- 入口 entry 是依赖关系图的起点，可以是单入口，也可以是多入口
  + 每个依赖关系图生成一个 bundle
  + 单入口对应一个依赖关系图，生成一个 bundle
  + 多入口对应多个依赖关系图，生成多个 bundle

```js
// 单入口：只有一组键值对，对应一个依赖关系图
entry {
  main: './src/entry.js'
}
// 值可以是数组，表示一个依赖关系图的起点可以是多个（multi-main entry）
entry {
  main: ['./src/main.js', './src/sub.js']
}
// 多入口：多组键值对，对应多个依赖关系图
entry: {
  home: './src/home.js',
  about: './src/about.js',
  vendor: ['vue', 'jquery', 'moment'] // 不推荐手动指定 vendor，而应使用 CommonsChunkPlugin 或 DllPlugin
}
```
> webpack 将图片、CSS 等所有资源都视为模块并打包到 js 中的设计理念可能是造成理解困难的重要原因。gulp、fis 等传统的构建工具通过定义任务流来处理不同类型的资源，自动化的执行原本需要人工执行的任务，而 webpack 原本的定位是 js bundler，它本非构建工具，却被用作了构建工具。我们更习惯 process assets，而不是 webpack 官网宣示的 bundle your scripts、images、styles、assets。正因为 webpack 用打包 js 模块的思路来对待所有资源，所以才会出现一些反直觉的用法，比如在 js 中 import CSS 或图片，又比如为了得到独立的 css 文件，必须额外配置插件。 

### chunk
- webpack 的本意是将所有模块打包成 1 个 js bundle
- 但是，我们可以通过配置打包出多个文件（separate files），这就等于是对 bundle 做了 code spliting
- 拆分出来的独立的 js 文件就被称为 chunk。注意，是 js
- 拆分的方式
  + 多入口，每一组键值对对应一个 chunk
  + CommonsChunkPlugin
  + 动态引入 import()

### Resource
- [webpack book](https://survivejs.com/webpack/)
- [webpack-front-end-size-caching](https://iamakulov.com/notes/webpack-front-end-size-caching/)
- [webpack dll](https://segmentfault.com/a/1190000005969643)
- [keep-webpack-fast](https://slack.engineering/keep-webpack-fast-a-field-guide-for-better-build-performance-f56a5995e8f1)