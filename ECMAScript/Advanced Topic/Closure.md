### What

闭包就是在函数内部定义的函数

### Why

- 闭包的设计是为了应对这样一个问题：一个函数要访问的变量该定义在哪儿？
- 定义在函数内，可能存在几个问题：
  + 在递归结构中无法对变量进行持续的修改
  + 其它函数无法访问这个变量
  + 函数执行完毕，变量会被自动回收
- 避免上述问题，可以定义在全局环境，然而这又引入了新的麻烦：命名冲突
- 闭包提供了一种新的可能：将变量定义在闭包中，既不会被自动回收，又可以多个函数共享

> 松本行弘对闭包的解释 ​​​：闭包和对象都是“数据与过程的结合”，对象是在数据中以方法的形式内含了过程，闭包则是在过程中以环境的形式内含了数据。

### How

- 本质就是闭包继承了外部作用域（链），因此闭包不仅可以读取，还可以修改外部作用域的值
- 除了可以读取外部变量外，闭包的另一个重要特性是，当它作为一个函数的返回值时，它的作用域链会一直保存到闭包不存在为止（即不会被垃圾回收机制自动回收），也就是说闭包具有比其外部函数更长的生命周期

```js
  var inner = (function outer () {
    var a = 1;
    return function () {
      console.log(a++)
    }
  })()
```


利用闭包，可以创造一些特殊的程序结构

```js
// obj 是一个包含两个闭包的对象，这两个闭包都可以共同访问一个外部变量：val
var obj = (function () {
  var val = 0; // private variable
  return {
	  increment: function (num) {
      val += num;
    },
    get: function () {
      return val
    }
  }
})();
```

```js
// 阶乘
var Factorial = (function () {
	var x = 1;
	return function Factorial (num) {
		if (num == 0) return x;
		x *= num;
		return Factorial(num - 1);
	}			
})();
Factorial(5); // 120
```