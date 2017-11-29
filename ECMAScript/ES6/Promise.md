### 基本用法

```js
/**
 * new Promise 无法取消
 * new Promise 接收一个函数参数
 * resolve 和 reject 是由语言本身实现的回调函数参数
 */
new Promise(function (resolve, reject) {
  // resolve(data) 状态由 pending => resolved | fulfilled
  // reject(data) 状态由 pending => rejected
  // 状态一旦改变，就不可再改变
})

/* resolve 和 reject 都是 microtask */

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

// 改进版：某些异步操作的方法本身就返回 Promise，那么用起来体验自然更好
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

// API
Promise.prototype.then(resolveCallback, rejectCallback) // 返回一个新的 Promise 实例
Promise.prototype.catch() // 语法糖，等于 .then(null, rejection)
Promise.all()
Promise.race()
Promise.resolve()
Promise.reject()
// 非标准 API
Promise.done()
Promise.finally()
Promise.try()
```

### 自制简易 Promise

```js
// 省略了参数校验
class Promise {
  constructor (func) {
    func(this._resolve.bind(this), this._reject.bind(this));
    this.status = 'pending';
  }

  _resolve (data) {
    if (this.status === 'pending') {
      this.status = 'resolved';
      setTimeout(() => {
        this._resolveCallback(data)
      }, 0)
    }
  }

  _reject (data) {
    if (this.status === 'pending') {
      this.status = 'rejected';
      setTimeout(() => {
        this._rejectCallback(data)
      }, 0)
    }
  }

  then (resolveCallback, rejectCallback) {
    this._resolveCallback = resolveCallback;
    this._rejectCallback = rejectCallback;
    return this // 实际上 then 方法返回值很复杂，这里做了极其粗略的简化
  }

  catch (rejectCallback) {
    return this.then(null, rejectCallback)
  }
}
```