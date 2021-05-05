### 正统的类与继承

- 类是对象的定义，而对象是类的实例（Instance）。类不可直接使用，要想使用就必须在内存上生成该类的副本，这个副本就是对象
```java
// 以Java为例：
public class Group { } // 创建一个类
Group a = new Group(); // 实例化一个对象
// 通过继承，子类可以直接从父类获得其所有的属性和方法，继承的实现机制是"复制、拷贝"
public class Child extends Parent { } // 创建一个子类，继承父类的方法
```


### 原型与继承

- 和正统的面向对象语言不同，JavaScript 中不存在正统意义的"类"。原因是 Brendan Eich 在设计 JavaScript 的时候，不希望又重新设计一门面向对象的语言（因为已经有 C++ 和 Java 了），并且他希望把 JavaScript 设计得更简单，因此他借鉴了 Java 的语法，用构造函数代替类，所以 JavaScript 中的对象是通过构造函数创建的
- JavaScript 本身是一个借鉴多种语言，仓促交媾的产物
-	严格的讲，JavaScript 中既不存在类，也不存在实例，尽管这些概念是如此的深入人心，以致于误用起来是那么顺其自然
- 没有类，那么 JavaScript 怎么实现继承呢？ Brendan Eich 为构造函数设置了一个 prototype 属性。这个属性包含一个对象（即"原型对象"），所有实例对象需要共享的属性和方法，都放在这个对象里面；不需要共享的属性和方法，就放在构造函数里面
- 与类继承的"复制、拷贝"不同，原型继承的机制是"引用、关联"
- JavaScript 中所有的对象都是由构造函数生成的，每个对象都共同继承构造函数的原型对象中的属性和方法
- 在对象内部有两种方式访问它继承的原型：
	+ `obj.constructor.prototype`
	+ `obj.__proto__`
- 第一种方法，源于每个对象都拥有一个内置属性 constructor 指向其构造函数
- 第二种方法源于每个对象都有一个内部指针[[prototype]]，直接指向其原型，这个内部指针是不可访问的，但是有些浏览器暴露出一个`__proto__` 属性，用于直接访问原型对象


### 原型链与继承

- 因为所有对象都是由构造函数生成的，也就是说所有对象，都必然继承某个原型，而原型本身也是对象，它也会继承别的原型，如此环环相扣就形成了原型链
- JavaScript 的对象系统是一个类似族谱的树状结构，每一个分支都通过原型链一脉相承
- 基于原型链的继承机制和作用域链的工作机制非常类似：当调用对象的某个属性时，如果对象的自有属性中不存在该属性，那么 JavaScript 引擎就会沿着该对象的原型链向上查找，直到找到第一个匹配的属性名为止
- 原型链总有个尽头吧，就好像 DOM 只有唯一的根元素一样，所有原型链都汇聚到同一个源头，那就是 Object.prototype，它也是一个对象，也有[[prototype]]属性，那么 `Object.prototype.__proto__ === ？`（null）


### 原型的关联规则

- 每个对象的[[prototype]]具体指向谁，或者说每个对象的原型究竟是谁，取决于对象的创建方式
  + 对象直接量的原型对象是 Object.prototype

```js  
var obj = {}; obj.__proto__ === Object.prototype // true
```

  + 通过 `Object.create()` 创建的对象，其原型是由第一个参数指定的对象
  + 通过构造函数创建的对象，其原型即构造函数的原型对象
- 构造函数的 prototype 原型并不都是普通的对象，例如：

```js
typeof Function.prototype // "function"
Array.isArray(Array.prototype) // true
// 但这不重要
```

- 比较特殊的是函数也有继承原型，注意，这里很容易混淆“函数的继承原型”与“函数的prototype原型”，它们是两回事，函数不会从自身的prototype 指向的原型对象中继承任何属性或方法，所有的函数都共同继承一个原型对象，那就是 Function.prototype
- 为什么函数也有继承对象，也有 `__proto__` 属性？这不奇怪，因为函数不过是可调用的对象

```js
Array.__proto__ === Function.prototype // true
Function.__proto__ === Function.prototype // true
Object.__proto__ === Function.prototype // true
var f = function () {}; f.__proto__ === Function.prototype // true
```
