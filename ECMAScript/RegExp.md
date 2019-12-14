### 规则

#### 元字符
- 字母和数字
- 符号（须转义）
- 字符类 Character Classes：匹配某种字符或字符集的符号
  + .  除 `\r` 与 `\n` 之外的任何字符，但在字符集中，点( . )失去其特殊含义，匹配一个字面点( . )
  + \n 换行
  + \r 回车
  + ...
- 匹配某个位置的符号 Boundaries
  + 见零宽断言部分

#### 字符集 [] Character Sets
- 匹配集合中任一字符，可用 - 简写，可用 ^ 取反
- 例：[a-z]、[^0-9]
- 常用字符类被预设为特定元字符
  + \w 字母、数字和下划线，即[a-zA-Z0-9_]
  + \W 非字母、数字和下划线，即[^a-zA-Z0-9_]
  + \d 数字，即[0-9]
  + \D 非数字，即[^0-9]
  + \s 空白 [ \t\v\n\r\f]
  + \S 非空白

#### 限定符 | 数量词 Quantifiers
- 贪婪：匹配尽可能多的字符
  +	{n} 匹配前一项 n 次
  + {n, m} 匹配前一项不少于 n 次，不超过 m 次
  +	{n，} 匹配前一项不少于 n 次
  +	? 匹配前一项 0 次或 1 次，表示可选
  +	\+ 匹配前一项至少 1 次
  +	\* 匹配前一项任意次数，包括 0 次

- 懒惰：匹配尽可能少的字符
  + *? 重复任意次，但尽可能少重复
  + +? 重复1次或更多次，但尽可能少重复
  + ?? 重复0次或1次，但尽可能少重复
  + {n, m}?	重复n到m次，但尽可能少重复
  + {n, }? 重复n次以上，但尽可能少重复

#### 条件分支 |

#### 分组 ()
- () 用于定义子模式，如 ([0-3]){3} 可匹配 000 ~ 333

#### 反向引用
- 子模式会记忆其匹配的文本，可以通过“美元符 + 组号”的方式引用
- 组号不能超过 9，因为 `/10` 会被认为是 `/1` 和 `0`
- 分组后面有量词的话，分组最终捕获到的数据是最后一次的匹配
- 例：`/(\d+)/.test('#333'); RegExp.$1 // 333`

#### 后向引用
- 表达式中可通过组号引用子模式，用于匹配重复性文本。通过组号引用的是子模式匹配并记忆的文本，而不是子模式
- 例：\1表示＂此处的文本应该和组号为 1 的子表达式所匹配的文本一致＂
- 组号分配规则：
  + \0 对应整个正则表达式
  + 从左向右扫描两遍的：第一遍只给未命名组分配，第二遍只给命名组分配
  + 可使用 `(?:)` 取消子表达式组号分配权
- 分组命名
  + 命名：(?<name>)
  + 引用命名分组：\k<name>

#### 零宽断言 Assertions
- 零宽，表示字符串中不占位的位置，如字符串 'ab'，a 和 b 之间的那个位置
- 断言，即条件判断
- 零宽断言就是符合一定条件的位置
- 语法（后两种 ES5 中不支持）
  + (?=exp) 此位置后应该匹配 exp
  + (?!exp) 此位置后不应该匹配 exp
  + (?<=exp) 此位置前应该匹配 exp
  + (?<!exp) 此位置前不应该匹配 exp
- 常用零宽断言被预设为特定元字符
  + ^ 匹配开头，此位置前不可存在任何符号
  + $ 匹配结尾，此位置后不可存在任何符号
  + \b 单词边界 => \W 与 \w 之间的位置
  + \B 非单词边界
- 这些代表位置的元字符又被称为“锚”


#### 修饰 flag
JavaScript 支持三个 flag
- i：模式匹配不区分大小写
-	g：模式匹配是全局的，应该找出所有的匹配
-	m：在多行模式中执行匹配


### RegExp related API of String

- search(exp)
  + 参数：正则表达式
  + 返回：第一个匹配模式的子串的起始位置，如果没有匹配项，则返回 -1
  + 注意：只能匹配第一个，自动忽略 g 修饰符
- replace(target, replacement)
  + 参数：第一个是字符串或正则，第二参数为字符串或函数
  + 返回：替换后的新字符串，并不修改原字符串
  + 第二个字符串参数中可以用 $ 引用子模式

```js
function capitalize (str) {
  return str.toLowerCase().replace(/(^|\s)\w/g, function (c) {
    return c.toUpperCase()
  })
}
```
> [replace 第二个参数为函数的用法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace#指定一个函数作为参数)

- match(exp)
  + 参数：正则表达式
	+ 返回：null 或数组
	+ 注意：全局模式下，返回所有匹配的文本构成的数组；非全局模式下，返回第一个匹配的文本和子模式匹配的文本构成的数组

```js
'1234'.match(/\d/g)
=> ["1", "2", "3", "4"]
'1234'.match(/\d/)
=> ["1", index: 0, input: "1234"]
'1234'.match(/(\d)/)
=> ["1", "1", index: 0, input: "1234"]
```

- split(divider)
  + 参数：分隔符。可以是字符串，也可以是正则。
	+ 返回：切分后的数组。


### RegExp API

- exec(str)
  + 返回：null 或数组
  + 无论是否全局，均返回同样的数组，由第一个匹配的文本和子模式匹配的文本构成的数组
  + 当使用全局模式时，exec 会动态的设置 lastIndex，从而可以迭代的调用 exec 方法
- test(str)
  + 与 exec 方法等价
- RegExp.$1
  + 执行正则相关操作，全局变量将捕获匹配到的子模式

```js
var reg = /[a-z]/g;
var text = 'a1b2c3';
var result;
while ((result = reg.exec(text)) != null) {
  console.log(result[0]); // a, b, c
  console.log(result.index); // 0, 2, 4
  console.log(reg.lastIndex); // 1, 3, 5
}
console.log(reg.lastIndex); // 0
```

> [最全正则表达式总结](http://www.lovebxm.com/2017/05/31/RegExp/)


### Example

```js
// 去除字符串两边的空格
function trim (str) {
  return str.replace(/(^\s*)|(\s*$)/g, "")
}

// 保留手机号码前三位与后四位，其余数字用四个 * 替代
// 涉及知识点：反向引用
function hidePhoneNumber (num) {
  var str = String(num);
  return str.replace(/(\d{3})\d*(\d{4})/, '$1****$2');
}

// 调换两个单词的位置，'hello world' => 'world hello'
function exchangeTwoWords (str) {
  return str.replace(/(\w+)\s(\w+)/, '$2 $1')
}

// 解析URL
// 此例来自《JavaScript权威指南》
var urlRE = /(\w+):\/\/([\w.]+)\/(\S*)/;
var url= "http://www.example.com/article/1";
var result = url.match(urlRE);
if (result != null) {
  var url = result[0]; // 与正则匹配的文本 "http://www.example.com/article/1"
  var protocol = result[1]; // 与第一个子模式匹配的文本 "http"
  var host = result[2]; // 与第二个子模式匹配的文本 "www.example.com"
  var path = result[3]; // 与第三个子模式匹配的文本 "article/1"
}

// 将短横线型的名称转换为驼峰型
var camelize = function (str) {
  return str.replace(/-(\w)/g, function (_, c) { return c ? c.toUpperCase() : ''; })
}

// 将驼峰型的名称转换为短横线型
var hyphenate = function (str) {
  return str.replace(/\B([A-Z])/g, '-$1').toLowerCase()
}
/* 上述两个正则来自 vue.js 源码 */

// 数字千分位加逗号
var addComma = function (str) {
  str.replace(/\B(?=(\d{3})+(?!\d))/g, ',')
}

// 解析字符串中的对象或数组
// 这是 jQuery 中的一个方法，如果给 DOM 节点属性传入对象或数组，就需要从字符串中解析出对象或数组。
// [\w\W] 表示一切字符
function getData (data) {
  var brace = /^(?:\{[\w\W]*\}|\[[\w\W]*\])$/
  if ( brace.test( data ) ) {
    return JSON.parse( data );
  }
}
```