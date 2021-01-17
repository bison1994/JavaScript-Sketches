## What

HTTP 是明文传输，HTTPS 是加密传输，目的是解决三类风险

+ 窃听风险 => 加密 
+ 篡改风险 => 校验
+ 冒充风险 => 证书

从信息接收者的角度看，安全意味着：

1. 可以确信：信息的确来自预期的沟通对象，而不是别人
2. 可以确信：信息没有被篡改，是完整无误的
3. 可以确信：该信息只有自己知道，没有被其他人获知

HTTPS 依赖 TLS。TLS 是传输层加密协议，是 HTTPS 安全的基础，其前身是 SSL，主流版本有 SSL3.0、TLS1.0、TLS1.1、TLS1.2

```
--- 应用层 ---
--- 【SSL/TSL 套接字】 ---
--- SSL/TSL 子层 ---
--- 【TCP 套接字】 ---
--- TCP 传输层 ---
```

<br />

## How

#### 1、如何加密？

通过会话密钥对称加密

#### 2、如何防篡改？

#### 3、如何认证身份？

浏览器对证书的校验，依赖一种 chain of trust 的机制。即通过上级签发者的合法性来确认证书的合法性

> 具体算法：[RFC 5280](https://tools.ietf.org/html/rfc5280#section-6)

浏览器对证书的校验包括：通过签名校验完整性、是否过期、是否被吊销等


<br />

## Handshake

1. browser => server

客户端发送信息：我支持哪些加密协议和算法

2. server => browser

服务器从中选择一种协议和算法，同时附上自己的证书与公钥，返回给客户端

3. browser => server

客户端校验服务器身份，确认无误，生成一个随机的共享密钥，用服务器公钥加密后发给服务器

4. server => browser

服务器用私钥解密，得到共享密钥，随后双方通过共享密钥对称加密信息进行通信


<br />

## Is HTTPS really safe?

首先，单从技术上讲，协议及其实现总有可能存在漏洞，[例如](https://robertheaton.com/2015/04/06/the-ssl-freak-vulnerability/)

其次，绕开或破坏 HTTPS 本身的基本构件，也可以突破 HTTPS

- Break into any Certificate Authority
- Compromise a router near any Certificate Authority
- Compromise a Certificate Authority's recursive DNS server
- Attack some other network protocol, such as TCP or BGP
- A government could order a Certificate Authority to produce a malicious certificate (only speculative)
- If the client is somehow tricked into trusting a certificate and public key whose private key is controlled by an attacker, trouble begins. [see](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)


<br />

## HSTS

HTTP Strict Transport Security

目的：强制使用 HTTPS，确保浏览器绝对不会发送 http 的请求，从而避免劫持，增强 HTTPS 的有效性

实现方式：

- 通过 header 告诉浏览器强制使用 HTTPS nginx server 块配置：`add_header Strict-Transport-Security "max-age=63072000; includeSubdomains;`，浏览器会缓存一份需要强制走 HTTPS 的名单
- 将域名添加到 [preload list](https://hstspreload.org)，浏览器厂商提供的 HSTS 名单，可避免首次 http 访问被劫持
- 直接在浏览器内部配置：chrome://net-internals/#hsts

具体过程：

- 首次用 http 访问，浏览器返回 301 或 302，用 Location 头指明 HTTPS 版本的地址
- 浏览器重新请求 HTTPS 版本的地址，响应返回 Strict-Transport-Security 头，浏览器将该域名添加到 HSTS 缓存
- 再次用 http 访问，浏览器会返回 307 Internal Redirect，注意，不是服务器返回的，是浏览器“伪造”的响应，请求根本没发出去
- 浏览器自动请求 HTTPS 版本的地址

> [Status Code:307 Internal Redirect和Non-Authoritative-Reason:HSTS问题](https://www.jianshu.com/p/005f3466b714)

> [你所不知道的 HSTS](http://www.barretlee.com/blog/2015/10/22/hsts-intro/)


## Proxy & HTTPS

Charles 和 Fiddler 这类代理如何抓到 HTTPS 数据明文？

基本原理是利用中间人攻击的模式，并将代理服务的证书安装到客户端作为根证书，代理作为服务器与客户端建立 HTTPS 通信，同时作为客户端与真正的服务端建立另一段 HTTPS 通信

> https://docs.mitmproxy.org/stable/concepts-howmitmproxyworks/#explicit-https
