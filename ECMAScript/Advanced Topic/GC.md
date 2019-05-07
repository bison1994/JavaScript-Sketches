# 垃圾回收

### 内存泄露（Memory Leaks）

根本原因：unwanted reference


### 回收算法

**引用计数（reference-counting）**

- 如果一个对象被引用的次数为 0，则清除之
- 问题是无法应对循环引用的情况
- IE6\7 采用该算法

```js
// 循环引用示例
const element = document.getElementById('button')
function onClick(event) {
    element.innerHtml = 'text'
}
element.addEventListener('click', onClick)
```


**标记清除（mark-and-sweep）**

<img src="https://cdn-images-1.medium.com/max/1600/1*TdWDj8PsWRuSbeBE6fe5KA.png">

- 定义一组根对象（roots），标记为 active（即不是 garbage）
- 能够从跟对象递归触达的对象，都标记为 active
- 其它没有标记的对象被自动清除
- 2012 年后几乎所有现代浏览器都采用了该算法

```js
// 标记清除可以解决一部分循环引用的问题
function f() {
  var o = {};
  var o2 = {};
  o.a = o2; // o references o2
  o2.a = o; // o2 references o

  return 'azerty';
}

f();
```

> 显然，所有的全局变量，永远不会被清除


### 导致内存泄露的常见原因

- 意外的全局变量
- 被遗忘的 setInterval
- 忘记清除对 DOM 节点的引用
- 闭包 closure
