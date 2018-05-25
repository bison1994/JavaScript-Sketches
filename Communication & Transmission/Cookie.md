### What
cookie 是以分号区隔的名值对形式储存于客户端的一段字符串。

### 工作机制
同域内，浏览器发送的任何请求都会带上 cookie，cookie 设置于请求头中的 Cookie 字段。<br>
服务端可以在响应头 Set-Cookie 字段任意操作 Cookie，客户端也能通过 `document.cookie` 读取非 httponly 的 cookie 以及设置 cookie

### 组成
Cookie 是由键值对构成的字符串，其中有几个重要字段：
- [name]
- [value]
- [domain] 默认为当前域，用于限制客户端脚本读取 cookie 的权限
- [path] 默认为当前页面的路径，功能同上
- [expires] 未设置 expires => session cookie，储存于内存；设置了 expires => persistent cookie，储存于本地
- [httponly] httponly 默认值为 0，设为 1 表示仅存在于 http 层，客户端脚本无权读取
- [secure] 带有 secure 标识，表示该 cookie 仅用 https 传输

name 和 value 为必填，其余为选填

### Cookie 的局限
- cookie 只能储存单一格式的数据
- 单个域名下 cookie 总数有限，一般不超过 50 个
- 单个域名下 cookie 总体大小有限，一般不超过 4kb
- 设置和操作 cookie 比较麻烦，需要自行封装方法
- 过度使用 cookie 会影响请求与响应的效率


