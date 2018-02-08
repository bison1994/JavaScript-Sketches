### What
RxJS is a library for composing asynchronous and event-based programs by using observable sequences. It provides one core type, the Observable, satellite types (Observer, Schedulers, Subjects) and operators inspired by Array#extras (map, filter, reduce, every, etc) to allow handling asynchronous events as collections.

- Observable：可观察序列、流（stream）、管道
- operator：流可以转换（map, filter, scan, throttle...）、可以控制流程（delay, takeUntil...）、可以组合（merge, race, zip...）
- 异步操作
- 事件驱动：lodash for events
- 观察者模式：observer 用于订阅 Observable
- 迭代器模式（Iterator pattern）
- 类似 jQuery 的链式调用