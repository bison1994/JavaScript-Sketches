```js
var Dep = null

function defineReactive (obj, key, val) {
  var deps = [];
  Object.defineProperty(obj, key, {
    get: function () {
      if (Dep) {
        deps.push(Dep)
      }
      return val
    },
    set: function (newVal) {
      val = newVal;
      deps.forEach(func => func())
    }
  })
}

function defineComputed (obj, key, func) {
  func = func.bind(obj);
  var value;
  Dep = function () {
    value = func();
  };
  value = func();
  Dep = null;
  Object.defineProperty(obj, key, {
    get: function () {
      return value
    }
  })
}

var obj = {}

defineReactive(obj, 'a', 0);
defineComputed(obj, 'b', function () {
  var a = this.a;
  return a + 1
})

console.log(obj.b)
obj.a += 1;
console.log(obj.b);
obj.a += 1;
console.log(obj.b);
obj.a += 1;
console.log(obj.b);

```