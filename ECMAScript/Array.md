### API

- 常规
  + .length
  + Array.isArray(arr)
  + .toString()
- 格式转换
  + .join(divider)
  + Array.from()
- 栈与队列
  + .push()
  + .pop()
  + .shift()
  + .unshift()
- 排序
  + .reverse()
  + .sort()
- 分割与组合
  + .concat()
  + .slice(start, end)
- 删除/替换
  + .splice(start, n, value)
- 查询
  + .indexOf(value,start)
  + .lastIndexOf()
  + .includes(value)
  + .find(function(val, index, arr) {}, thisArg) 返回满足函数的值，如无则返回 undefined
  + .findIndex(function(val, index, arr) {}, thisArg) 返回满足函数的值的索引，如无则返回 -1
- 迭代
  + .every()
  + .some()
  + .filter()
  + .forEach()
  + .map()
  + .entries()
- 归并
  + .reduce(function (prev, cur, index, array) {}, init)
  + .reduceRight()
- 填充
  + .fill(value, start, end)
- 创建
  + Array.of()
- 复制
  + .copyWithin(target, start, end)


### 数组排序

sort 内部采用某种原地排序算法，不同的引擎实现可能不同，例如 v8 采用的是 QuickSort

```js
function compare (v1, v2) {
  if (v1 < v2) {
    return -1
  } else if (v1 > v2) {
    return 1
  } else {
    return 0
  }
}
// 数字排序
var numbers = [4, 2, 5, 1, 3]
numbers.sort(function(a, b) {
  return a - b
})
```

如果没有传 compareFunc，那么默认算法是将元素转化为 string，按 UTF-16 code units values 升序排列

> [sort 与随机排序问题](https://www.h5jun.com/post/array-shuffle.html)


### 迭代方法的参数

```js
// 迭代方法除接收一个迭代器函数外，还接受第二个参数，表示迭代器函数的 this 指向的对象
xxx(function (item, [index], [array]) {
  return sth
}, [thisArg])
```


### 数组操作技巧

```js
// 复制数组（浅复制）
var copies = someArray.slice(0)

// 生成自然数列
var arr = Array(9).fill().map((_, i) => i)
Array.from({length: 5}, (_, index) => index)

// 计算数组中某一个值出现的次数
var countOccurrences = (arr, value) => arr.reduce((pre, next) => next === value ? pre + 1 : pre, 0)

// 找出数组中唯一的值
arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i))

// 乱序
var shuffle = arr => arr.sort(() => Math.random() - 0.5)

// 解析 query
var query = 'name=Peter&age=12'
query.split('&').reduce(function (pre, next) {
  next = next.split('=')
  pre[next[0]] = next[1]
  return pre
}, {})
```
