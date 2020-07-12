## ES6 modules in Browsers

### module

A module is just a file. One script is one module. As simple as that.

### 使用方式

```html
<script type="module">
  import { x } from './xx.js'
</script>

<script type="module" src="./app.js"></script>

<script type="module">
  import x from 'https://xxx'
</script>

<script type="module">
  alert(import.meta) // information about the current module
</script>

<!-- all dependencies are fetched (analytics.js), and the script runs -->
<!-- doesn't wait for the document or other <script> tags -->
<script async type="module">
  import { report } from './analytics.js'
  report()
</script>
```

### how es module work

- Construction：按照依赖关系图，找到 import、下载文件、将文件解析为 module record
  - 为什么不支持 import xx from `${path}/xx.js`，因为在构建阶段，文件没有被执行，不知道 path 是啥。可通过 import() 解决此问题
  - 在构建阶段，浏览器会用一个 map 记录模块的加载状态，保证每个模块仅被加载和执行一次
  - 加载
    - Modules work only via HTTP(s), not in local files
    - External scripts from another origin require CORS headers
    - Module scripts are always **deferred**, same effect as defer attribute
    - Inline scripts are also deferred
  - 解析
    - Always “use strict” (so top-level `this` is undefined.)
    - Each module has its own top-level scope
    - 解析的产物 module record 包含 AST、import entries 等模块信息

- Instantiation：为所有的 export 分配内存（此时不会填充值，只是找到位置），同时将 import 和对应的 export 做绑定/关联
  - live bindings：import 和 export 的变量是同一个引用

- Evaluation：执行 top-level code，获得实际的 export 值，并填充到内存

> 模块的加载和解析，分别由 HTML spec 和 ES module spec 两个规范描述


### 循环依赖的场景

```js
// a.js
import { b } from './b.js'
console.log("a: " + b)
export var a = 'a'

// b.js
import { a } from './a.js'
console.log("b: " + a)
export var b = 'b'
setTimeout(() => console.log("b: " + a), 0)

// 执行 a.js，依次打印
b: undefined
a: b
b: a
```

### 参考

> [https://javascript.info/modules-intro](https://javascript.info/modules-intro)

> [https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive/)
