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

### 二叉树实现
```js
class BinaryTree {
  constructor (key, value) {
    this.root = new this.node({
      key: key,
      value: value
    })
  }

  node (options) {
    this.key = options.key;
    this.value = options.value;
    this.parent = options.parent;
    this.left = null;
    this.right = null;
  }

  set (key, value) {
    var node = this.root;
    while (node) {
      if (key === node.key) {
        node.value = value;
        return;
      }
      if (key > node.key) {
        if (node.right) {
          node = node.right;
        } else {
          node.right = new this.node({
            key: key,
            value: value,
            parent: node
          })
          return;
        }
      } else {
        if (node.left) {
          node = node.left;
        } else {
          node.left = new this.node({
            key: key,
            value: value,
            parent: node
          })
        }
      }
    }
  }

  get (key) {
    var node = this.root;
    while (node) {
      if (node.key === key) {
        return node.value;
      }
      node = key > node.key ? node.right : node.left
    }
    return null
  }

  dele (key) {
    var node = this.root;
    while (node) {
      if (node.key === key) {
        if (!node.right) {
          var min = node.left;
        } else {
          var min = this.min(node.right);
          min.left = node.left;
          min.right = node.right;
        }
        if (node.key > node.parent.key) {
          node.parent.right = min;
        } else {
          node.parent.left = min;
        }
        return null
      }
      node = key > node.key ? node.right : node.left
    }
    return null
  }
}

```