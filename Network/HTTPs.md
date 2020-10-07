### What

- HTTP 是明文传输，HTTPs 是加密传输，目的是解决三类风险
  + 窃听风险 => 加密信息，防窃听 
  + 篡改风险 => 校验信息，防篡改
  + 冒充风险 => 身份证书，防冒充


### How

- TLS 是传输层加密协议，是 HTTPS 安全的基础，其前身是 SSL，主流版本有 SSL3.0、TLS1.0、TLS1.1、TLS1.2
- 对称加密
- 基本过程
  + 协商加密算法
  + 签名认证（Certificate Exchange）
    - 服务端发送用私钥加密的证书，客户端对证书进行验证（[验证算法](https://www.ssl.com/article/browsers-and-certificate-validation/)）
    - 客户端会从 CA 预先获取证书列表
    - 客户端本质上只需要验证公钥
  + 计算会话密钥（session key）
    - 客户端生成一个随机的 key，用协商的加密算法+服务端公钥加密后，发送给服务端，服务端通过私钥解密得到共享密钥
    - 使用会话秘钥而非直接用公钥进行加密是为了降低计算量，提高性能
  + 随后双方即可采用"会话密钥"进行加密通信

> If the client is somehow tricked into trusting a certificate and public key whose private key is controlled by an attacker, trouble begins. [see](https://robertheaton.com/2014/03/27/how-does-https-actually-work/)

```
--- 应用层 ---
--- 【SSL/TSL 套接字】 ---
--- SSL/TSL 子层 ---
--- 【TCP 套接字】 ---
--- TCP 传输层 ---
```


### HTTPs 一定安全吗

首先，单从技术上讲，协议及其实现总有可能存在漏洞，[例如](https://robertheaton.com/2015/04/06/the-ssl-freak-vulnerability/)

其次，绕开或破坏 HTTPs 本身的基本构件，也可以突破 HTTPs

- Break into any Certificate Authority
- Compromise a router near any Certificate Authority
- Compromise a Certificate Authority's recursive DNS server
- Attack some other network protocol, such as TCP or BGP
- A government could order a Certificate Authority to produce a malicious certificate (only speculative)

> [reference](https://love2dev.com/blog/how-https-works/)


### HSTS（HTTP Strict Transport Security）

目的：强制使用 HTTPS，确保浏览器绝对不会发送 http 的请求，从而避免劫持，增强 HTTPS 的有效性

实现方式：

- 通过 header 告诉浏览器强制使用 https，例如 nginx server 块配置：`add_header Strict-Transport-Security "max-age=63072000; includeSubdomains;`，浏览器会缓存一份需要强制走 HTTPS 的名单
- 将域名添加到 [preload list](https://hstspreload.org)，浏览器厂商提供的 HSTS 名单，可避免首次 http 访问被劫持
- 直接在浏览器内部配置：chrome://net-internals/#hsts

具体过程：

- 首次用 http 访问，浏览器返回 301 或 302，用 Location 头指明 HTTPS 版本的地址
- 浏览器重新请求 HTTPS 版本的地址，响应返回 Strict-Transport-Security 头，浏览器将该域名添加到 HSTS 缓存
- 再次用 http 访问，浏览器会返回 307 Internal Redirect，注意，不是服务器返回的，是浏览器“伪造”的响应，请求根本没发出去
- 浏览器自动请求 HTTPS 版本的地址

> [Status Code:307 Internal Redirect和Non-Authoritative-Reason:HSTS问题](https://www.jianshu.com/p/005f3466b714)

> [你所不知道的 HSTS](http://www.barretlee.com/blog/2015/10/22/hsts-intro/)
