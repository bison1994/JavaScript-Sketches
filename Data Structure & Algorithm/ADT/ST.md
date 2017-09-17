### 符号表
符号表就是键值对，它的两个核心操作分别是设置 set 与读取 get。实现符号表 API 的关键是查找。

### API
- set(key, value)
- get(key)
- delete(key)
- size()

有序符号表
- min() 最小键
- max() 最大键
- floor(key) 小于等于 key 的最大键
- ceil(key) 大于等于 key 的最小键
- rank(key) 小于 key 的键的数量
- select(k) 排名为 k 的键
- keys() 返回已排序的键
- keys(lo, hi)
- size(lo, hi)