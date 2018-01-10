### 符号表
- 实现符号表的关键是查找
- 链表查找的问题是效率很低，平均需要 N/2 次比较
- 数组查找的问题是虽然查找的效率提高到了对数级，但需要保证有序性，且插入的操作由于涉及数据的位移，效率仍然很低
- 二叉树本质上是链表与数组二分查找的结合，查找快，插入也快
- 二叉树极端情况下可能退化为链表，解决此问题的办法是使用平衡二叉树

### 二分查找

```js
function binarySearch (arr, key, lo, hi) {
  if (lo > hi) return lo;
  var mid = lo + Math.floor((hi - lo) / 2);
  var k = arr[mid];
  if (key > k) {
    return binarySearch (arr, key, mid + 1, hi)
  } else if (key < k) {
    return binarySearch (arr, key, lo, mid - 1)
  } else {
    return mid
  }
}

function binarySearch (arr, key) {
  var lo = 0;
  var hi = arr.length - 1;
  while (hi > lo) {
    var mid = lo + Math.floor((hi - lo) / 2);
    var k = arr[mid];
    if (key > k) {
      lo = mid + 1
    } else if (key < k) {
      hi = mid - 1;
    } else {
      return mid
    }
  }
  return lo
}
```


### 二叉树
```js
class Node {
  constructor (key, value, left = null, right = null) {
    this.key = key;
    this.value = value;
    this.left = left;
    this.right = right;
  }
}
class Binary {
  constructor () {
    this.root = null;
  }

  put (key, value) {
    this.root = this._put(this.root, key, value)
  }

  _put (node, key, value) {
    if (!node) return new Node(key, value);
    if (key > node.key) {
      node.right = this._put(node.right, key, value)
    } else if (key < node.key) {
      node.left = this._put(node.left, key, value)
    } else {
      node.value = value
    }
    return node
  }

  get (key) {
    return this._get(this.root, key)
  }

  _get (node, key) {
    if (!node) return null;
    var k = node.key;
    if (key > k) {
      return this._get(node.right, key)
    } else if (key < k) {
      return this._get(node.left, key)
    } else {
      return node.value
    }
  }
}

```