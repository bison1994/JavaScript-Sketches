### Symbol

- Symbol 是一种新的原始数据类型，表示独一无二的值
- 设计该数据类型主要是为了解决对象的属性名冲突的问题
- 定义和访问 Symbol 属性都需要用方括号
- Symbol 属性是公开属性，但不可被遍历，只能通过 Object.getOwnPropertySymbols 或 Reflect.ownKeys 访问

```js
var s = Symbol();
var obj = {
  [s]: 'hello',
  s: 'world'
}
obj[s]; // 'hello'
obj['s']; // 'world'

Object.keys(obj) // ['s']
```