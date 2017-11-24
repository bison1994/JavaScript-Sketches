### Proxy

Proxy 是一个全局的构造函数，`new Proxy(target, handler)`，用于拦截对目标的某些操作，增强了 JavaScript 元编程的特征
<br>
target 可以是对象，也可以是函数

```js
// ES5 的写法
var obj = {
  get value () {
    return this._value
  },
  set value (val) {
    this._value = val;
  }
}

var obj = Object.defineProperty({}, 'value', {
  get: function () {
    return this._value
  },
  set: function (val) {
    console.log(val);
    this._value = val
  }
})

// ES6 的写法
var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(key);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(value);
    return Reflect.set(target, key, value, receiver);
  }
})
```

拦截操作包括：

```js
get(target, propKey, receiver)           // 赋值
set(target, propKey, value, receiver)    // 取值
has(target, propKey)                     // 查找
apply(target, object, args)              // 函数调用
construct(target, args)                  // 实例化构造函数
deleteProperty(target, propKey)
ownKeys(target)
getOwnPropertyDescriptor(target, propKey)
defineProperty(target, propKey, propDesc)
preventExtensions(target)
getPrototypeOf(target)
isExtensible(target)
setPrototypeOf(target, proto)
```


### Reflect

Reflect 是一个全局对象，专门用于部署语言内部的一些方法，增强了 JavaScript 函数式的特征

```js
Reflect.apply(target, thisArg, args)
Reflect.construct(target, args)
Reflect.get(target, name, receiver)
Reflect.set(target, name, value, receiver)
Reflect.defineProperty(target, name, desc)
Reflect.deleteProperty(target, name)
Reflect.has(target, name)
Reflect.ownKeys(target)
Reflect.isExtensible(target)
Reflect.preventExtensions(target)
Reflect.getOwnPropertyDescriptor(target, name)
Reflect.getPrototypeOf(target)
Reflect.setPrototypeOf(target, prototype)
```