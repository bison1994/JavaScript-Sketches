### 从 URL 到页面显示发生了什么

- 浏览器 url 自动补全、预先进行 DNS lookup、安全检查
- 读取 DNS 缓存。大致分为浏览器缓存和系统缓存，各有时效限制，短则几秒，长则数小时
- 读取 host 文件
- DNS Lookup
- 三次握手/四次握手


> [从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/) 百度吴多益
> [web-browser-dns-caching-bad-thing](https://dyn.com/blog/web-browser-dns-caching-bad-thing/)