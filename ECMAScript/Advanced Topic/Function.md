> [参考](https://juejin.im/post/59eff1fb6fb9a044ff30a942)

函数的使用技巧和各类设计模式，主要是基于闭包、高阶函数（以函数为参数或以函数为返回值的函数），并结合 apply 等函数方法实现的。

### call、apply&bind
- call 和 apply 本质上是函数调用的一种方式，可以指定函数调用的上下文，改变函数内部 this 的指向
- bind 用于绑定 this 指向
- call 和 bind 都是基于 apply 的

> [如何实现 bind 方法](https://zhuanlan.zhihu.com/p/25379434)

```js
// example
function hasOwn (obj, key) {
  return Object.prototype.hasOwnProperty.call(obj, key)
}
// 将类数组转化为数组
var newArray = Array.prototype.slice.call(arrayLikeObj)

// 类数组对象借用数组方法
Array.prototype.push.call(arguments, 'value')
Array.prototype.shift.call(arguments) // 获取第一个参数

(function () {
  Array.prototype.forEach.call(arguments, function (val) {
    console.log(val + 1)
  })
})(1, 2, 3) // 输出2, 3, 4

// 数组借用 Math 方法
Math.max.apply(null, arr)

// 借用构造函数
function f (a) {
  this.a = a
}
function foo () {
  f.apply(this, arguments)
  console.log(this.a)
}
new foo('a') // 输出a
```


### 递归 Recursion

```js
// 倒计时
var second = 60
function countdown (btn) {
  if (second == 0) { 
    btn.removeAttribute("disabled")
    btn.value = "获取验证码" 
    second = 60 
  } else { 
    btn.setAttribute("disabled", true) 
    btn.value = second + "秒后重新获取" 
    second -- 
  } 
  setTimeout(function() { 
    countdown(btn)
  }, 1000)
}

// 菲波那切数列
function Fibonacci (n , a1 = 1 , a2 = 1) {
  if ( n <= 1 ) { return a2 }
  return Fibonacci (n - 1, a2, a1 + a2)
}

// 遍历节点树（深度优先遍历）
function walk (node, func) {
  func(node)
  node = node.firstChild
  while (node) {
    walk(node, func)
    node = node.nextSibling
  }
}

// 广度优先遍历
function walk (node, func) {
  func(node)
  node = node.nextSibling
  while (node) {
    walk(node, func)
    node = node.firstChild
  }
}
```


### 柯里化 Currying

- 柯里化就是将多参数函数转化为单参数函数：f(arg1, arg2) => f(arg1)(arg2)
- 柯里化又称部分求值。是指向函数传参后，函数不会立即求值，而是将参数保存在闭包中，等到真正需要求值的时候再求值
- 柯里化是因为 lambda 演算只有一个参数才被发明的，是函数式编程的一个自然结果

```js
f(1) // 仅仅将参数保存起来，除此以外什么都不做
f(1)(2) // 同上
f(1)(2)(3) // 真正执行了

const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length
    ? fn(...args)
    : curry.bind(null, fn, arity, ...args)
```


### uncurrying

一种使用匿名单参数函数来实现多参数函数的方法
uncurrying 的作用是将通过 apply/call 实现对象间互调方法的方式泛化为一种通用形式
Function.prototype.uncurrying = function () {
  var self = this
  return function () {
  var obj = Array.prototype.shift.call(arguments)
    return self.apply(obj, arguments)
  }
}


### 记忆 Memorize

通过缓存，避免重复的计算

```js
// Create a cached version of a pure function
function cached (fn) {
  var cache = Object.create(null)
  return (function cachedFn (str) {
    var hit = cache[str]
    return hit || (cache[str] = fn(str))
  })
}
```

> [memoize-one](https://github.com/alexreardon/memoize-one)


### 惰性载入

惰性载入的基本思想是只在调用的时候创建，而不是在程序初始化的时候创建。

通过替换变量，仅在第一次调用时执行有关逻辑，以后再次调用无须重复执行有关逻辑。


### 函数节流（throttle）

避免过于频繁的执行函数（例如 onscroll、onmousemove、onresize 的回调函数）

```js
_.throttle = function (func, wait) {
  var context, args, result
  var timeout = null
  var previous = 0
  var later = function() {
    previous = _.now()
    timeout = null
    result = func.apply(context, args)
    if (!timeout) context = args = null
  }
  return function () {
    var now = _.now()
    var remaining = wait - (now - previous)
    context = this
    args = arguments
    if (remaining <= 0 || remaining > wait) {
      clearTimeout(timeout)
      timeout = null
      previous = now
      result = func.apply(context, args)
      if (!timeout) context = args = null
    } else if (!timeout) {
      timeout = setTimeout(later, remaining)
    }
    return result
  }
}
```
  
与函数节流类似的是函数防抖（debounce），表示直到某一段时间 t 以后，再执行下一次函数。当 t 设定为常数时，就是函数节流。

```js
_.debounce = function (func, wait, immediate) {
  var timeout, args, context, timestamp, result

  var later = function() {
    var last = _.now() - timestamp

    if (last < wait && last > 0) {
      timeout = setTimeout(later, wait - last)
    } else {
      timeout = null
      if (!immediate) {
        result = func.apply(context, args)
        if (!timeout) context = args = null
      }
    }
  }

  return function() {
    context = this
    args = arguments
    timestamp = _.now()
    var callNow = immediate && !timeout
    if (!timeout) timeout = setTimeout(later, wait)
    if (callNow) {
      result = func.apply(context, args)
      context = args = null
    }

    return result
  }
}
```


### 函数串行

```js
const pipeFunctions = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)))
```