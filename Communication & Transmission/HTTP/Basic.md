### 网络
- 网络的本质是连接 + 传输。网络通信的本质是交换数据包
- 网络采用分层架构，每一层拥有各自的协议，封装特定的能力，每一层依次建立于下层之上，层与层之间通过接口实现沟通
- 网络分层
  + 应用层（Http、DNS、FTP...）
  + 传输层（TCP、UDP...）
  + 网络层（IP...）
  + 链路层（802.11、Ethernet...)
  + 物理层

### 资源

- Http 会为每一种资源打上数据格式标签（MIME Type）
- MIME 是指对传统电子邮件技术规范的一个扩展，它规定了 Content-type 字段，用于描述资源的数据格式。后来该字段被 HTTP 采用，用于标识资源类型
- 主要类型一共10种：
	application、audio、font、example、image、message、model、multipart、text、video
- 主要类型下分若干次级类型
- 如果主类型为 text，通常还需要指明编码类型 charset


### URL

- 基本结构
  + 协议 scheme
  + 服务器地址 host
  + 资源路径 path
- 细分结构
  + scheme、user、password、host、port、path、params、query、hash


### 报文的结构

- HTTP 报文由三部分组成：起始行、首部和主体
- 请求报文
```
<Method> <URL> <Version>
<Headers>
<Entity-body>
```

- 响应报文
```
<Version> <status> <Reason-phrase>
<Headers>
<Entity-body>
```


### 首部

- HTTP定义了几种类型的首部：通用、请求、响应、实体、扩展
- 应用程序可以自定义首部
- General
  + Connection
  + Date 构建报文的时间
  + MIME-Version
  + Trailer
  + Transfer-Encoding 告知接收端编码方式，确保报文可靠传输
  + Update 发送端可能想要升级使用的新版本或协议
  + Via 中间节点
  + Cache-Control
    - 请求头：no-cache | no-store | max-age | ...
    - 响应头：private | no-cache | no-store | no-transform | ...
  + Pragma
- Request Headers
  + User-Agent
  + Host 接收端主机名 + 端口
  + Referer 发送端的 URL，可用于防图片盗链
  + Accept
    - Accept 客户端能够接收的内容类型
    - Accept-Encoding
    - Accept-Language
  + Range
  + Expect 发送端所要求的服务端行为
  + If-Match
  + If-Modified-Since
  + If-Unmodified-Since
  + If-None-Match
  + If-Range
  + Cookie
  + Client-IP
  + UA-*
    - UA-CPU | UA-OS | ...
  + Authorization
  + ...

> 尽管规范中没有定义，但很多客户端程序实现了 Client-IP 和 UA-*

- Response Headers
  + Age
  + Public
  + Server
  + Accept-Ranges
  + Set-Cookie
  + WWW-Authenticate
  + ...

- Entity Headers
  + Allow 可对实体执行的请求方法列表
  + Location 告知客户端实体实际位于何处，用于重定向
  + Content
    - Content-Length | Content-Location | Content-Type | Content-Range | Content-MD5 | ...
  + ETag
  + Expires
  + Last-Modified

> [参考](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)

### Method

方法的分类
- Safe method
  + Safe：GET、HEAD
  + Unsafe: POST、PUT、DELETE
- Idempotent method
  + Idempotent: GET、HEAD、PUT、DELETE、OPTIONS、TRACE
  + None-Idempotent: POST

HTTP1.1 定义的七种方法
- GET
- HEAD
  + 功能同 GET，只是响应中仅包含响应头，不包含实体
  + 用于 testing hypertext links for validity, accessibility, and recent modification.
- POST
- PUT
  + 将请求中携带的实体数据存放到 URI 指定的位置
  + 如果服务器是创建新的资源，则返回 201（created），如果是覆盖已有的资源，则返回 200 或 204
  + 如果服务器不支持创建相应资源，则返回 501（not implemented）
  + PUT 和 POST 都可以用于 create，但它们在语义上有区别：PUT 类似于给一个对象的某个属性赋值；POST 类似于给一个数组 push 一个值
- TRACE
- OPTIONS
- DELETE

> [post 相比 get 有很多优点，为什么现在的 HTTP 通信中大多数请求还是使用 get？- 回答作者: 罗志宇](https://www.zhihu.com/question/31640769)


### Status

- 1xx: 表示请求已经接受，继续处理（Informational）
- 2xx: 表示请求已经处理（Successful）
  + 204 No Content
- 3xx: 重定向（Redirection）
  + 301 永久重定向 Moved Permanently
  + 302 暂时重定向 Move temporarily
  + 304 Not Modified
- 4xx: 一般表示客户端有错误（Client error）
  + 400 请求无效 Bad Request
  + 401 未授权 Unauthorized
  + 403 禁止访问 Forbidden
  + 404 无法找到资源 Not Found
  + 405 Method Not Allowed
- 5xx: 一般表示服务端有错误（Server error）
  + 500 服务器内部错误 Internal Server Error
  + 501 Not Implemented
  + 502 Bad Gateway
  + 503 Service Unavailable
  + 504 Gateway Timeout


### HTTP1.0

- 每个请求都需要打开一个新的 TCP 连接，一个 TCP 连接只能发一个请求，请求完毕后则关闭，新的请求需要重建连接

### HTTP1.1
- 持久连接（connection: keep-alive），允许多个请求复用同一个 TCP 连接
- 默认所有连接都是持久连接
- 服务端为了减少开销会设置 timeout，超过一定时间（例如 nginx 默认为 65 秒）没有发生新的请求，就会自动断连
- 针对同一个域名，浏览器可并行建立的连接数一般为 4、6、8 个
- 长连接 enables 管道机制（HTTP pipelining） => 同一个连接中并行响应多个资源

> HTTP keep-alive 与 TCP keep-alive 是完全不同的东西。前者通常由服务端控制，如果一定时间内没有新的请求，就断开连接（断开 HTTP 所依赖的 TCP 连接）。