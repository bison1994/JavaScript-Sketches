### 客户端储存的应用场景

- 离线化
- 缓存某些资源
- 前后端通信
- 跨页面数据共享

### Cookie
- session cookie：会话结束即清除
- persistent cookie：超过设定时间（expire）即清除
- Cookie 的局限
  + cookie 只能储存单一格式的数据
  + 每个域名下可设置的 cookie 总数是有限的，一般不超过 50 个
  + 每个 cookie 的大小有限，一般不超过 4kb
  + 设置和操作 cookie 比较麻烦，需要自行封装方法

### Web Storage
- Web Storage 一定程度上弥补了 cookie 的局限性，提供了一种容量更大且允许跨会话存在的储存机制
- storage 是一个引用类，它有两个实例：sessionStorage 和 localStorage
- 一般可储存容量小于 5MB，移动端可能更小。稳妥起见，一般不要超过 2MB
- 当储存容量达到上限，继续添加数据会报错（不会执行写入操作）。需要手动清理空间大小
- API
  + .clear() 删除所有值
  + .key(index)	获取 index 处的值的名字
  + .getItem(name) 根据 name 获取相应的值
  + .setItem(name, value)	为指定的 name 设置一个值，或者用于设置新的名值对
  + .removeItem(name)	删除由 name 指定的名值对

### IndexedDB
- IndexedDB 是在浏览器中保存结构化数据的一种数据库
- 它采用对象储存数据，而不是用表
- 它也受同源限制。大小一般不超过5MB