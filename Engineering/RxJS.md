### What
RxJS is a library for composing asynchronous and event-based programs by using observable sequences. It provides one core type, the Observable, satellite types (Observer, Schedulers, Subjects) and operators inspired by Array#extras (map, filter, reduce, every, etc) to allow handling asynchronous events as collections.
<br>
Observables are **created** using Rx.Observable.create or a creation operator, are **subscribed** to with an Observer, **execute** to deliver next / error / complete notifications to the Observer, and their execution may be **disposed**.
<br>

### What（人话版）
- 加强版的 Promise
- 订阅发布中心（EventEmitter）
- 数据流转换功能

一个应用的整个生命周期就是一连串的事件，事件会产生数据的输入、处理与输出。RxJS 专注于事件与数据流的处理，试图通过一种“Observable stream”的抽象模型来描述一连串的事件并处理相应的数据流

### feature
- Observable：可观察序列、流（stream）、管道、类似 EventEmitter
- observer: simply a set of callbacks
- operator：流可以转换（map, filter, scan, throttle...）、可以控制流程（delay, takeUntil...）、可以组合（merge, race, zip...）
- 异步操作：Observables are able to deliver values either synchronously or asynchronously
- 事件驱动：lodash for events
- 观察者模式：observer 用于订阅 Observable
- 迭代器模式（Iterator pattern）
- 类似 jQuery 的链式调用

### 基本用法
```js
// **create**
var ob = Rx.Observable.create(function subscribe (observer) {
  // **excute**
  observer.next(1)
  if (Math.random() > .5) {
    observer.error(2)
  }
  observer.complete()
})
// **subscribe**
// Observable could be subscribed more than once  
ob.subscribe({
	// Next notifications represent actual data being delivered to an Observer. 
	// Error and Complete may happen only once, and there can only be either one of them.
  next: val => console.log(val),
  error: err => console.log(err),
  complete: () => console.log('done')
})
// or
ob.subscribe(
  val => console.log(val),
  err => console.log(err),
  () => console.log('done')
)
// **dispose**
var subscription = observable.subscribe(x => console.log(x));
subscription.unsubscribe();
```

### Operator
An Operator is a function which creates a new Observable based on the current Observable. This is a pure operation: the previous Observable stays unmodified

```js
// 生成数据流
Rx.Observable.fromEvent(document.getElementById('input'), 'input').map(event => {
	console.log(event.target.value)
})
```

> [Categories of operators](http://reactivex.io/rxjs/manual/overview.html#categories-of-operators)

### 应用一：搜索推荐
```js

```