### 字符集
- Unicode 字符集
- 仅支持 16 位 UTF-16 编码，不支持 32 位
- ES6 增加了对 32 位的支持

参考：[Unicode与JavaScript详解](http://www.ruanyifeng.com/blog/2014/12/unicode.html) by 阮一峰

### 严格模式

### 值的类型
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

### 类型检测
- typeof 操作符
@return undefined | boolean | number | string | object | function

- Object.prototype.toString().call()
@return [[class]]<br>
@example [object Function]

- is*
@example Array.isArray()

- instanceof 操作符

### 类型转换
分为隐式和显式<br>
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
ES5 中有两种声明变量的命令：var 和 function。<br>

变量的声明和赋值是分开的两个步骤，如果只是声明变量而没有赋值，则该变量的值为 undefined。变量的声明是最先被解析的，其次才是赋值，因此在函数中声明的变量会被提前至函数顶部，这被称为声明提前（hoisting）。<br>

全局变量是全局对象的属性，局部变量是"调用对象"或"声明上下文对象"的属性。变量与作用域紧密相关。<br>

参考：[我用了两个月的时间才理解 let](https://zhuanlan.zhihu.com/p/28140450) by 方应杭

### 数组
JavaScript 数组是键名（索引）为有序整数列的特殊的对象。<br>
如果数组中某个索引没有赋值，会自动以 undefined填充。<br>
数组的索引不一定是连续的，索引非连续的数组被称为稀疏数组。<br>
数组的 length 属性值总是等于索引最大值 + 1，length 并不一定等于数组元素的个数。<br>
将 length 设置为比当前最大索引更小的一个数，尾部的元素会被截掉（删除）。

### 函数
#### 参数
- 形参 parameter：定义函数时设置的参数；
- 实参 arguments：调用函数时传入的参数。
参数是函数的局部变量，在函数执行完成后自动销毁。<br>

传参是一种隐式赋值（按值传递）。如果传入一个变量，那么实际上传入的是该变量的一个副本。如果变量是基本类型，那么传入的是值的副本；如果变量是引用类型，那么传入的是内存地址的副本。<br>

JavaScript 中函数的形参是非必须的，因为解析器真正解析的是可通过函数内部属性 arguments 访问的实参数组。arguments 是一个类数组对象，在函数内部可以用 arguments[i] 代表参数，arguments[i] 与形参本质上是同一个实参的两个变量。向函数传递实参的个数是不受限的，因为 arguments 对象的长度由传入实参的个数决定，与形参无关。

#### 函数声明 Declaration
声明函数通常有两种方式：
- 函数声明（语句）
	function name ( ) { } // 声明一个变量，定义一个函数，然后将变量关联到此函数
- 函数（定义）表达式
	function ( ) { } // 常使用赋值语句的形式var name = function ( ) { }

区别：
1. 函数声明类似于变量声明，在作用域中会最先执行（hoist），所以可在函数定义之前调用函数，函数表达式则不可，因为变量的声明执行在先，赋值的执行在后。
2. 函数表达式是一种受限的语句，它只能出现在全局作用域或者其它函数中，而不能出现在其它语句中（比如条件和循环）。函数声明则可以出现在任何位置。


