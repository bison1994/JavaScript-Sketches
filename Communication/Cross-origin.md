### 同源策略
跨域请求并非是浏览器限制了发起跨站请求，而是请求可以正常发起，到达服务器端，但是服务器返回的结果会被浏览器拦截。

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