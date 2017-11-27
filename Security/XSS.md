### What
XSS（Cross Site Scripting）是指从外部向网页注入脚本，然后脚本在某种情况下被网页执行的一种漏洞攻击。漏洞体现在两处：输入点与输出点。

- 反射型
  + 通常将 XSS 的 Payload 写在 URL 中，通过浏览器直接“反射”给用户
  + 这种攻击方式通常需要诱使用户点击某个恶意链接，才能攻击成功
  + 也称为非持久性 XSS
- 存储型
  + 通常将 XSS 储存到服务器中
  + 其它用户浏览包含恶意脚本的网页，就会触发攻击
  + 也称为持久性 XSS
- DOM Based XSS

> 一切输入都是有害的


### How

### 措施
- HTTPOnly
- 输入检查
  + 通常会检测并过滤一些敏感字符（黑名单）
  + 问题是有可能把正常的输入也过滤掉了
  + 因此更妥当的办法不是过滤，而是替换，比如将某些半角符号转化为全角
  + 根据场景，还可以采用白名单的思路
- 输出检查
  + 编码
  + 转义
- CSP
  + [Content Security Policy](https://jaq.alibaba.com/community/art/show?articleid=518)