### 服务端推送之 Comet

```js
var listener = new EventSource("**.php");
listener.onmessage = function () {
  var data = event.data
}
```


### 跨文档消息传递

使用 `postMessge()` 发送信息，通过 window 对象的 message 事件处理接收的信息


### WebSocket

WebSocket 是 Http 之外的一种新的协议，其目的是实现双向沟通的实时通讯，支持音频、视频以及其它任意数据通信
  
```js
// 创建套接字
var socket = new WebSocket("ws://ws.**.com")
socket.onmessage = function() { } // 服务器发送了消息
socket.onopen = function() { } // 套接字已连接
socket.send(data) // 向服务器传送信息（支持UTF-8字符串、二进制数据）
socket.close() // 关闭连接
```


### Server-Sent Event

- 所谓SSE，就是浏览器向服务器发送一个HTTP请求，然后服务器不断单向地向浏览器推送“信息”（message）
-	SSE与WebSocket功能相似，但有以下区别：
	+ WebSocket是全双工通道，可以双向通信，功能更强；SSE是单向通道，只能服务器向浏览器端发送。
	+ WebSocket是一个新的协议，需要服务器端支持；SSE则是部署在HTTP协议之上的，现有的服务器软件都支持。
	+ SSE是一个轻量级协议，相对简单；WebSocket是一种较重的协议，相对复杂。
	+ SSE默认支持断线重连，WebSocket则需要额外部署。
	+ SSE支持自定义发送的数据类型。

```js
var source = new EventSource(url) // url为服务器地址，必须与当前网页同域

/*
 * 实例 source 有以下属性
 * 属性	readyState	0：未连接；1：已连接；2：已断线
 * 方法	close()	关闭连接
 * 事件	open	连接建立时触发
 * 事件	message	收到数据时触发
 * 事件	error	出错时触发
 */
```


### fetch

```js
fetch(url, { config }).then((response) => {
  response.json().then((data) => {
  	// data
	})
}).catch((err) => {
  // error
})
// 仅在网络失败时返回 err；4** 或 5** 的情况不会报错
```
- config
  + credentials	请求默认是不带 cookie，需将该选项设为 'include'
	+ mode	'cors'
	+ method	HTTP方法
	+ headers	在对象中设置请求头
	+ body	提交的数据
- response
  + headers	`response.headers.get('Content-Type')`
	+ status	状态码
	+ statusText	状态文本
	+ type	'basic'、'cors'或'opaque'
	+ url
	+ ok	`true/false`
	+ redirected	
	+ .json()
	+ .text()	
	+ .formData()	
	+ .arrayBuffer()	
	+ .blob()	
