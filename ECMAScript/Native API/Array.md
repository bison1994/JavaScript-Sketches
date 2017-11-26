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

### 比较函数的基本格式
```js
function compare(v1, v2){
  if (v1 < v2) {
    return -1;
  } else if (v1 > v2) {
    return 1;
  } else {
    return 0;
  }
}
```


### 迭代方法的函数参数
```js
function (item, index, array) {
  return sth;
}

// 迭代方法除接收一个迭代器函数外，还接受第二个参数，表示迭代器函数的 this 指向的对象
```


### 数组操作的一些技巧
```js
// 复制数组（浅复制）
var copies = someArray.slice(0);

// 生成自然数列
var arr = Array(9).fill().map((_, i) => i);
```