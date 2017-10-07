### 函数参数

- 默认参数
  + 默认值仅当未提供实参或传入 undefined 的情况下才会被使用
  + 默认参数本质上是函数在内部自动使用 let 初始化变量
- rest 参数
  + rest 变量是一个真正的数组
- 上述方法在 Function 构造函数中同样有效

### name 属性
每个函数都增加了 name 属性，便于显示调试信息

### 内部属性 new.target
[[Construct]] 方法被调用，new 操作符调用的目标（通常为构造函数本身）将被赋给 `new.target`；如果 [[call]] 被执行，那么 `new.target` 的值为 undefined

### 块级函数
可以在代码块中用 function 声明函数，函数仅在块中有效，存在变量提升

### 箭头函数
- this 由词法作用域而定，即函数定义时所在的对象，是固定不变的
- 不可以当作构造函数
- 没有 arguments 对象，如果在箭头函数中使用 arguments，其实用的是包含函数的 arguments
- 没有 prototype


### class
class是一种语法糖
class仍然是函数
不存在变量提升
class Name {
  constructor (a, b) {
  	this.a = a;
	this.b = b;
  }
  // 除非显式的使用this，否则属性和方法自动定义在prototype中
  doSth ( ) { }
  // 方法之间不能用逗号
  doSthElse ( ) { }
}
// 实例化一个对象
var obj = new Name(1, 2);

继承
class ColorPoint extends Point {
  constructor (x, y, color) {
    super(x, y); // 调用父类的constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
	super关键字表示父类的构造函数，用来新建父类的this对象。子类必须在constructor方法中调用super方法，否则新建实例时会报错。因为子类没有自己的this对象，而是继承父类的this对象。只有super方法才能返回父类实例，因此在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。
