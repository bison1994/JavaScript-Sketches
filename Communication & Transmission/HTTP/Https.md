### What
- HTTP 是明文传输，HTTPs 是加密传输，目的是解决三类风险
  + 窃听风险 => 加密信息，防窃听 
  + 篡改风险 => 校验信息，防篡改
  + 冒充风险 => 身份证书，防冒充

### How

- 对称加密
- 基本过程
  + 客户端向服务器端索要并验证公钥
  + 双方协商生成"对话密钥"（session key），使用会话秘钥而非公钥进行加密是为了降低计算量，提高性能
  + 双方采用"对话密钥"进行加密通信
- HTTP 升级到 HTTPs

### HTTPs 一定安全吗


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
