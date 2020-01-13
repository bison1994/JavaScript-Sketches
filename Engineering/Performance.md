## 什么是性能

对网页而言，性能主要是指加载快。对于 js 框架而言，性能主要是指算法速度。对于后端接口或服务，性能又有其它定义。本文主要探讨网页性能

- **快**
  + 加载速度
  + 渲染速度
  + 交互、动画流畅度
- 稳定
- 低耗


## 衡量标准

- 传统指标
  + 首字节
  + DOMContentLoaded
  + load
- 渐进式网页指标（[Progressive Web Metrics](https://developers.google.com/web/fundamentals/performance/user-centric-performance-metrics)）
  + FP（First Paint）
  + FCP（First Contentful Paint）
  + FMP（First Meaningful Paint）
- 检测 DOM 节点变化最大的时刻
 

## 测量手段/工具

- Date.now()
- [Navigation Timing](https://www.w3.org/TR/navigation-timing/#sec-navigation-timing-interface)
  + PerformanceTiming
  + PerformanceNavigation
  + window.performance
- [PerformanceResourceTiming](https://www.w3.org/TR/resource-timing/)
- [User Timing](https://www.w3.org/TR/user-timing/)
  + PerformanceMark
  + PerformanceMeasure
  + Performance extensions
- [Performance Observer](https://github.com/bison1994/JavaScript-Sketches/blob/master/Client/Observer.md)
- [资源加载时序图分析](https://chromedevtools.github.io/timeline-viewer/)
- Lighthouse、Web Pagetest
- YSlow

> [Google web.dev](https://web.dev)


## 性能优化方案体系

### 针对加载速度

提高加载性能的四个基本方向：时间规划、距离规划、体量规划、特定环境规划

**时间规划**

- 并行/管道
  + [BigPipe](https://xianyulaodi.github.io/2018/02/10/BigPipe%E5%B0%8F%E6%8E%A2/)
  + [流式 JSON](https://www.zhihu.com/question/297751687/answer/887453401)
- 前置
  + SSR
  + prefetch、prerender、preload
  + 服务端推送


**距离规划**

距离规划的基本思路是缩短资源加载路径，常用手段有：

- CDN （雅虎14条之2）
- 缓存（服务器缓存、localStorage、离线化、本地化、web worker、redis...）
- [kv-storage](https://developers.google.com/web/updates/2019/03/kv-storage)
- [stale-while-revalidate](https://mp.weixin.qq.com/s/hW5POjIEujBaIyd4kpiIPQ)


> 动静分离：不经常变化的类静态数据，基本策略是缓存、本地化；要求实时更新的动态数据，基本策略是前置，尽早请求


**体量规划**

通常来说，加载资源的数量/体积越小，加载性能越高。针对 web 而言，主要是限制请求数量和资源体积

- 减少请求次数（雅虎14条之1）
  + 合并资源（spites）或合并资源的请求（CDN Combo）
  + 合并多个 ajax 请求
  + CSS inline
  + 使用 CSS、SVG、Inline Image、Icon-font 代替图片
  + 避免使用 @import 和 iframes
  + 控制域名数量，减少 DNS 查询（雅虎14条之9）
- 减少重复请求
  + 客户端缓存（雅虎14条之3、13）
  + 使用可缓存的 get 请求（雅虎14条之3、14）
- 减少不必要的请求
  + 移动端字体图标只需要用 ttf
- 只请求当前环境需要的资源
  + 增量加载（懒加载、渐进式加载、异步加载）
  + 按需加载 polyfill
    - [Conditionally-load-polyfills](https://golb.hplar.ch/2018/02/Conditionally-load-polyfills.html)
    - [loading-polyfills-only-when-needed](https://philipwalton.com/articles/loading-polyfills-only-when-needed/)
  + [modern 构建模式](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)
- 减小资源体积（雅虎14条之4、10）
  + 压缩
  + Gzip（服务端开启，只压缩文本文件，**不要压缩图片文件**）
  + 图片优化（渐进式、尺寸适配、剪裁、格式）
  + 根据网络、设备 DPI 加载不同的图片
  + 控制 cookie 大小
  + 控制 header 大小
  + 增量更新：只加载变化的部分
  + 用字符数最少的代码：例如用 void 0 代替 undefined
- 减小无效资源（雅虎14条之4、12）
  + 删除无效的代码（Dead Code Elimination）
    - 可使用浏览器开发工具检测
  + 删除重复的代码（[检测重复代码](https://elijahmanor.com/js-copypaste-detect/)）
- 减少无效请求
  + 避免空 src
  + 避免重定向（雅虎14条之11）


**特定环境规划**

涉及三个环境：网络协议、客户端（浏览器/webview、移动端/桌面端、小程序...）、服务端

- 协议层面：UDP、QUIC、SPDY、http 2
- 浏览器
  + 域名分片（Domain Sharding、SPDY）
    - PC 端可分片 2-4 个为宜
    - 移动端 DNS 解析很慢，分片不要超过 2 个
  + 针对加载过程的优化
    - [浏览器页面资源加载过程与优化 from 网易](https://juejin.im/post/5a4ed917f265da3e317df515)
    - [什么阻塞了 DOM](https://juejin.im/post/587f4afb61ff4b00651b3c18)
    - [关键路径优化](https://www.lucidchart.com/techblog/2018/03/13/the-critical-path-optimizing-load-times-with-the-chromedev-tools/)（雅虎14条之5、6）
    - [css-and-network-performance](https://csswizardry.com/2018/11/css-and-network-performance/)
- webview
  + 容器初始化时间
- 服务端
  + 负载规划

> [front-end-performance-checklist-2018](https://www.smashingmagazine.com/2018/01/front-end-performance-checklist-2018-pdf-pages/)

> [how-medium-does-progressive-image-loading](https://medium.com/@jmperezperez/how-medium-does-progressive-image-loading-fd1e4dc1ee3d)

> [h5 秒开方案大全](http://www.alloyteam.com/2019/10/h5-performance-optimize/)


### 针对渲染性能、交互、动画流畅度

- 浏览器环境
  + 控制 DOM 和 CSS 的层级深度
  + 点击延迟问题
  + 硬件加速
  + 减少重排
    - 重排由 CPU 处理，重绘由 GPU 处理，CPU 的处理效率远不及 GPU，重排一定会引发重绘，重绘不一定会引发重排
    - [CSS 属性的改变对不同浏览器重绘/重排的影响](https://csstriggers.com/)
    - 避免图片尺寸变化
- 代码层
  + 少用 js 动画
  + requestAnimationFrame
  + 合并多个 DOM 操作
  + 函数节流
  + DOM 元素离线更新（例如使用 Document Fragment）
- 交互设计
  + 动效元素少用阴影和渐变色（纯色、扁平化）

> [7-performance-tips-jank-free-javascript-animations](https://www.sitepoint.com/7-performance-tips-jank-free-javascript-animations/)

代码执行的效率，对用户而言基本无感知，但是对追求极致的框架而言，代码层的性能就很重要，除了算法设计，还有原生 api 的执行效率（测量工具：[jsperf](https://jsperf.com/)、[benchmarkjs](https://github.com/bestiejs/benchmark.js/)）

- call vs apply
- for vs forEach vs map
- ...

> [performance-vs-readability](https://blog.usejournal.com/performance-vs-readability-2e9332730790)


## 性能保障

- 建立指标
- 监控数据（维度、分位...）
- 巡检与通报
- CI 环节校验


> [Web Performance Geeks](https://calendar.perfplanet.com/)
