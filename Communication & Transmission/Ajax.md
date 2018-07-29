### 客户端发起 Http 请求的方式

- `<img src="">`
  + 返回图片格式数据
  + 不受同源限制
  + 常用于仅发送不接受的场景
- `<iframe src="">`
  + 返回 HTML 文档
  + 受限于同源政策
- `<script src="">`
  + 返回 js 文件
  + 不受同源限制
- XMLHttpRequest
  + XMLHttpRequest 对象的每一个实例支持一个独立的请求/响应对
  + 老版的实现仅支持文本类型的数据，且受同源限制
  + 新版本（XHR2，IE10 以下不支持，IE8/9 可用 XDomainRequest）扩展了许多功能
    - 可以设置HTTP请求的时限
    - 可以使用FormData对象管理表单数据
    - 可以上传文件
    - 可以跨域请求（CORS）
    - 可以获取服务器端的二进制数据
    - 可以获得数据传输的进度信息

> [XHR2 参考](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)

### Asynchronous JavaScript and XML

- Ajax 是一种操控客户端 http 请求，实现客户端和服务端异步通信的 Web 应用层协议
- Asynchronous：ajax 由独立的线程执行，不会阻塞主线程
- JavaScript：ajax 由 JS 发起。但 JS 对请求头的操作是受限的
- XML：响应的数据格式，目前被 JSON 取代

### Http 请求的四个组成部分
- 方法（必须）
- 地址（必须）
- 请求头（可选）
- 请求主体（可选）

```js
var xhr = new XMLHttpRequest();

// 设置方法和地址，第三个参数：true 表示异步发送，false 表示同步发送
xhr.open("get", "example.php", false);

// 设置请求头，通常 send 方法会自动设置，一般无须操作
xhr.setRequestHeader();

// 发送，参数为请求主体
xhr.send(null);

// 取得响应
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4) {
    if (xhr.status >= 200 && xhr.status <= 300 || xhr.status == 304) {
      var type = xhr.getResponseHeader("Content-Type");
      // 返回文本格式数据，也可能是文档格式数据 responseXML
      if (type.match(/^text/)) {
        var data = xhr.responseText; 
      }
      // 获得JSON格式的数据
      if (type === "application/json") {
        var data = xhr.responseText; 
        data = JSON.parse(data);
      }
    }
  }
}
```
> 一次ajax请求, 并非所有的部分都是异步的, 至少"readyState==1"的 onreadystatechange 回调以及 onloadstart 回调就是同步执行的

### API

- 属性
  + readyState
    - 0 (begin）=> 1 (open-loadstart) => 2 (send) => 3 (loading-progress) => 4 (done-load)
  + status
  + statusText
  + timeout
  + responseText、responseXML、responseType、responseURL
  + withCredentials
  + upload
    - returns an XMLHttpRequestUpload object
- 方法
  + abort()
  + getResponseHeader() | getAllResponseHeaders()
  + setRequestHeader()
  + open()
  + send()
- 事件
  + readystatechange
  + loadstart
  + progress
  + load
  + loadend
  + timeout
  + error

```js
xhr.onprogress // 下载的进度
xhr.upload.onprogress // 上传的进度

// 进度信息保存在事件对象里
event.lengthComputable // 是否可计算进度
event.loaded // 已经传输的字节
event.total // 需要传输的总字节

// 相关事件
load // 传输成功完成。
abort // 传输被用户取消。
error // 传输中出现错误。
loadstart // 传输开始。
loadEnd // 传输结束，但是不知道成功还是失败
```


### 数据格式与编码
由于 Http 只能传输特定格式的数据，因此数据的发送和接收都需要相应的编码和解码的工作，有的工作是由浏览器实现的，而有的工作需要人工完成。

- 表单型数据编码
  + 将名与值进行URL编码（十六进制转义码）并以=连接，名值对之间以&连接
  + 该数据格式的 MIME 类型：application/x-www-form-urlencoded
  + 使用 POST 方法提交数据时，需设置 Content-Type 为该值
  + 可在 URL 中使用该种数据类型进行 GET 只读查询

- JSON 编码
```js
xhr.setRequestHeader("Content-Type", "application/json");
xhr.send(JSON.stringify(data));
```

- 文件
```js
// XHR2 支持 Blob 的数据格式
input.addEventListener("change", function () {
  var file = this.files[0]; 
  var xhr = new XMLHttpRequest();
  xhr.open("POST", url);
  xhr.send(file);
})
// jQuery
// 使用 FormData 对象一次性上传混合多种格式的数据（multipart/form-data）。
input.addEventListener("change", function (e) {
  var file = e.target.files[0];
  var fd = new FormData();
  fd.append('file', file);
  $.ajax({
    url: '',
    type: 'post',
    processData: false,
    contentType: false,
    data: fd
  })
})
```

### ajax 的执行过程







