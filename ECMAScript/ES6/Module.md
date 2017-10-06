	与CommonJS和AMD模块作为对象在运行时动态加载不同，ES6模块不是对象，并且是在编译时静态加载的。
	模块功能由两个命令组成：export和import，一个负责输出接口，一个负责输入其它模块提供的接口。
export
	可以输出的内容有：变量、函数和对象
	不能用export直接输出一个值，而需要使用接口形式
	export m // 直接输出一个变量，错误
	export 1 // 直接输出一个值，错误
	export {a: a} // 直接输出一个对象，错误
	export var m = 1 // 正确
	export function f () { } // 正确

第二种形式
	var m = 1;
	function f () { };
	export {m, f}

第三种形式
	export default sth
	用于指定默认输出，其它模块引入时可自定义引入模块的名称
	一个文件中只能使用一次
	本质上是输出了一个default变量，上述命令的含义是将sth赋值给default
	因此使用export default可以直接输出值
	export default 1 // 直接输出一个数值
	export default m // 直接输出一个变量
	export default function () { } // 直接输出一个函数，即使有函数名，仍输出匿名函数
	export default { } // 直接输出一个对象

import
写法一：
	import {a, b, c} from "file.js"
	输入接口必须与file.js输出的接口名称一致，大括号不能省（ES7可能会简化）

写法二：
	import * as obj from "file.js"
	引入file.js所有的输出值并将其赋予对象obj
	obj.a // 调用file.js输出的a

写法三：
	如果引入目标模块的输出采用export default形式，那么输入模块的写法是：
	import name from "file.js"
	name是自定义的一个名字，无须使用大括号
	import $ from 'jquery'; // jquery就是一个函数

写法四：
	import "file.js"
	加载模块并执行，但不输入任何值
