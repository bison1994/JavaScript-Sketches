set：成员唯一的数组
属性	长度	size
方法	增删查	add()
		delete()
		has()
		clear()
	遍历	keys()
		values()
		entries()
		forEach()
数组去重
方法一
function dedupe(array) {
  return Array.from(new Set(array));
}
方法二
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];

WeakSet：成员只能是对象且不可遍历的set
三个方法：add(), delete(), has()

map：键可以为各种数据类型的对象
Map的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。
属性	长度	size
方法	增删查	set(key, value)
		get(key)
		delete(key)
		has(key)
		clear()
	遍历	keys()
		values()
		entries()
		forEach()
map => array
let map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);
[...map] // [[1,'one'], [2, 'two'], [3, 'three']]

WeakMap：只接受对象作为键名的map
