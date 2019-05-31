### 同源策略

- 由浏览器实现
- 同源
  + 相同的域（包括子域）
  + 相同的端口
  + 相同的协议


### CORS 跨域资源共享

- ajax 早期版本严格遵守同源政策，既不能从另一个域读取数据，也不能向另一个域发送数据
- CORS 是 W3C 推出的新标准，用于解除严格的跨域限制，提供一种实现跨域的机制，基本过程如下：
  + 浏览器使用绝对路径向不同源的目标域发送请求，请求头带 `Origin: www.self.com`，如果是复杂请求，还会带上 `Access-Control-Request-Headers`、`Access-Control-Request-Method`
  + 目标域在响应中返回服务端设置的 `Access-Control-Allow-Origin`、`Access-Control-Allow-Methods`、`Access-Control-Allow-Headers`
  + 浏览器根据返回的许可信息，判断请求是否合法
  + 浏览器如未检测到 Access-Control-Allow-Origin，或者判断请求不合法，则拒绝执行响应并提示跨域错误
- 服务端可以将 Access-Control-Allow-Origin 设为 *，表示允许任何域进行通信
- CORS 跨域机制默认不会发送 cookie，要解除这个限制，需要三个条件
  + xhr 对象设置 withCredentials = true
  + Access-Control-Allow-Origin 不得为 *
  + 服务端设置响应头：Access-Control-Allow-Credentials: true
- Access-Control-Expose-Headers
  + 服务器设置的 header 白名单，告诉浏览器通过检验


**Preflighted requests（带预检的请求）**

- 简单请求
  + 方法仅限于：get、post、head
  + 仅允许浏览器自动添加的头和规范定义的安全头（CORS-safelisted request-header）
  + Content-Type 仅限于：
    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain
  + 其它（见 CORS 参考）
- 复杂请求
  + 只要不满足上述条件，则视为复杂请求，会先发送 options 预检请求。例如：无论是 get 还是 post，只要 content-type 为其它值（比如 application/json），或者自定义的设置了规定之外的 header，都会视为复杂请求。
- Access-Control-Max-Age
  + 服务端可返回上述头，告诉浏览器对预检请求的缓存时间，单位为秒
  + 浏览器内部有自己的最大缓存时间，chrome 设置的是 10 分钟
- 复杂请求，如果 preflight 请求返回 302 重定向，浏览器会直接拒绝执行（简单请求没问题）。不过以后浏览器可能会[修复这一问题](https://github.com/whatwg/fetch/commit/0d9a4db8bc02251cc9e391543bb3c1322fb882f2)


> [CORS 参考](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) from MDN

> [跨域的常见错误](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS/Errors)


### JSONP

- 原理
  + 声明一个全局函数用于接收回调
  + 通过 script 标签发起 get 请求，并在请求参数中带上回调函数的名称
  + 后端获取回调函数名，将数据与函数名拼接为一段 js 代码，返回浏览器
  + 浏览器执行该函数
- 缺点
  + 只能发送 get 请求
  + 需要服务端支持
  + 没有 error 事件，结果不可控

```html
<script>
  function jsonpCallback (data) {
    data = JSON.parseInt(data)
  }
</script>
<script src="http://www.demo.com/ajax/jsonp?callback=jsonpCallback"></script>
```

```php
$callback = !empty($_GET['callback']) ? $_GET['callback'] : 'callback';
echo $callback.'(.json_encode($data).)';
```

### Server-side proxy

### window.postMessage


### 其它跨域技术

- 图片
- iframe


### 检测跨域问题

```js
// https://github.com/eligrey/FileSaver.js/blob/master/src/FileSaver.js
function corsEnabled (url) {
  var xhr = new XMLHttpRequest()
  // use sync to avoid popup blocker
  xhr.open('HEAD', url, false)
  try {
    xhr.send()
  } catch (e) {}
  return xhr.status >= 200 && xhr.status <= 299
}
```