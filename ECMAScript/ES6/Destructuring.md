### what

一种创建并为变量赋值的方便机制

### Why
for Easier Data Access


### how

```js
let obj = {a: 0, b: 1}
```

```js
let {a, b} = obj
// 假设 obj 中不存在属性 b，则 b 的值初始化为 undefined
```

```js
let a, b;
{a, b} = obj // 该赋值表达式返回一个值，就是 obj
```

```js
// 别名
let {a: _a, b: _b} = obj
```

```js
// 嵌套
let obj = {
  a: {
    b: 1
  }
}
let {a: {b}} = obj
```

```js
// 数组
let arr = [1, 2, 3];
let [a, b] = arr;
let [, , c] = arr; 
// 类似于对象，数组解构同样可以嵌套、设置默认值
```

```js
let arr = [1, 2, 3];
let [a, ...rest] = arr;
// rest => [2, 3]
```

```js
// 参数解构
function foo (a, b, {c, d}) {};
foo(1, 2, {c: 3, d: 4});
```
