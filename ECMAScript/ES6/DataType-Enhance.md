### 字符串

- unicode 扩展：codePointAt、fromCodePoint、at、normalize、正则 u 标志
- 方法扩展
  + 重复：repeat()
  + 确认子串:	includes()、startsWith()、endsWith()，均返回布尔值
  + 补全长度:	padStart()、 padEnd()
  + 正则扩展
- 模板字符串
  + 多行
  + Substitution 变量替换 `${ }`
  + 模板标签函数
  + String.raw()


### 数组
- 扩展运算符

```js
// 参数为数组时代替 apply 方法
var arg = [14, 3, 77];
Math.max.apply(null, arg) // ES5 的写法
Math.max(...arg) // ES6 的写法

// 复制数组
var copy = [...arr]

// 合并数组
[1, 2, ...more1, ...more2]

// 解构赋值
[...rest] = [1, 2, 3] // rest => [1, 2, 3]
[a, ...rest] = [1, 2, 3] // rest => [2, 3]
// 需注意，这种方式下，...rest 只能放在最后一位

// 展开字符串
'hello'.split('') // ES5
[...'hello'] // ES6

// 任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组
[...document.querySelectorAll('div')]

// 数组去重
[...new Set(arr)]
```

- Array.from()
  + 用于将两类对象转为真正的数组：类数组对象和可遍历（iterable）对象

```js
const args = [...arguments]
const args = Array.from(arguments)

// 接收第二个函数参数用于遍历操作
Array.from([1, 2, 3], (value, index) => value * index) => [0, 2, 6]

Array.from({length: 5}, (_, index) => index)

const chunk = (arr, size) =>
  Array.from({length: Math.ceil(arr.length / size)}, (v, i) => arr.slice(i * size, i * size + size))
```

- Array.of()
  + 返回参数值组成的数组
- copyWithin(target, start, end)
  + 在数组内部复制一段值并覆盖指定位置的值
- find(callback)
- findIndex(callback)
- fill()
- entries()
- keys()
- values()
- includes()


### 对象

- 字面量语法形式扩展
  + 属性与方法的简写形式
  + 动态计算属性 `[]`
- 方法扩展
  + Object.is()
  + Object.assign()
  + Object.getPrototypeOf()
  + Object.setPrototypeOf()
  + Object.keys()
  + Object.values()
  + Object.entries()
- super
  + super 是指向当前函数原型的指针
  + super 只能在简写方法中使用


> [30-seconds-of-code](https://github.com/Chalarangelo/30-seconds-of-code)