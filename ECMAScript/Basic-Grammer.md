### 字符集

- `Unicode` 字符集
- 仅支持 16 位 `UTF-16` 编码，不支持 32 位
- ES6 增加了对 32 位的支持

> [Unicode 与 JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html)


### 标识符 Identifier

- 名称：变量名、函数名、对象名或属性名...
- 语法标记：声明变量（var function let const），控制流程（for while if else）...
  + 关键字
  + 保留字


### 操作符/运算符 Operator

- 符号: + - || &&...
- 词语：typeof delete...
- 优先级

### 值 Literal/Value

> 从形式上看，写程序就是用标识符符与操作符来操控值。


### 严格模式

- ES5 引入严格模式，目的是保持向后兼容的前提下，规范某些不确定、不合理、不安全的行为，降低代码解析与执行的容错性，为下一代标准做铺垫

- 可以在脚本顶部或函数内顶部使用 `"use strict";` 编译指示，在全局或局部开启严格模式。考虑到文件合并，一般都在自执行函数顶部使用该指令

- 部分规范项
  + 未使用 `var` 声明的变量不再默认设置为全局变量，而是报错
  + 全局上下文函数中的 `this` 不再指向 `Global`，而是 `undefined`，避免扰乱 `window` 对象的属性
  + 出于安全考虑，禁用 `arguments.callee`、`arguments.caller`、`f.caller`、`f.arguments`
  + 禁用 `with`，目的是性能优化

> 更多参考之 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)

> 更多参考之 [阮一峰](http://www.ruanyifeng.com/blog/2013/01/javascript_strict_mode.html)


### 值的类型

值的类型本质上是一门语言提供的抽象数据类型，值本来本来是没有类型的，因为任何值归根结底都是 bit

- Number
- String
- Boolean
- Null
- Undefined
- Object
  + Function
  + Array
  + Date
  + RegExp
  + Error
  + JSON
  + Promise
  + ...
- Symbol (ES6)

##### Number
- JavaScript 中所有数都用64位浮点数表示，整数也是如此。因此1 === 1.0 // true
- 如同任何使用二进制浮点数的编程语言，JavaScript 中的小数运算也会失去一定的精确性，例如:console.log(0.1 + 0.2) // 0.30000000000000004
- 科学计数法，如：3.14e12。
- 四个特殊的数：Infinity、NaN、Number.MAX_VALUE 和 Number.MIN_VALUE
- NaN表示非数字。之所以设计 NaN 这样一个值，是为了避免程序在本应计算出数值却未得到数值时抛出错误而停止运行。
- 任何有 NaN 参与的运算都会返回NaN；NaN 和任何数都不相等，包括其自身。


##### Boolean
- 当一个表达式需要被解析为布尔值却无法计算出布尔值时，会由系统按如下规则自动转换为真值或假值，使得逻辑运算能够正常进行：
	+ 除了 false、null、" "、0、NaN、undefined 为假值，其它的值均为真值
- 真/假值并不是一种值，它仅仅表示某个值可被解析为 true 或 false


##### Undefined
- undefined 是一个全局变量，表示"值的空缺"或"未定义"（missing value），例如：变量已声明却未初始化（赋值）、未指定返回值时函数的返回值、未指定实参时形参的值、对象属性不存在等。
- 可使用`void 0`来获得 undefined 值。


##### Null
- null 是一个特殊的值："空值"（empty value）。
-	typeof null == object // true，null 并不是对象，但它的数据类型是对象
-	undefined == null // true
-	undefined === null // false


### 值的分类

- 基本类型/原始类型 primitive => 分配于栈内存 => static allocation => immutable
- 引用类型 reference => 分配于堆内存 => dynamic allocation => mutable
  + 构造函数
  + 基本包装类型
    - Number
    - String
    - Boolean
  + 单体内置对象
    - Global
    - Math
    - JSON

> [栈内存与堆内存的解释一](http://net-informations.com/faq/net/stack-heap.htm)

> [栈内存与堆内存的解释二](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)


### 类型检测

- typeof 操作符
  + @return undefined | boolean | number | string | object | function

- Object.prototype.toString().call()
  + @return [[class class]]
  + @example [object Function]

- is*
  + @example Array.isArray()

- instanceof 操作符


### 类型转换

分为隐式转换和显式转换

- Number => String
  + String(num)
  + num.toString()
  + num + ''
  + num.toFixed(n)
- String => Number
  + Number(str)
  + +str | str * 1
  + parseInt(str) | parseFloat(str)


### 变量声明

- ES5 中有两种声明变量的命令：var 和 function

- 变量的声明和赋值是分开的两个步骤，如果只是声明变量而没有赋值，则该变量的值为 undefined。变量的声明是最先被解析的，其次才是赋值，因此在函数中声明的变量会被提前至函数顶部，这被称为变量提升（hoisting）

- 全局变量是全局对象的属性，局部变量是"调用对象"或"声明上下文对象"的属性。变量与作用域紧密相关


### 表达式与运算符

- 表达式 Expression 是可以计算出一个值的短语。任何表达式一定会返回一个值
  + 原始表达式
  + 初始化表达式
  + 函数声明表达式
  + 函数调用表达式
  + 对象创建表达式
  + 属性访问表达式
  + 算数表达式
  + 关系表达式
  + 逻辑表达式
  + 赋值表达式


### 语句

语句，是JavaScript的执行单位。从形式上看，语句就是以分号结尾的一段代码。无论这段代码是表达式，还是别的什么东西，甚至什么都没有，只要以分号结尾，就会被解释器当做语句。

- 条件
- 循环
- 跳转


### 数组

- JavaScript 数组是键名（索引）为有序整数列的特殊的对象
- 创建方式
  + 数组字面量：var arr = [a, b, c] // 自动创建索引属性
  + 构造函数：var arr = new Array(n); // 索引属性尚未创建，需手动创建
  + 构造函数：var arr = new Array('abc', 1, 2, true) // 返回以参数为值的数组
- 数组的索引不一定是连续的，索引非连续的数组被称为稀疏数组
- 数组的 length 属性值总是等于索引最大值+1，length并不一定等于数组元素的个数
- 如果将 length 设置为比当前最大索引小的一个数，那么尾部的元素会被截掉



### 函数

##### 参数

- 参数是函数的局部变量，在函数执行完成后自动销毁
  + parameter 形参：定义函数时设置的参数
  + arguments 实参：调用函数时传入的参数

- 传参是一种隐式赋值。如果传入一个变量，那么实际上传入的是该变量的一个副本：如果变量是基本类型，那么传入的是值的副本；如果变量是引用类型，那么传入的是内存地址的副本

- JavaScript 中函数的形参是非必须的，因为解析器真正解析的是可通过函数内部属性 `arguments` 访问的实参数组。`arguments` 是一个类数组对象，在函数内部可以用 `arguments[i]` 代表参数，`arguments[i]` 与形参本质上是同一个实参值的两个变量。向函数传递实参的个数是不受限的，因为 arguments 对象的长度由传入实参的个数决定，与形参无关


##### Declaration

- 函数声明（语句）

```js
function name () {} // 声明一个变量，定义一个函数，然后将函数关联到此变量
```

- 函数（定义）表达式

```js
function () {} // 常使用赋值语句的形式 var name = function () {}
```

区别：
1. 函数声明类似于变量声明，在作用域中会最先执行（hoist），所以可在函数定义之前调用函数。函数表达式则不可，因为变量的声明执行在先，赋值的执行在后

2. 函数表达式是一种受限的语句，它只能出现在全局作用域或者函数中，而不能出现在语句块中（比如条件和循环）。函数声明则可以出现在任何位置


##### Invoke

- 仅当调用函数时，函数内部的语句才会执行

- 调用函数的方式有四种：作为函数调用、作为方法调用、作为构造函数、call 和 apply 调用

- 函数调用后总会返回一个值，具体返回什么值，与调用的方式有关。作为函数或方法调用，返回值要么是 return 指定的值，要么是 undefined。作为构造函数调用，返回值一定是一个对象

- 函数有两个内部（internal-only）方法：[[call]] 与 [[Construct]]。当函数未被 new 调用时，执行 [[call]] 方法；当函数被 new 调用时，执行 [[Construct]] 方法。带有 [[Construct]] 方法的函数被称为构造函数（constructor）。箭头函数没有 [[Construct]] 方法


#### this

this 指向一个对象，具体指向谁，取决于函数定义和调用的方式，与定义在哪儿无关

1. 作为函数调用，`this` 指向 `global`。但在严格模式中，`this` 为 `undefined`

```js
'use strict';
(function () {
  console.log(typeof this); // undefined
})()
```

2. 作为对象方法调用，`this` 指向调用对象（调用对象会作为一个隐式的实参传给函数）

```js
var a = 1;
var obj = {
  a: 2,
  f: function () {
    // f 作为方法调用，所以 this 指向 obj
    console.log(this.a) // 2
    foo(); // foo 作为普通函数调用，this 指向 global 或 undefined，而不是 obj
    function foo () {
      console.log(this.a) // 1
    }
  }
};
obj.f();
```

3. 作为构造函数调用，执行下列逻辑
  - 创建一个空对象
  - 该对象继承构造函数的 `prototype` 属性
  - 该空对象被赋值给构造函数内部的 `this` 对象
  - 执行构造函数
  - 如果构造函数中显式的 return 了一个对象，那么就返回这个对象（此对象是没有继承原型的，除非返回的是 this）。否则，一律返回新创建的空对象。

4. 间接调用，`this` 由 `call()、apply()` 的第一个参数指定

5. 显式绑定，`this` 由 `bind()` 的参数指定

6. 箭头函数，ES6 定义了箭头函数，采用词法作用域作为 `this` 的指向


### 对象

- 分类
  + 本地对象（内置对象）
    - 单体内置对象：Global、Math、JSON
    - 构造函数（ES5 中有 9 个）
      + 基本包装类型：String、Number、Boolean // 字面量和实例对象不一样
      + Array、Function、RegExp、Object // 字面量和实例都是对象
      + Date、Error // 没有字面量
  + 宿主对象
  + 自定义对象

- 创建
  + 直接量
  + 构造函数
  + `Object.create(proto, [properties])`
  + `Object.create(null)`

- 属性
  + 对象的属性分为自有属性和继承属性
  + 属性值可以是任何 JavaScript 值，以及 getter&setter，因此属性又可分为数值属性和存取器属性（Accessor Property）
	+ 每个数值属性有四个特性：值（value）、可写（writable）、可枚举（enumarable）和可配置（configurable）
  + 每个存取器属性也有四个特性：读取（get）、写入（set）、可枚举（enumarable）和可配置（configurable）
    - `Object.getOwnPropertyDescriptor(object, prop)` 获取对象属性的 descriptor
    - `Object.defineProperty(object, prop, descriptor)` 设置对象属性的 descriptor，返回修改后的对象
  + JavaScript 内置的对象属性是不可枚举的
	+ 将 configurable 设置为 false 的操作是不可逆的，禁止配置的属性将无法再次修改其属性描述符，同时也不可删除
  + 遍历属性
    - `for-in` 语句会遍历对象所有可枚举的属性（包括原型链上可枚举的属性）
    - `Object.keys(obj)` 返回 obj 所有可枚举的自有属性的键名数组
    - `Object.getOwnPropertyNames(obj)` 返回 obj 所有自有属性的键名数组
	+ 检测属性
    - `in` 查询所有属性，包括原型链，包括不可枚举属性
    - `hasOwnProperty()` 检测属性是否为自有属性
    - `propertyIsEnumerable()` 检测属性是否为可枚举的自有属性

- 复制
  + 考虑特殊类型
  + 递归复制考虑环
  + JSON 克隆的局限性
  + 考虑是否克隆 \_\_proto\_\_

- 扩展性
  + 禁止扩展 `Object.preventExtensions(object)`
  + 封闭 `Object.seal(object)`
  + 冻结 `Object.freeze(object)`

- [原型链](https://github.com/bison1994/JavaScript-Sketches/blob/master/ECMAScript/Advanced%20Topic/Prototype.md)
- [面向对象](https://github.com/bison1994/JavaScript-Sketches/blob/master/ECMAScript/Advanced%20Topic/OOP.md)