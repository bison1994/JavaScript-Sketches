### 作用
用于分担高计算量的任务，确保主线程运行流畅

### 限制
- 同域限制，子线程加载的脚本文件，必须与主线程的脚本文件在同一个域
- DOM 限制，无法操控 DOM
- 脚本限制，作用域独立，无法访问主线程及其它 web work 的作用域
- 文件限制，子线程无法打开本机的文件系统（file://）

### 分类
- 普通: 只能与创造它们的主进程通信
- Shared Worker: 能被所有同源的进程获取
- Service Worker: 实际上是一个在网络应用与浏览器或网络层之间的代理层。它可以拦截网络请求，使得离线访问成为可能

### 子线程的创建

```js
/* 主线程 */
var worker = new Worker(/* js file */)
worker.postMessage(string | object); // tell worker to start up
```

### 主线程与子线程的通信

```js
/* 子线程 */
self.onmessage = function (event) {
  var data = event.data; // get message from parent thread
  self.postMessage(string | object); // send message to parent thread
}
/* 主线程 */
worker.onmessage = function (event) {
  var data = event.data; // get message from child thread
  self.postMessage(string | object); // send message to child thread
}
// 主线程与子线程间也可以传递二进制数据。
```

### 关闭子线程

```js
/* 主线程 */
worker.terminate();
/* 子线程 */
self.close();
```
