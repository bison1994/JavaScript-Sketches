### 客户端储存的应用场景

- 记录用户操作行为的数据
- 记录表单信息
- 跨页面数据共享
- 缓存数据，减少请求，降低网络负荷

### Cookie
- session cookie：会话结束即清除；
- persistent cookie：超过设定时间（expire）即清除；
- Cookie 的局限
  + cookie只能储存单一格式的数据；
  + 每个域名下可设置的cookie总数是有限的，一般不超过50个；
  + 每个cookie的大小有限，一般不超过4kb；
  + 设置和操作cookie比较麻烦，需要自行封装方法

### web storage
- web storage 一定程度上弥补了 cookie 的局限性，提供了一种容量更大且允许跨会话存在的储存机制
- storage 是一个引用类，它有两个实例：sessionStorage 和 localStorage
- 一般可储存容量小于 5MB
- API
  + .clear() 删除所有值
  + .key(index)	获取 index 处的值的名字
  + .getItem(name) 根据 name 获取相应的值
  + .setItem(name, value)	为指定的 name 设置一个值，或者用于设置新的名值对
  + .removeItem(name)	删除由 name 指定的名值对

### IndexedDB
- IndexedDB 是在浏览器中保存结构化数据的一种数据库
- 它采用对象储存数据，而不是用表
- 它也受同源限制。大小一般不超过5MB


### 缓存
- 缓存分为：服务端缓存和客户端缓存
- 缓存需要服务端和客户端共同完成，因此通常使用 HTTP Headers
- 浏览器在发送请求前，首先会检查是否存在对应的缓存。如果有，则判断缓存时间是否过期。如果是，再依次根据 Etag 和 Last-Modified 向服务端验证资源是否改变

### HTTP Headers
- Expires
  + 规定缓存失效时间。属于 HTTP1.0
  + 服务器告诉浏览器，在过期时间以前，都直接从缓存中读取该文件，而不再向服务器发送请求
  + 禁止缓存：`Pragma: no-cache`

- Cache-Control: max-age=2592000
  + 规定缓存有效时间。属于 HTTP1.1，优先级高于 Expires
  + 服务器告诉浏览器，在获取资源后2592000秒内，都直接从缓存中读取该文件，而不再向服务器发送请求
  + 禁止缓存：`Cache-Control: max-age=0, no-cache`

- ETag/If-None-Match
	+ 客户端储存响应头中的 ETag，在请求头中发送 If-None-Match，向服务端验证文件是否改动
  + ETag优先级高于 Last-Modified，所以会率先验证

- Last-Modified/If-Modified-Since
	+ 如果超出缓存时间，客户端会发送请求检测文件是否更新。如果未更新，服务器返回 `304 Not Modified`
	+ 客户端储存响应头中的 Last-Modified，在请求头中发送 If-Modified-Since，向服务端验证文件是否改动

```js
// 关闭缓存
xhr.setRequestHeader("If-Modified-Since", "0");
xhr.setRequestHeader("Cache-Control", "no-cache");

// meta 标签的问题是所有缓存代理服务器都不支持，因为代理服务器不会解析文档内容
<meta http-equiv="Pragma" content="no-cache"> 
```

### 设置缓存的决策流
- 是否需要缓存
- 是否被中间层缓存 => public/private
- 最大缓存时间 => max-age
