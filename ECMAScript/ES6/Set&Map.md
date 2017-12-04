### Set

- Set 是一种数据结构，类似于数组，但成员都是唯一的
- Set 避免了 `NaN !== NaN` 的问题，确保成员的唯一性
- Set 键名和键值是同一个值

```js
var s1 = new Set()
var s2 = new Set([1, 2, 3])
```

- API
  + 属性
    - size
  + 方法
    - add(value)
    - delete(value)
    - has(value)
		- clear()
		- keys() // 返回遍历器对象
		- values() // 同上
		- entries() // 同上
		- forEach()
- 应用
  + 数组去重

```js
// 方法一
function dedupe(array) {
  return Array.from(new Set(array));
}
// 方法二
let s = new Set([3, 5, 2, 2, 5, 5]);
let unique = [...s];
```

 + 并、交、差

### WeakSet

- 成员只能是对象
- 不可遍历
- API
  + add(value)
  + delete(value)
  + has(value)

```js
const a = [{a: 1}, [2, 3]];
const ws = new WeakSet(a);
```

### Map

- 键可以是各种数据类型的键值对型数据结构
- 突破了原生对象只能以字符串作为键名的限制，提供了更灵活的键值对结构
- 键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。使用对象作为键名，可以解决了同名属性碰撞（clash）的问题
- API
  - 属性
    +	size
  - 方法
    + set(key, value)
    + get(key)
		+ delete(key)
		+ has(key)
		+ clear()
		+ keys()
		+ values()
		+ entries()
		+ forEach()

### WeakMap

只接受对象作为键名的 map