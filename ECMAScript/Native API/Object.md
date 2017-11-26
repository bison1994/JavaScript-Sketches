- 构造函数
  + .constructor
- 原型
  + .__proto__
  + Object.getPrototypeOf()
  + Object.setPrototypeOf()
  + .isPrototypeOf()
- 数据属性
  + Object.keys(obj) 以数组形式返回可枚举自有属性
  + Object.getOwnPropertyNames(obj) 以数组形式返回所有自有属性
  + Object.hasOwnProperty() 判断某个属性是否为自有属性
  + .propertyIsEnumerable() 判断某个属性是否可枚举
- 描述属性
  + Object.getOwnPropertyDescriptor()
  + Object.defineProperties()
  + Object.defineProperty()
- 扩展性
  + Object.preventExtensions() => Object.isExtensible()
  + Object.seal() => Object.isSealed()
  + Object.freeze() => Object.isFrozen()
- 创建
  + Object.create()
- 拆封取值
  + .valueOf() 返回包装对象的原始值，如果对象没有原始值，则 valueOf 将返回对象本身。一般由 JS 内部调用
- 检测 
  + .toString()
  + .toLocaleString()
- ES6
  + Object.assign(target, ...sources)
  + Object.is(val1, val2)