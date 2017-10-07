Symbol是一种新的原始数据类型，表示独一无二的值。设计该数据类型是为了解决对象的属性名冲突的问题。
使用Symbol定义和访问属性都需要用方括号
var s = Symbol();
var obj = {
  [s]: ‘hello’
}
obj[s]; // ‘hello’
Symbol属性是公开属性，但不可被遍历，只能通过Object.getOwnPropertySymbols或Reflect.ownKeys访问。
