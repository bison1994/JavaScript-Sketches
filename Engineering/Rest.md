### Restful API

> 一套 API 的设计风格或约定

- 利用 HTTP 动词表明意图
  + GET    获取资源
  + POST   新建或更新资源
  + PUT    更新资源
  + DELETE 删除资源
- 约定以 JSON 格式传送数据
- url 使用名词与层级结构来表示某种资源
- 正确的设置状态码
- 使用 HTTP Header 发送某些元数据
- 对接口使用基于 JWT 的无状态认证机制
- 对接口访问进行频率限制（Rate-limit）