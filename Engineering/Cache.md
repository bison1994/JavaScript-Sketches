### 客户端缓存

- 客户端缓存分为：强制缓存与协商缓存，强制缓存优先级更高
- 缓存机制由服务端和客户端共同实现，二者通过 HTTP Headers 通信
- 浏览器在发送请求前，首先会检查本地是否存在对应的缓存。如果有，则判断缓存时间是否过期（新鲜度测试 Freshness check）。如果是，再依次根据 Etag 和 Last-Modified 向服务端验证资源是否改变
- 浏览器默认的缓存是放在硬盘里的（from disk cache），但浏览器有一定的优化机制：刷新页面时，体积小于一定值的资源会走 memory cache（from memory cache）。后者与网页进程相关，进程消失（切换或关闭 tab），缓存即消失
- 静态文件的缓存时间通常在服务器中配置，例如 Nginx：

```
location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
  expires 365d;
}
```


### HTTP Headers

- 强制缓存
  + Expires
    - 规定缓存失效时间。属于 HTTP1.0
    - 服务器告诉浏览器，在过期时间以前，都直接从缓存中读取该文件，而不再向服务器发送请求
    - 禁止缓存：`Pragma: no-cache`
    - 缺点：服务端和客户端时间可能不一致
  + Cache-Control: max-age=2592000
    - 规定缓存有效时间。属于 HTTP1.1，优先级高于 Expires
    - 服务器告诉浏览器，在获取资源后2592000秒内，都直接从缓存中读取该文件，而不再向服务器发送请求
    - 禁止缓存：`Cache-Control: max-age=0, no-store`
    - 使用协商缓存：`Cache-Control: max-age=10000, no-cache`

- 协商缓存
  + Last-Modified/If-Modified-Since
	  - 如果超出缓存时间，客户端会发送请求检测文件是否更新。如果未更新，服务器返回 `304 Not Modified`
	  - 客户端储存响应头中的 Last-Modified，在请求头中发送 If-Modified-Since，向服务端验证文件是否改动
    - 缺点：可能存在文件内容没有变化但 last-modified time 变化的情况
  + ETag/If-None-Match
	  - 客户端储存响应头中的 ETag，在请求头中发送 If-None-Match，向服务端验证文件是否改动
    - ETag 优先级高于 Last-Modified，所以会率先验证

```js
// 取消 get 请求的缓存
xhr.setRequestHeader("If-Modified-Since", "0")
xhr.setRequestHeader("Cache-Control", "no-cache")

// meta 标签的问题是代理服务器不支持，因为代理服务器不会解析文档内容
<meta http-equiv="Pragma" content="no-cache"> 
```

> [浏览器缓存机制 by Laruence](http://www.laruence.com/2010/03/05/1332.html)

> [Web 缓存机制系列 from AlloyTeam](http://www.alloyteam.com/2012/03/web-cache-2-browser-cache/)
