### js 运行的基本模式

<img src="https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop/the_javascript_runtime_environment_example.svg">

1. Event loop。就是一个无限循环，该循环不断从消息队列（message queque、task queque）中取出事件，并将其 listener 函数入栈，栈清空后，再取下一个事件，直到栈和消息队列均被清空
2. Adding messages。比如点击按钮，如果点击事件绑定了 listener 函数，则该事件被立即推入消息队列。事件循环会从队列取出该事件，并将其 listener 函数入栈，栈内的函数会被立即执行
3. Run-to-completion。栈内的函数会一直执行直到清空，没清空前，其他事件消息只能在队列中等待，或者被延迟加入队列。比如在一个 2s 死循环期间发生的点击事件，会被推迟到两秒后执行

可见 JS 是一门事件驱动的语言，因为所有的代码执行都由某个事件（或称为消息）触发，事件包括：run script、dispatch click、setTimeout、onload...

JS 是非阻塞的，其实是说大多数耗时的 API 被设计成了异步模式（当然也有部分 API 是阻塞式的）。比如网络请求，发出请求是一个事件，事件循环取出该事件，会将其交给别的进程或线程执行。请求响应又是一个事件，当请求返回时，该事件被推入消息队列，然后被事件循环在某一轮 tick 中取出，相应的 listener 也即请求的回调函数被推入栈，然后执行

setTimeout 这个 API 的含义是说：x 秒后把该事件推入消息队列。当然，真正推的时候，可能栈还没有清空，或者消息队列还有事件在排队，就会导致 setTimeout 的回调函数执行时机延后

同步和异步的区分，本质就是看是否阻塞，阻塞的本质，可以理解为是否“霸占” stack


### Event-loop

<img src="https://blog-assets.risingstack.com/2016/10/the-Node-js-event-loop.png">

- 每个循环称为一个 tick，每个 tick 从消息队列里取一个事件 & 执行一个任务（macroTask）
- 当前执行栈清空后，就会进入下一个 tick，继续取消息队列中的队头事件
- 两类任务：
  + macrotask
    - macrotask 会被添加到宏任务队列（macroTaskQueue），排队等候被某一轮 tick 选中执行
    - 常见的 macrotask 有 setTimeout、MessageChannel、postMessage、setImmediate
  + microtask
    - microtask 会被添加到微任务队列（microTaskQueue）
    - 常见的 micro task 有 MutationObsever 和 Promise.then，还可以用 queueMicrotask 添加微任务
    - 在同一个 tick 中当宏任务的代码执行完之后，主线程会依次取出所有的 microtask 并执行
    - 一个 tick 一定是在清空所有同步任务以及 microtask 后才进入下一个 tick
    - 与宏任务的两个区别
      - 一轮 tick 内微任务可以执行多个
      - 同一轮 tick 中追加的微任务也会被执行
- Node.js 中的事件循环，和浏览器内的事件循环，是两套实现。不同浏览器的实现也有区别
  + [Understanding the Node.js event loop phases and how it executes the JavaScript code](https://dev.to/lunaticmonk/understanding-the-node-js-event-loop-phases-and-how-it-executes-the-javascript-code-1j9)
  + [从 JS 引擎到 JS 运行时 - libuv Event Loop](https://zhuanlan.zhihu.com/p/104501929)
  + [tasks-microtasks-queues-and-schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)

```js
for (macroTask of macroTaskQueue) {
  // 1. Handle current MACRO-TASK
  handleMacroTask()

  // 2. Handle all MICRO-TASK
  for (microTask of microTaskQueue) {
    handleMicroTask(microTask)
  }
}

// 在两个宏任务间插入 microtask
document.addEventListener('click', function () { console.log(1) }, false)
document.body.addEventListener('click', function () {
  console.log(2)
  Promise.resolve(3).then(res => console.log(res))
  setTimeout(function () { console.log(4) }, 0)
}, false)
// 2
// 3
// 1
// 4
```
