### OAuth

- [官网](https://oauth.net/2/)
- [OAuth 四种模式概述](https://itnext.io/an-oauth-2-0-introduction-for-beginners-6e386b19f7a9)

**基本模式**

<img src="https://raw.githubusercontent.com/bison1994/JavaScript-Sketches/master/Assets/OAuth.png">


**关键问题**

- 为什么需要用 authorization_code 去换取 access_token，而不是直接签发 access_token？

<br>

### PKCE（Proof Key for Code Exchange）

- 用于单点登录（SSO）

> [规范](https://tools.ietf.org/html/rfc7636)
