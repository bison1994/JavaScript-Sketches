### 基本概念

- 类
  + 对实体的建模
  + 一组关联的属性和行为的集合
  + 一种程序构建的基础结构
  + 生成对象的模板

- 对象
  + 可重复的从类独立“复制”出的数据结构，也成为类的实例 

- 抽象类
  + 抽象类，又称父类、超类，是对类的进一步抽象。抽象类只能被类继承，不能被实例化。
  + 抽象类、类和对象构成了程序描述的三级分类体系。

- 接口
  + 对类的抽象描述/设计需求：约定一种类应该实现什么

- 封装
  + 逻辑的抽象/内聚
  + 确保私有状态不被外部修改，这样内部才能放心修改，否则就叫“破坏了封装性” 

- 继承
  + 继承是指一个类继承另一个类，一定是父类与子类间的关系，而不是类与对象或对象与对象之间的关系
  + 实例对象“继承”了类的方法，这里的“继承”是另一个概念，勿混淆

- 多态
	+ 一种行为在不同场景下有不同的实现/表达
	+ 实现方式：父类的通用行为可以被子类用更特殊的行为重写


### 封装

##### 直接用对象会有什么问题？

```js
var parent = {
  a: 'a',
  obj: { value: 1 },
  foo: function () { console.log(this.a) }
}
var descendant = ?

// “拷贝式”、“混合式”继承
var descendant = $.extend(parent, { b: 'b' })
var descendant = Object.assign({ b: 'b' }, parent)
```

另一种“混合式”继承

```js
function Composition (target, source) {
  var desc  = Object.getOwnPropertyDescriptor
  var prop  = Object.getOwnPropertyNames
  var def = Object.defineProperty

  prop(source).forEach(
    function (key) {
      def(target, key, desc(source, key))
    }
  )
  return target
}
```

> 上述操作当然没问题，但是，它们不属于“面向对象”编程的范畴


##### 正统方式：构造函数  + prototype

- JavaScript 使用函数构造类（class）
- 使用原型，是为了避免通用的方法在每个实例对象中都拷贝一份，将通用属性或方法定义在原型对象中，所有实例均引用同一个原型
- 父类的方法如果不希望被继承，则应该定义为静态方法

```js
function Parent () {}
Parent.f = function () {} // 静态方法
```


### 继承

```js
function Parent () {
  this.a = 'a'
}
Parent.prototype = {
  foo: function () { console.log(this.a) }
}

function Descendant () {
  Parent.apply(this, arguments)
  this.b = 'b'
}
Descendant.prototype = Object.assign(Parent.prototype, { constructor: Descendant })
// or
Descendant.prototype = Object.create(Parent.prototype, { 
  constructor: {
    value: Descendant,
    enumerable: true,
    writable: true,
    configurable: true
  }
})

new Descendant()
```
