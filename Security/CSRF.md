### What

- 跨站点请求伪造（Cross Site Request Forgery），就是设法让终端用户执行某种带授权的非预期操作（trick a victim into submitting a forged request）。简单的说，就是构造出好像是用户发出的请求，但实际不是（服务器只识别 token，但不知道真正的发起人是谁）
- 伪造请求能够成功执行的两个条件
  + 获得用户权限
  + 请求的所有参数都能被攻击者获知（reproducible request）

> https://owasp.org/www-community/attacks/csrf


### How

场景一：构造 get 请求
<br>
某论坛支持在评论中发送超链接和图片外链，同时该论坛有一个 get 接口用于关注某人，因此攻击者可以构造一个用于关注自己的请求链接，以超链接形式发送到评论中，如果用户点击，就会发送该请求；如果以图片外链形式发送到评论中，则任何访问该页面的用户，无需点击，就会自动发送关注请求，而只要用户是已登录的，则该请求就能成功执行
<br>
<br>
场景二：构造 post 请求
<br>
在场景一中，因为是 get 请求，因此不需要跨站也有办法伪造请求，但如果接口的请求方法是 post，则需要编写代码，而代码需要存放在另一个网页，这就是跨站请求伪造。通常伪造 post 请求的方式是构造一个 form 表单用于提交 post 请求，然后用 js 自动提交表单。注意，form 表单提交是不受同源限制的。因此只要诱使用户点击链接进入该页面，该请求就会执行

> [什么是 CSRF](https://zhuanlan.zhihu.com/p/22521378)

> [OAuth 与 CSRF](https://www.zhihu.com/question/19781476)


### 防御措施

防御的基本思路就是破坏伪造请求得以成功的条件

- 针对跨站点
  + referrer
    - referrer check 只能防止一部分的 CSRF
    - 某些情况下浏览器可能不会发送 referrer
- 增加参数的不可预测性
  + 对参数加盐
    - 可能对数据分析不利
  + anti CSRF token
    - 新增一个不可预知的参数
    - token 是一个随机的值
    - token 可以储存在 input hidden 中
