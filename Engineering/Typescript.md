# Typescript

### Benefit

- 代码健壮性。避免常见的类型错误：cannot read property 'xxx' of undefined | undefined is not a function
- 天然的文档

**具体案例**

<img src="https://raw.githubusercontent.com/bison1994/JavaScript-Sketches/master/assets/ts-example-1.png">

<img src="https://raw.githubusercontent.com/bison1994/JavaScript-Sketches/master/assets/ts-example-2.png">


### Basic

使用 typescript，就是给 js 包装类型。js 中需要声明类型的地方，即需要用到 ts 的地方，主要有：

- 声明变量
- 声明函数
    + 函数参数
    + 函数返回值
- 声明类

如果不声明，typescript 会自动采用类型推断（Inferred Typing）

> 除了添加类型，ts 还给 class 声明添加了 public、private、protected 等类似 Java 中的语法，或者说 ts 提供了自己的 class 语法


**类型的分类**

- any
- build-in types
    + number
    + string
    + boolean
    + void
    + null
    + undefined
    + never
    + ...
- user-defined types
    + 接口 interface
    + 元组 Tuple
    + 泛型 Generics
    + 枚举 Enums
    + 数组 Array

**声明类型的语法**

- 冒号
- 类型断言
    + `<type>`Variable
    + Variable `as type`


**编译目标**

ts 最终要编译成 js，可以在配置中指定编译目标

```js
'compilerOptions': {
    'target': 'esnext' // 'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018' or 'esnext'
}
```


### 声明变量类型

```ts
const name: string = 'bob'
const name = 'bob' as string
const name = <string>'bob'

const notSure: any = 4

const list: number[] = [1, 2, 3]
const list: Array<number> = [1, 2, 3] // 泛型 Generics
const x: [string, number] = ['a', 0] // 元组 Tuple（当数组中含有不同类型数据时使用）

const f = string | number // Union
```


### 声明函数类型

```ts
// 完整版
const add: (x: number, y: number) => number
add = function (x, y) { return x + y }

// 简化版
const add = function (x: number, y: number): number { return x + y }
function add (x: number, y: number): number { return x + y }

// 函数的每个参数都是必填的，如果想设置为选填，有两种办法：一是用 ?: 设置选填，二是设置默认值
function fullName (first: string, last?: string) { return first + last }
function fullName (first: string, last = 'bob') { return first + last }

// 剩余参数
function fullName (first: string, ...other: string[]) { return first + other.join(' ') }
```

> 还可以用 interface 声明函数类型


### 定义 Interface

```ts
// 值必须严格遵守接口定义的结构，不能少一个属性，也不能多一个属性
// 如果要少一个属性，就需要使用可选属性；如果允许定义额外的属性，需要使用索引签名（Index Signatures）
interface Obj {
    x: string // 必填属性
    y?: number // 可选属性
    readonly z: boolean // 只读属性
    [extraProp: string]: any // 索引签名
    fn: (param1: string, param2: number) => boolean // 函数方法
    fn(param1: string, param2: number): boolean // 同上，简写形式
}
const obj: Obj = { ... }

// Interface 可以继承
interface Person { 
   age:number
} 

interface Musician extends Person { 
   instrument:string 
}

const drummer = <Musician>{}
drummer.age = 27
drummer.instrument = 'Drums'

// 继承多个：interface A extends B, C {}
```


### 类型断言 Type Assertions

可用于类型转换。但仅允许向子类型方向转换

```ts
const str = '1' 
const str2:number = <any> str  // ok
const str2:number = <number> <any> str  // ok
const str2:number = <number> str  // error: Type 'string' cannot be converted to type 'number'
```


### 泛型

泛型，即参数类型化。可以将泛型看成一个函数，可以将某个类型作为参数传入，从而得到一个新的 interface

```ts
interface Generic<T> {
    status: T
}
const obj: Generic<string> = { status: 'ok' }
const obj2: Generic<number> = { status: 1 }

function identity<T>(arg: T): T {
    return arg
}
const output = identity<string>('myString')
const output = identity('myString') // 也可以不指定具体类型，ts 会自动推导出 T = string

function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length)  // Array has a .length, so no error
    return arg
}

interface GenericIdentityFn {
    <T>(arg: T): T
}

let myIdentity: GenericIdentityFn = identity

// 让 T 的作用域扩大到整个 interface
interface GenericIdentityFn<T> {
    (arg: T): T
}

let myIdentity: GenericIdentityFn<number> = identity

// class 泛型
class GenericNumber<T> {
    zeroValue: T
    add: (x: T, y: T) => T
}
let myGenericNumber = new GenericNumber<number>()

// 对参数变量的限定（constraint）
interface Lengthwise {
    length: number
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length)  // Now we know it has a .length property, so no more error
    return arg
}

// 用参数变量的 type 限定其它变量
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key]
}
```

> 内置的泛型 class：Array、Promise...


### 枚举

枚举会被编译为真实存在的对象

```ts
enum Status {
    todo,
    doing,
    done
}

// 编译为
var Status
(function (Status) {
    Status[Status['todo'] = 0] = 'todo'
    Status[Status['doing'] = 1] = 'doing'
    Status[Status['done'] = 2] = 'done'
})(Status || (Status = {}))
// {0: "todo", 1: "doing", 2: "done", todo: 0, doing: 1, done: 2}
```


### 其它语法

除了类型系统，ts 还给自己加了别的戏（有些戏是必要的）

**modules**

ts 的模块化语法分为

- internal module：不需要 import，直接用，已更新为 namespace 语法
- external module：需要定义导出模块，然后用导入语法引用

ts 除了拥有和 ES6 一样的模块语法，还添加了其它语法：

```ts
// ZipCodeValidator.ts
export = ZipCodeValidator

// test.ts
import zip = require('./ZipCodeValidator') // specified no extension used
```

ts 编译器可以编译出不同模块化标准的代码

```
tsc --module commonjs Test.ts
```

或者通过配置指定模块化

```js
{
    'compilerOptions': {
        'module': 'esnext' // 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015' or 'esnext'
    }
}
```


**namespace**

namespace 用于替代老的 internal module 语法

```ts
namespace TutorialPoint { 
   export function add(x, y) { console.log(x + y) } 
}

// 同一个文件里的用法
TutorialPoint.add(1, 2)

// 其它文件里的用法
/// <reference path='Validation.ts' />
TutorialPoint.add(1, 2)

// namespace 编译为：
var TutorialPoint
(function (TutorialPoint) {
    function add(x, y) {
       console.log(x + y)
    }
    TutorialPoint.add = add
})(TutorialPoint || (TutorialPoint = {}))
```

也可以用 import 语法（一种 alias 写法）

```ts
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons
let sq = new polygons.Square()
```


**Ambient**

因为第三方库很可能不是用 ts 写的，怎么在 ts 项目里兼容第三方库的引用呢？

```ts
declare module 'date-fns'
```


