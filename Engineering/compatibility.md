### 兼容问题分类

- 根据语言
  + HTML
  + CSS
  + JS
- 根据平台/内核/JS 引擎
  + 移动端
    - Android webview
    - Android Browser
    - IOS webview
    - Safari
  + PC 端
    - IE9-
    - 现代浏览器


### 处理兼容问题的基本思路

1. 划定兼容范围
2. 优雅降级
3. 使用编译工具
4. 引导升级


### 基本手段

- 使用标准语法
- postcss、babel
- polyfill、shim
- css hack
  + 条件注释
  + 私有属性
  + 利用层叠


### 检测方法

- js
  + 能力检测
  + 客户端检测
  + modernizr
- css
  + @supports
- 其它工具
  + [css3test](https://css3test.com/)

> [细说浏览器特性检测](http://otakustay.com/feature-detection-event/)


### 查询

- [caniuse](http://caniuse.com/)
- [msdn](https://msdn.microsoft.com/en-us/library/cc351024(VS.85).aspx)
- [w3help](http://www.w3help.org/zh-cn/causes/ )

> [Taking the guesswork out of web compatibility](https://hacks.mozilla.org/2018/02/mdn-browser-compatibility-data/)