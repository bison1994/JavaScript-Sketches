### 基本概念

- Store
  + 管理 state 的容器，提供与 state 交互的 API
- Reducer
  + 修改 state 的纯函数，`reducer: (state, action) => newState`
  + 通过 `dispatch` 一个 `action` 触发 reducer
- Subscribe
  + 订阅者。state 变化时，store 会自动调用相应的订阅方法

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
    this.subscribers.forEach(fn => fn(this.value))
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