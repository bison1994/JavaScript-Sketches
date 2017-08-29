### 报文的结构
HTTP报文由三部分组成：起始行、首部和主体

#### 请求报文
&lt;Method> &lt;URL> &lt;Version> <br/>
&lt;Headers> <br/>
&lt;Entity-body>

#### 响应报文
&lt;Version> &lt;status> &lt;Reason-phrase> <br/>
&lt;Headers> <br/>
&lt;Entity-body>

<br/>

### 首部
HTTP定义了几种类型的首部：通用、请求、响应、实体、扩展 <br/>
应用程序可以自定义首部

- General
  + Connection
  + Date
    - 构建报文的时间
  + MIME-Version
  + Trailer
  + Transfer-Encoding
    - 告知接收端编码方式，确保报文可靠传输
  + Update
    - 发送端可能想要升级使用的新版本或协议
  + Via
    - 中间节点
  + Cache-Control
    - 请求头：no-cache | no-store | max-age | ...
    - 响应头：private | no-cache | no-store | no-transform | ...
  + Pragma
- Request Headers
  + User-Agent
  + Host
    - 接收端主机名 + 端口
  + Referer
    - 发送端的 URL
  + Accept
    - Accept | Accept-Encoding | Accept-Encoding | Accept-Language
  + Range
  + Expect
    - 发送端所要求的服务端行为
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
  + Allow
    - 可对实体执行的请求方法列表
  + Location
    - 告知客户端实体实际位于何处，用于重定向
  + Content
    - Content-Length | Content-Location | Content-Type | Content-Range | Content-MD5 | ...
  + ETag
  + Expires
  + Last-Modified
