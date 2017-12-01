### Iterator
- 为各种数据结构创建遍历接口
- 主要供 `for of` 语法使用，只要部署了 `Symbol.iterator`，就可以用 `for of` 遍历
- 默认的 Iterator 接口部署在数据结构的 `Symbol.iterator` 属性，一个数据结构只要具有 `Symbol.iterator` 属性，就可以认为是“可遍历的”（iterable）
- 遍历器必须部署 next 方法，还可以选择部署 return 方法（用于循环语句中的 break 、continue 或 throw error 情形）和 throw 方法（配合 Generator 使用）
- 原生具备 Iterator 接口的数据结构
  + Array
  + Map
  + Set
  + String
  + TypedArray
  + 函数的 arguments 对象
  + NodeList 对象

```js
// 基本形式
const obj = {
  [Symbol.iterator]: function () {
    return {
      next: function () {
        return condition 
          ? {
            value: ...
          }
          : {
            done: true
          }
      }
    }
  }
}
// for of 循环会自动调用 `obj[Symbol.iterator]().next` 方法

// 采用 generator
const obj = {
  * [Symbol.iterator]() {
    yield 'hello';
    yield 'world';
  }
}
```

### Generator
- Generator 函数是一个状态机
- 它返回一个遍历器接口

```js
function* gen () {
  yield 1;
  yield 2;
  yield 3;
  return false
}
var it = gen();
var value;
while (value = it.next().value) {
  console.log(value)
}
// for of 循环
for (let value of it) {
  console.log(value)
}

// 向 next 传值可以向函数内注入数据
function* gen () {
  var x = yield 1;
  var y = yield x + 1;
  var z = yield x + y + 1;
}
var it = gen();
var value;
while (value = it.next(1).value) {
  console.log(value)
}
```

### async

async 函数是 ES8 中用于简化异步操作的语法，它基本上可以看作是 Generator 的一种改进版

```js
// 基本用法
function fetch (value) {
  return new Promise ((resolve, reject) => {
    setTimeout(() => {
      resolve(value)
    }, 1000)
  })
} 
(async function () {
  var a = await fetch(1);
  console.log(a);
  var b = await fetch(2);
  console.log(b);
  var c = await fetch(3);
  console.log(c);
})();

// 对比：generator 版
function* gen () {
  yield fetch(1);
  yield fetch(2);
  yield fetch(3);
}
var it = gen();
(function step () {
  var result = it.next();
  if (!result.done) {
    Promise.resolve(result.value).then(value => {
      console.log(value);
      step();
    })
  }
})()
```

> [javascript-es-2017-learn-async-await-by-example](https://codeburst.io/javascript-es-2017-learn-async-await-by-example-48acc58bad65)