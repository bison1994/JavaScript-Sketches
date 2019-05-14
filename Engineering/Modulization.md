> [前端模块化简史](http://www.cnblogs.com/kidney/p/6673189.html)

### webpack 模块化实现原理

```js
// a.js 入口文件
var b = require('b.js')
var c = require('c.js')

console.log(b.name)
console.log(c.name)

// b.js
console.log('module b runs')

exports default {
  name: 'b'
}

// c.js
var b = require('b.js')
console.log('module c runs')
console.log(b.name)

exports default {
  name: 'c'
}

// a.js 编译后 
var a = function (module, require) {
  var b = require(1)
  var c = require(2)

  console.log(b.name)
  console.log(c.name)
}

// b.js 编译后
var b = function (module, require) {
  console.log('module b runs')

  module.exports = {
    name: 'b'
  }
}

// c.js 编译后
var c = function (module, require) {
  var b = require(1)
  console.log('module c runs')
  console.log(b.name)
  
  module.exports = {
    name: 'c'
  }
}

webpack([a, b, c]);

var webpack = function (modules) {
  var installedModules = {};

  function require (id) {
    if (installedModules[id]) return installedModules[id].exports

    var module = installedModules[id] = {}

    modules[id](module, require);

    return module.exports
  }

  require(0);
}
```