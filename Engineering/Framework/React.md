### React 的整体思想
- 组件就是函数
- 如果是为了复用，那么组件应该是纯函数，尽可能 stateless
- store 中的数据是 immutable 的，一方面避免可变的共享状态源，另一方面利于回溯
- 使用 container 组件处理逻辑，presentational 组件负责展示
- redux 的三个核心理念
  + 单一数据源
  + read-only store，只能通过 reducer 改变 state
  + reducer should be pure function

### 组件通信
- 父子组件
- 状态提升
- 订阅发布
- 状态管理


### 生命周期
- Mounting
  + constructor()
  + componentWillMount()
  + render()
  + componentDidMount()
- Updating
  + componentWillReceiveProps()
  + shouldComponentUpdate()
  + componentWillUpdate()
  + render()
  + componentDidUpdate()
- Unmounting
  + componentWillUnmount()


### 最佳实践
- shouldComponentUpdate
- 使用 Immutable Data Structures
- 使用 propsType 或 typescript 做参数类型校验，一方面可作为组件的描述文档，另一方面可增强可靠性
- 数据与用到它的组件绑定（connect）
- 组件粒度尽量小，除了灵活可复用外，还有利于页面更新的性能

> [React 最佳实践](https://github.com/camsong/blog/issues/6) by 会影 from 阿里


### Immutable
- 理念
  + 共享的可变状态是万恶之源
	+ 函数式编程对纯函数的追求
	+ 尽管引用类型十分美妙，但是“伴随着特权，随之而来的是更大的责任” （With great power comes great responsibility）
- 优势
  + 管理复杂度，使数据变化更可控
	+ Undo/Redo/Time travel
	+ 利于追踪变化和测试
	+ 拥抱函数式编程
- 问题
  + 深复制耗内存
	+ 嵌套越多，层级越深，修改起来很麻烦
- 办法
  + Immutable.js
	+ seamless-immutable
- 原理
  + Persistent Data Structure（持久化数据结构）
  + Structural Sharing（结构共享）

> [参考](https://zhuanlan.zhihu.com/p/20295971)