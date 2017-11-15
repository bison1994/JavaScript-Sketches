### What

函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数 A 的内部调用函数 B，那么在 A 的调用帧上方，还会形成一个 B 的调用帧。等到 B 运行结束，将结果返回到 A，B 的调用帧才会消失。如果函数 B 内部还调用函数 C，那就还有一个 C 的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）

> [how-does-javascript-actually-work-part-1](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)

### 尾调用

- 尾调用是一种函数调用的优化方式。在函数中 return 一个函数的调用，就称为尾调用。如果尾调用自身，就称为尾递归
- 使用尾调用的好处在于减少调用帧以节省内存
- 使用尾调用，当外部函数运行完毕，引擎就会删除其调用帧，如果不是尾调用而是直接调用，那么外部函数的调用帧仍然会存在，因而造成内存的浪费，在递归函数中，如果保存的调用帧过多，就会导致栈溢出（stack overflow：Maximum call stack size exceeded）
- 尾调用优化的前提是尾调用函数不可引用外层函数的内部变量
- ES6 的尾调用优化只在严格模式下开启
- 纯粹的函数式编程语言没有循环操作命令，所有的循环都用递归实现，这就是为什么尾递归对这些语言极其重要
- 不同 js 环境的最大栈帧数不同，Chrome 大概为一万多，Firefox 为五万多

```js
// 非尾调用
function factorial (n) {
  if (n === 1) return n;
  return n * factorial(n - 1);
}
factorial(12000); // 栈溢出

// 非尾调用（此例引自阮一峰http://www.ruanyifeng.com/blog/2015/04/tail-call.html）
// 原文说这是尾调用优化，但在 Chrome 中运行仍然报溢出错误，即使加上 use strict
function factorial (n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}
factorial(12000, 1); // 栈溢出

// 非尾调用
function factorial (n, total) {
  if (n === 1) return total;
  var a = n - 1;
  var b = n * total;
  return factorial(a, b);
}
factorial(12000, 1); // 栈溢出

// 尾调用
function factorial (n, total) {
  if (n === 1) return total;
  n -= 1;
  total *= n;
  return factorial(n, total);
}
factorial(12000, 1); // Infinity
```