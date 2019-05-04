### 单线程 & Task
- JavaScript 是单线程的。即一次只能完成一件任务，如果有多个任务，就必须排队，前面一个任务完成，再执行后面一个任务
- 任务，就是由某个事件引发的一系列操作指令。事件可能是：run script、dispatch click、setTimeout、onload...
- 一旦发生某个事件，该事件就会被推入**事件队列**（schedule a task）
- JS 线程内含一个无限的事件循环，只要当前**执行栈**被清空，就会立即检查事件队列里有没有事件，如果有就执行事件对应的任务，没有就继续轮询检查
- 如果当前执行栈一直没有执行完，就永远不会执行下一个事件，这就是阻塞
- 所有 JS 代码的执行必然是由某个事件引发的，因为 JS 线程只会运行事件队列中事件的对应任务
- 所以说 JS 是一门事件驱动的语言
- 事件队列，也叫任务队列（Task Queque），但为了厘清概念，以下统一使用事件队列


### 同步与异步
- 微观的划分
  + 一个事件会引发许多任务，甚至其他事件
  + 推送到执行栈的任务就是同步任务
  + 推送到事件队列的任务就是异步任务（准确的说，是推送到事件队列的事件所对应的任务就是异步任务）
  + 异步任务通常是一种事件，事件一定会被推送到事件队列异步执行
  + 判断一个任务是不是异步，就看它是不是一种事件型任务
- 宏观的划分
  + 将某些操作（如 I/O）交给其它线程，通过回调事件延后执行相应任务，不阻塞当前执行栈，这种任务就是异步任务


### 事件队列
- 在执行线程外，存在着一个事件队列，又称为消息队列或任务队列
- 事件队列就是一个先进先出的数据结构，可以看作是待办事项列表
- 执行线程将上一个事件对应的的任务执行完毕后，就会立即读取任务队列头部，如果有待执行的事件，那么就执行该事件对应的任务


### Event-loop
- JS 线程内含一个无限的事件循环
- 每个事件循环称为一个 tick，每个 tick 执行一个事件（macroTask）
- 当前执行栈中所有该执行的任务都执行完毕后，就会进入下一个 tick，取出事件队列中的队头事件
- 事件是挨个挨个取的，但任务却不一定挨个挨个的执行，因为任务可能是异步的
- 每个 tick 选择执行哪些任务取决于任务的类型：
  + task
    - 普通的同步任务，同步任务不会被推送到事件队列，而是直接推送到执行栈，按照指令的顺序依次执行，而且是在同一个 tick 中执行
  + microtask
    - microtask 会被添加到事件队列（microTaskQueue）
    - 在同一个 tick 中当所有同步任务执行完之后，主线程会依次取出任务队列中所有的 microtask 并执行
    - 更准确的说，在一个 tick 中，每当同步任务清空后，就会执行一次 microtask checkpoint，并清空所有 microtask，随后还可能有其它同步任务（比如事件冒泡带来的任务）继续添加到当前执行栈
    - 因此 microtask 并不一定都是在 tick 最后执行的，但 tick 最后一定会执行 microtask checkpoint，也就是说 microtask 的执行时机有两种
      + after every callback, as long as no other JavaScript is mid-execution
      + at the end of each task
    - 一个 tick 一定是在清空所有同步任务以及 microtask 后才进入下一个 tick
    - 从执行的时机看，microtask 像是同步任务；从任务队列的角度看，microtask 又像是异步任务
  + macrotask
    - macrotask 也会被添加到事件队列（macroTaskQueue），但不会被当前 tick 执行，而是在之后的某个 tick 中执行
- 浏览器中各种事件的回调函数通常会在事件发生时直接作为同步任务推送到执行栈，任务执行过程中，可能还伴随着浏览器渲染线程的绘制工作

```js
for (macroTask of macroTaskQueue) {
  // 1. Handle current MACRO-TASK
  handleMacroTask();

  // 2. Handle all MICRO-TASK
  for (microTask of microTaskQueue) {
    handleMicroTask(microTask);
  }
}

// 在同步任务间插入 microtask
document.addEventListener('click', function () { console.log(1) }, false)
document.body.addEventListener('click', function () {
  console.log(2);
  Promise.resolve(3).then(res => console.log(res))
  setTimeout(function () { console.log(4) }, 0)
}, false)
// 2
// 3
// 1
// 4
```

### macrotask | microtask
- 常见的 macro task 有 setTimeout、MessageChannel、postMessage、setImmediate
- 常见的 micro task 有 MutationObsever 和 Promise.then


### Node.js 中的 setImmediate 和 process.nextTick
- process.nextTick：microTask
- setImmediate: macroTask

> setImmediate 和 setTimeout 可能存在竞态关系（race condition）

> [参考](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
