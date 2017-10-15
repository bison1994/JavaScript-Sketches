### 异步操作

- 异步操作就是将当前程序从当前执行流中剥离，放入任务队列中。任务队列就是“待办事项列表”，当前执行流结束之后，执行线程才会去查询任务队列，执行相关程序
- 实现异步操作的方式有四种：回调函数、定时器、Promise、Async/Await


### 定时器

- setTimeout 的运行机制
  + 开启一个计时器，然后将指定的代码移出本次执行，等到下一轮 Event Loop 时，再检查计时器是否到了指定时间
  + 如果到了，就执行对应的代码
  + 如果不到，要么就等再下一轮 Event Loop 时重新判断，要么就等时间到了再执行
- 由于同步任务的完成时间是不确定的，因此 setTimeout 指定的任务不一定会严格按照预定时间执行
- `setTimeout(f, 0)` 并不会立即执行 f，而是将 f 添加到消息队列中，在本轮同步任务执行完后再（尽可能早的）执行
- `setTimeout(f, 0)` 的一个作用就是调整事件发生的顺序，另一个作用是将计算量大、耗时的任务（如DOM操作）分块处理


### 传统回调方法的问题


### jQuery中的异步函数

```js
function fetch () {
  var def = $.Deferred();
  setTimeout(function () {
    if (Math.random() > 0.5) {
      console.log('success');
      def.resolve();
    } else {
      console.log('wrong');
      def.reject();
    }
  }, 1000)
  return def;
}

$.when(fetch())
.done (function (data) {
  console.log(data)
})
.fail (function (data) {
  console.log(data)
})
.then (function () {
  console.log('second handler')
})
.then (function () {
  console.log('third handler')
})
```

### Promise

Promise 构建了一个可信任的异步事件处理系统，解决了回调中存在的“竞态”、“不可信任”、“吞噬异常”、“回调地狱”的问题。
由 Promise 指定的异步任务会在本轮 Event Loop 结束末尾执行，而不像其它"正常"的异步任务是在下一轮 Event Loop 中执行

```js
new Promise (function (resolve, reject) {  
  resolve(value); 
  // reject(value); 
}).then((value) => {
  // then 方法的第一个函数参数为成功回调函数
}, (value) => {
  // then 方法的第二个函数参数为失败回调函数
}).then((data) => {
  // data 为上一步操作的返回值
}).catch((data) => {
  // 捕获 reject
})
```

resolve()既可能是完成（fulfill），也可能是拒绝（reject），取决于传入的参数。如果参数为Promise或thenable，则状态由参数的决议值决定，如果是其它直接量，则表示完成。
then 和 catch 方法会创建并返回一个新的Promise，以便于链式调用。
如果发生异常或错误，Promise 状态自动进入 reject，并将错误信息传入相应回调。
创建一个状态为拒绝的 Promise：
Promise.resolve(f()).then((data) => {
  console.log(data);
})

### Generator
	定义一系列状态，使用 yield 分段，返回一个包含各个状态的遍历器对象。

### async/await

async 声明一个异步函数，此函数需要返回一个 Promise 对象。await 等待一个 Promise 对象 resolve 或 reject，并拿到结果，因此无需再写 then 方法。

```js
async function fetch () {
  return new Promise ((resolve, reject) => {
    if (/* success */) {
    resolve(/* data */)
  } else {
    reject(/* data */)
  }
  })
} 
(async function () {
  var result = await fetch();
  /* function will not be invoked until the result is returned */
})();
```
