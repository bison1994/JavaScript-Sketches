### 同源策略

- 相同的域（包括子域）
- 相同的端口
- 相同的协议


### CORS 跨域资源共享
ajax 早期版本严格遵守同源政策，既不能从另一个域读取数据，也不能向另一个域发送数据。CORS 是 W3C 推出的新标准，用于解除严格的跨域限制，提供一种实现跨域的机制，基本过程如下：

1. 浏览器使用绝对路径向不同源的目标域发送请求，请求头带 Origin: www.self.com
2. 目标域判断 Origin，如果符合预设，则在响应中返回 Access-Control-Allow-Origin: www.self.com，否则不发送该字段
3. 浏览器如未检测到 Access-Control-Allow-Origin，则拒绝接受响应并提示跨域错误

- 服务端可以将 Access-Control-Allow-Origin 设为 *，允许任何域进行通信
- CORS 跨域机制默认不会发送 cookie，要解除这个限制，需要三个条件
  + xhr 对象设置 withCredentials = true
  + Access-Control-Allow-Origin 不得为 *
  + 服务端设置响应头：Access-Control-Allow-Credentials: true
- CORS 允许跨域请求带来的一个问题是：数据可以被跨域发送到任何目标域


### JSONP
1. 声明一个全局函数用于接收回调
2. 通过 script 标签发起 get 请求，并在请求参数中带上回调函数的名称
3. 后端获取回调函数名，将数据与函数名拼接为一段 js 代码，返回浏览器
4. 浏览器执行该函数

```html
<script>
  function jsonpCallback (data) {
    data = JSON.parseInt(data)
  }
</script>
<script src="hhttp://www.demo.com/ajax/jsonp?callback=jsonpCallback"></script>
```

```php
$callback = !empty($_GET['callback']) ? $_GET['callback'] : 'callback';
echo $callback.'(.json_encode($data).)';
```
JSONP 的缺点
- 没有 error 事件，结果不可控

### 其它跨域技术
- 图片
- iframe
- 反向代理

### 跨域请求与 Session