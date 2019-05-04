> [promise 规范](https://promisesaplus.com)、[promise 规范中文版](https://juejin.im/post/5b9ce8fe6fb9a05cf3711c98)

### 基本用法

```js
/**
 * new Promise 无法取消
 * new Promise 接收一个函数参数
 * resolve 和 reject 是由语言本身实现的回调函数参数
 */
new Promise(function (resolve, reject) {
  // resolve(data) 内部状态 [[PromiseState]] 由 pending => fulfilled
  // reject(data) 内部状态由 pending => rejected
  // 状态一旦改变，就不可再改变，任何一个 Promise 只会返回两种状态之一
})

// 比回调嵌套更“优雅”的语法
new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1)
  }, 1000)
})
.then(data => {
  console.log(data)
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(data + 1)
    }, 1000)
  })
})
.then(data => {
  console.log(data)
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(data + 1)
    }, 1000)
  })
})
.then(data => {
  console.log(data)
})

// 改进版：某些异步操作的方法本身就返回 Promise，用起来体验会更好
function fetch (data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(data + 1)
    }, 1000)
  })
}

fetch(0)
.then(data => {
  console.log(data);
  return fetch(data);
})
.then(data => {
  console.log(data);
  return fetch(data);
})
.then(data => {
  console.log(data);
})

// 对同一个 promise 可以添加多个 then 方法
var p = fetch(1);
p.then(data => console.log(data))
p.then(data => console.log(data))
p.then(data => console.log(data))
```
> resolve 和 reject 都是 microtask

```js
// API
Promise.prototype.then(resolveCallback, rejectCallback) // 返回一个新的 Promise 实例
Promise.prototype.catch() // 语法糖，等于 .then(null, rejection)
Promise.all()
Promise.race()
// 以下两个方法用于处理非 promise 的 thenable，目的是向后兼容
Promise.resolve(data) // 返回一个状态为 fulfilled 的 promise
Promise.reject(data) // 返回一个状态为 rejected 的 promise
// 非标准 API
Promise.done()
Promise.finally()
Promise.try()
```

### Promise Chain

- then 方法返回一个新的 promise，因此可以链式调用
- Promise 链中的 promise 能够向下一个 promise 传递数据
- 一旦 resolve，所有的 then 都会依次执行，不会阻断，包括 catch 后跟的 then
- **reject 时，catch 后跟的 then 也会执行!**
- catch 不会继续传递到后面的 catch，除非在 catch 中 throw error

```js
new Promise(res => res(1))
  .then(val => {
    console.log(val);
    return val + 1;
  })
  .then (val => console.log(val))
// 1
// 2

new Promise(resolve => resolve(1))
 .then(() => console.log(1))
 .catch(() => console.log(2))
 .then(() => console.log(3))
 .catch(() => console.log(4))
// 1
// 3

new Promise((_, reject) => reject(1))
 .then(() => console.log(1))
 .catch(() => console.log(2))
 .then(() => console.log(3))
 .catch(() => console.log(4))
// 2
// 3
```
