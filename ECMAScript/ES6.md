# ECMAScript 6 (2015)

### let & const

Block-Level Declarations

### Template Literals

### Arrow Functions

### Spread Operator

### Extended Parameter Handling

### Enhanced Object Properties

### Destructuring Assignment

### Modules

##### export

- 可以输出的内容有：变量、函数和对象
- 不能用 export 直接输出一个值

```js
export m // 直接输出一个变量，错误
export 1 // 直接输出一个值，错误
export { a: a } // 直接输出一个对象，错误

export var m = 1 // 正确
export function f () {} // 正确

var m = 1
function f () {}
export { m, f } // 正确
```

- export default value
  + 用于指定默认输出值，其它模块引入时可自定义引入模块的名称
  + 一个文件中只能使用一次
  + 本质上是输出了一个 default 变量，并将 value 默认赋值给 default

```js
export default 1 // 直接输出一个数值
export default m // 直接输出一个变量
export default function () {} // 直接输出一个函数，即使有函数名，仍输出匿名函数
export default {} // 直接输出一个对象
```


##### import

```js
// 写法一：
// 输入接口必须与 file.js 输出的接口名称一致，大括号不能省（ES7 可能会简化）
import { a, b, c } from "file.js"

// 写法二：
// 引入 file.js 所有的输出值并将其赋给对象 obj
import * as obj from "file.js"
obj.a // 调用 file.js 输出的 a

// 写法三：
// 如果引入目标模块的输出采用 export default 形式
import name from "file.js"
// name 是自定义的一个名字，无须使用大括号，例如
import $ from 'jquery'

// 写法四：
// 加载模块并执行，但不输入任何值
import "file.js"
```

### Promise

- [Promises/A+](https://promisesaplus.com)、[Promises/A+ 中文版](https://juejin.im/post/5b9ce8fe6fb9a05cf3711c98)

- Promise Chain
    + then 方法返回一个新的 promise，因此可以链式调用
    + Promise 链中的 promise 能够向下一个 promise 传递数据
    + 一旦 resolve，所有的 then 都会依次执行，包括 catch 后跟的 then
    + **reject 时，catch 后跟的 then 也会执行!**
    + catch 不会继续传递到后面的 catch，除非在 catch 中 throw error

- [promisify](https://javascript.info/promisify)

### Classes

### Symbol Type

### Iterators & Generators

### Map/Set & WeakMap/WeakSet

### Typed Arrays

### Meta-Programming

- Proxy
- Reflection

### New Built-In Methods

### Enhanced Regular Expression

### Extended Literals

### Internationalization & Localization
