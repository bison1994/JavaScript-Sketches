### 字符集

- `Unicode` 字符集
- 仅支持 16 位 `UTF-16` 编码，不支持 32 位
- ES6 增加了对 32 位的支持

> [Unicode 与 JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html)


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


### 值的分类

- 基本类型 primitive => 分配于栈内存 => static allocation
- 引用类型 reference => 分配于堆内存 => dynamic allocation
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
略


### 流程控制语句
略


### 数组
略


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


#### this

this 指向一个对象，具体指向谁，取决于函数定义和调用的方式

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

- 创建
  + 直接量
  + 构造函数
  + `Object.create(proto, [properties])`
  + `Object.create(null)`

- 属性
  + 对象的属性分为自有属性和继承属性。属性值可以是任何 JavaScript 值，以及 getter&setter，因此属性又可分为数值属性和存取器属性
	+ 每个数值属性有四个特性：值（value）、可写（writable）、可枚举（enumarable）和可配置（configurable），每个存取器属性也有四个特性：读取（get）、写入（set）、可枚举（enumarable）和可配置（configurable）
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

- [原型链]()
- [面向对象]()
  