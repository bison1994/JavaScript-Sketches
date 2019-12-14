### What

- HTTP 是明文传输，HTTPs 是加密传输，目的是解决三类风险
  + 窃听风险 => 加密信息，防窃听 
  + 篡改风险 => 校验信息，防篡改
  + 冒充风险 => 身份证书，防冒充


### How

- TLS 是传输层加密协议，是 HTTPS 安全的基础，其前身是 SSL，主流版本有 SSL3.0、TLS1.0、TLS1.1、TLS1.2
- 对称加密
- 三个基本过程
  + Hello【协商加密算法】
  + Certificate Exchange【签名认证】服务端发送用私钥加密的证书，客户端对证书进行验证
    - 浏览器会从 CA 预先获取证书列表
    - 客户端本质上只需要验证公钥
  + Key Exchange【会话密钥】双方协商生成"会话密钥"（session key）
    - 客户端生成一个随机的 key，用协商加密算法+服务端公钥加密后，发送给服务端，服务端通过私钥解密得到共享密钥
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


### HSTS

- 好处
  + 防止中间人劫持
  + 减少一次 302 重定向
- 如何使用
  + 服务器配置示例：nginx server 块：`add_header Strict-Transport-Security "max-age=63072000; includeSubdomains;`
  + 添加到 [preload list](https://hstspreload.org)，避免首次访问被劫持

> [参考一](https://www.jianshu.com/p/caa80c7ad45c)

> [参考二](https://www.jianshu.com/p/005f3466b714)

> [你所不知道的 HSTS](http://www.barretlee.com/blog/2015/10/22/hsts-intro/) by 小胡子哥
