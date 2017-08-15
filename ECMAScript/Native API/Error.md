- new Error('错误信息')
- new TypeError('参数类型错误')
- new RangeError('范围错误')

```js
try {
  throw new Error('something wrong')
} catch (e) { 
  throw e.name + ': ' + e.message // Uncaught Error: something wrong
}

try {
  throw new Error('something wrong')
} catch (e) { 
  console.error(e.name + ': ' + e.message) // Error: something wrong
}
```

以下对象一般由 JS 引擎调用
- new ReferenceError('变量不存在')
- new SyntaxError('语法错误')