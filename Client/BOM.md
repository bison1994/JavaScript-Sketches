### Window
Window 对象是客户端脚本的全局对象。该对象不仅集成了 ECMAScript 的 API，同时还提供客户端自有的 API。


### 框架
每一个窗口/框架都拥有自己的 Window 对象。所有框架都保存在 frames 集合中，可通过 top.frames[name]的形式访问框架的 Window 对象。<br>
在顶层框架中，`top === window` <br/>
子框架中可通过 parent 对象访问其直属父框架。


### 计时器
实际的延迟往往大于 setTimeout() 设定的延迟，这是因为 JavaScript 是单线程的，setTimeout() 只有等前面的队列执行完毕，才会被执行。


### Location
- Property
  + href
  + hash
  + pathname
  + search
  + ...
- Method
  + reload()
    - reload(true) 表示绕过缓存
  + assign(URL)
    - 加载新的文档
  + replace(URL)
    - 直接替换当前页面，浏览器不会生成新的历史记录，用户无法通过 back 按钮回到前一页


### History
- Property
  + length
    - 历史记录的条数
- Method
  + go(n)
  + forward()
  + back()
  + pushState(data, title, url)
  + replaceState(data, title, url)


### Navigator
- appName | appVersion
  + 客户端名称及版本
- userAgent
- cookieEnabled
- platform
  + 操作系统相关信息
- geolocation


地理位置API依赖两个因素：设备与定位信息来源。

```js
navigator.geolocation.getCurrentPosition(update, handleError) // 单次请求
navigator.geolocation.watchPosition(update, handleError) // 只要位置发生变化就请求
```

watchPosition() 方法返回一个 id，用于 clearWatch()
update 函数接收一个参数，用于储存包含地理信息的位置对象

```js
function update (data) {
  var latitude = data.coords.latitude; // 获取纬度
  var longitude = data.coords.longitude; // 获取经度
  var accuracy = data.coords.accuracy; // 获取精度
  // 注意：以下信息并非所有设备都支持
  var altitude = data.coords.altitude; // 获取海拔高度，单位为m
  var heading = data.coords.heading; // 获取相对于正北的前进方向
  var speed = data.coords.speed; // 获取速度，单位m/s
}
```


### 对话框
alert()、prompt()、confirm()


### 打开新页面
open(url)


### 异常处理
Window 对象的 onerror 属性用于捕获异常并在控制台输出指定的信息

```js
window.onerror = function (message, source, lineno, colno, error) {}
```


### Document
Window 对象的 document 属性指向 Document 对象


### Vibration
navigator.vibrate(1000); // 使设备震动1s


### Luminosity
用于屏幕亮度调节，当亮度传感器感知外部亮度发生显著变化时，会触发 devicelight 事件


### Orientation
陀螺仪。用于获取设备摆放方向

```js
window.addEventListener("deviceorientation", function (event) {
  var alpha = event.alpha;
  var beta = event.beta;
  var gamma = event.gamma;
});
```


### Camera API
由系统实现，触发`<input type="file">`时自动提示是选择本地文件还是拍照上传


### Web Speech
用于语音输入


### Web Notification
用于在用户桌面显示通知信息


### Web Performance
ES5 引入"高精度时间戳"这个API，部署在 performance 对象上，用于测量网站性能


### Fullsreen
用于实现全屏展示


### Pointer Lock API
指针锁定。与 Fullsreen API 绑定使用


### Page Visibility
用于判断页面是否处于当前窗口

监测页面可见性变化的事件：`VisibilityChange`

页面可见性状态

- document.hidden // true | false
- document.visibilityState // visible | hidden | prerender（页面正在渲染，不可见）


### requestAnimationFrame

```js
function animate () {
  while (/* condition */) {
    requestAnimationFrame(animate);
  }
}
requestAnimationFrame(animate); // 执行
```


### Online/Offline
是否处于离线状态（断网）

```js
document.body.onoffline = function () {}
```