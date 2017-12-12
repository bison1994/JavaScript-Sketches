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

[参考](https://github.com/bison1994/JavaScript-Sketches/blob/master/ECMAScript/ES6/Promise.md)


### Generator

[参考](https://github.com/bison1994/JavaScript-Sketches/blob/master/ECMAScript/ES6/Iterators%26Generators.md)


### async/await

[参考](https://github.com/bison1994/JavaScript-Sketches/blob/master/ECMAScript/ES6/Async.md)


### ReactiveX