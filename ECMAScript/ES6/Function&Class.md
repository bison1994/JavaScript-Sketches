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
- 没有 arguments 对象，如果在箭头函数中使用 arguments，其实用的是继承自外部函数的 arguments
- 没有 prototype

### class
- class 是一种语法糖
- class 仍然是函数
- 不存在变量提升，声明行为类似于 let
- 所有的方法都是不可枚举的（non-enumerable）
- 除非显式的使用 this，否则属性和方法自动定义在 prototype 中
- 类是一等公民

```js
var methodName = 'sayHello';
class Name {
  constructor (a, b) {
    this.a = a;
    this.b = b;
  }
  doSth () {}

  // 方法之间不能用逗号
  doSthElse () {}

  // 动态计算类名
  [methodName] () {}

  // getter 和 setter
  get value () {}

  set value (val) {}

  // 静态方法，等效于 Name.create = function () {}
  static create() {}
}

// 另一种形式
var Name = class {}

// 只能用 new 实例化一个类
var obj = new Name(1, 2);

// 继承
class ColorPoint extends Point {
  constructor (x, y, color) {
    super(x, y); // 调用父类的 constructor(x, y)
    this.color = color;
  }

  toString() {
    return this.color + ' ' + super.toString(); // 调用父类的toString()
  }
}
```

super 关键字表示父类的构造函数，子类没有自己的 this 对象，而是继承父类的 this 对象。
只有 super 方法才能返回父类实例，子类必须在 constructor 方法中调用 super 方法，才能使用 this，否则新建实例时会报错
