> [参考](https://promisesaplus.com)

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
  // 状态一旦改变，就不可再改变
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

```js
new Promise(res => res(1))
.then(val => {
  console.log(val);
  return val + 1;
})
.then (val => console.log(val))
// 1
// 2
```

### 自制简易 Promise

```js
// 省略了参数校验
class Promise {
  constructor (func) {
    func(this._resolve.bind(this), this._reject.bind(this));
    this.status = 'pending';
    this._resolveCallback = [];
    this._rejectCallback = [];
    this.resolve = function (data) {
      if (typeof data.then === 'function') {
        var promise = new Promise(data.then);
        promise.status = 'fulfilled';
        return promise
      } else {
        return new Promise(resolve => resolve(data))
      }
    }
    this.reject = function (data) {
      if (typeof data.then === 'function') {
        var promise = new Promise(data.then);
        promise.status = 'rejected';
        return promise
      } else {
        return new Promise((_, reject)  => reject(data))
      }
    }
  }

  _resolve (data) {
    if (this.status === 'rejected') return;
    this.status = 'fulfilled';
    // 应该是 microtask，这里仅用 setTimeout 作简化模拟
    setTimeout(() => {
      this._resolveCallback.forEach(func => setTimeout(() => {
        func(data);
      }, 0))
    })
  }

  _reject (data) {
    if (this.status === 'fulfilled') return;
    this.status = 'rejected';
    setTimeout(() => {
      this._rejectCallback.forEach(func => setTimeout(() => {
        func(data);
      }, 0))
    }, 0)
  }

  then (resolveCallback, rejectCallback) {
    if (resolveCallback) {
      this._resolveCallback.push(resolveCallback);
    }
    if (rejectCallback) {
      this._rejectCallback.push(rejectCallback);
    }
    return this // 实际上 then 方法返回值很复杂，这里做了极其粗略的简化
  }

  catch (rejectCallback) {
    return this.then(null, rejectCallback)
  }
}
```