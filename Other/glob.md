### what

- Unix 系统中用于匹配路径的一种简化的正则表达式
- 不同的语言有各自的 glob 实现

### wildcard character

> [js implement](https://www.npmjs.com/package/glob)

- `*` 匹配任意数量的字符
- `？` 匹配 1 个字符
- `[...]` 匹配字符集中的某一个
- `[^...]` | `[!...]` 匹配字符集外的字符
- `!(pattern|pattern|pattern) ` 匹配不属于集合中任一模式的字符
- `?(pattern|pattern|pattern)` 匹配集合中的任一模式 0 次或 1 次
- `+(pattern|pattern|pattern) ` 匹配集合中任一模式至少 1 次
- `*(a|b|c)` 匹配集合中任一模式任意次数（包括 0 次）
- `@(pattern|pat*|pat?erN)` 匹配集合中的某一个模式
- `**` 跨路径匹配任意字符