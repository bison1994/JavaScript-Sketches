### React 的整体思想
- 组件就是函数
- 如果是为了复用，那么组件应该是纯函数，尽可能 stateless
- store 中的数据是 immutable 的，一方面避免可变的共享状态源，另一方面利于回溯
- 使用 container 组件处理逻辑，presentational 组件负责展示
- redux 的三个核心理念
  + 单一数据源
  + read-only store，只能通过 reducer 改变 state
  + reducer should be pure function

### 开发要点
- 使用 propsType 或 typescript 做参数类型校验，一方面可作为组件的描述文档，另一方面可增强可靠性
- 数据与用到它的组件绑定（connect）


> [React 最佳实践](https://github.com/camsong/blog/issues/6) by 会影 from 阿里
> [Finally understand Redux by building your own Store](https://toddmotto.com/redux-typescript-store) by Todd Motto

```js
class Store {
	constructor (reducers = {}) {
		this.state = {};
		this.reducers = reducers;
		this.subscribers = []
	}

	get value () {
		return this.state
	}

	reduce (state, action) {
		var newState = {};
		for (var key in this.reducers) {
      newState[key] = this.reducers[key](state[key], action)
		}
  	return newState
	}

	dispatch (action) {
		this.state = this.reduce(this.state, action);
		this.subscribers.forEach(fn => fn(this.value)
	}

	subscribe (fn) {
		this.subscribers = [...this.subscribers, fn]
	}
}

const initState = {
	todo: [],
	loaded: false
}

function todoReducer (state = initState, action) {
	switch (action.type) {
		case 'ADD_CASE':
			return {
				...state,
				todo: [...state.todo, { content: action.content }]
			}
		default:
			return state
	}
}

const reducers = {
	todo: todoReducer
}

const store = new Store(reducers)
```