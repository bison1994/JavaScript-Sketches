# Typescript

### Benefit

- 在编译期提前发现错误
- 天然的文档
- 代码自动补全提示


**具体案例**

<img width="500px" src="https://raw.githubusercontent.com/bison1994/JavaScript-Sketches/master/Assets/ts-example-1.png">

<img width="500px" src="https://raw.githubusercontent.com/bison1994/JavaScript-Sketches/master/Assets/ts-example-2.png">


### 类型

使用 typescript，就是给 js 包装类型。js 中需要声明类型的地方，即需要用到 ts 的地方，主要有：

- 声明变量
    + 普通声明
    + 解构
- 声明函数
    + 函数参数
    + 函数返回值
- 声明类

如果不声明，typescript 会自动采用类型推断（Inferred Typing）


**类型的分类**

- 原子类型
    + number | string | boolean | object
    + Array<T> | [] | ReadonlyArray<T>
    + any
    + void
    + null | undefined (sub-types)
    + never
    + enum
    + Literal Types
        - string
        - number
    + this
- 复合类型
    + interface | class
    + tuple
- 工具类型
    + [see](http://www.typescriptlang.org/docs/handbook/utility-types.html)
- 组合类型
    + &（intersection）
    + |（Union）
- 计算类型
    + Index types `keyof`
    + Mapped types `[P in keyof T]: T[P]`
    + Conditional Types `T extends U ? X : Y`
        - Exclude<T, U>
        - Extract<T, U>
        - NonNullable<T>
        - ReturnType<T>
        - InstanceType<T>


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

// 参数解构
function f ({ a, b }: { a: number, b: number }) {
    return a + b
}

// 还可以用 interface 声明函数类型
interface Counter {
    (start: number): string // 函数体
}

// Hybrid Types
interface Counter {
    (start: number): string // 函数体
    interval: number // 静态变量
    reset(): void // 静态方法
}

// 用第一个参数声明 this 类型（编译后会被忽略）
function f (this: any, a: string) { this.a = a }

// this 可以被自动推导
var obj = {
    a: "",
    f (a: string) {
       this.a = a
    }
}
```


### 接口 Interface

```ts
// 值必须严格遵守接口定义的结构，不能少一个属性，也不能多一个属性
// 如果要少一个属性，就需要使用可选属性；如果允许定义额外的属性，需要使用索引签名（Index Signatures）
interface Obj {
    x: string // 必填属性
    y?: number // 可选属性
    readonly z: boolean // 只读属性
    [extraProp: string]: any // 索引签名
    fn: (param1: string, param2: number) => boolean // 函数方法
    fn (param1: string, param2: number): boolean // 同上，简写形式
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

// 继承多个
interface A extends B, C {}

// 实现
class A inplements Interface {}
```


### 泛型 Generic

泛型，用参数变量表示类型。具体类型由实际调用时指定

- interface
- function
- class

```ts
// interface
interface Generic<T> {
    status: T
}
const obj: Generic<string> = { status: 'ok' }
const obj2: Generic<number> = { status: 1 }

// function
function identity<T>(arg: T): T {
    return arg
}
const output = identity<string>('myString')
const output = identity('myString') // 也可以不指定具体类型，ts 会自动推导出 T = string

interface GenericIdentityFn<T> {
    (arg: T): T
}

let myIdentity: GenericIdentityFn<number> = identity

// class
class GenericNumber<T> {
    zeroValue: T
    add: (x: T, y: T) => T
}
let myGenericNumber = new GenericNumber<number>()
```

期望泛型满足某种条件，用 extend 对泛型变量作限定（constraint）

```ts
interface Lengthwise {
    length: number
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length) // 期望参数有 length 属性
    return arg
}

loggingIdentity('') // ok
loggingIdentity({ length: 1 }) // ok
loggingIdentity(1) // error

// 用泛型参数 T 的属性限定泛型参数 K
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key]
}
```


### 枚举 Enum

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

### 类型推导 Type Inference

**Basic**

- 赋值操作
- 函数参数默认值
- 函数返回值（前提是返回的值/变量本身有 type 信息）

> [参考](https://basarat.gitbooks.io/typescript/content/docs/types/type-inference.html)


**Contextual Typing**

- 事件对象的属性
- `this` in arrow function


### 类型的兼容

- type 兼容 subtype
- 结构的一致性兼容
    + interface
    + function
    + class
    + generic

```ts
interface Named {
    name: string
}

let x: Named
// y's inferred type is { name: string location: string }
let y = { name: "Alice", location: "Seattle" }
x = y // ok. y has at least the same members as x
```


### Type Guards

某些情况下（联合类型、可能为 null 的值）即便代码完全正确，但 ts 的类型系统会判定错误，因此需要做些额外调整

- in
- typeof | instanceof
- is
- !

```ts
// 案例一
interface Bird {
    fly (): any
}

interface Fish {
    swim (): any
}

function getSmallPet (): Fish | Bird {
    return Math.random() > 0.5 ? { fly() {} } : { swim() {} }
}

const pet = getSmallPet()
if (pet.swim) { // errors
    pet.swim() // errors
} else {
    pet.fly() // errors
}

if ('swim' in pet) {
    pet.swim()
} else { 
    pet.fly()
}

// 案例二
function f (name: string | null): string {
  function postfix (epithet: string) {
    return name.charAt(0) + '.  the ' + epithet // error, 'name' is possibly null
    // 解决方案：return name!.charAt(0) + '.  the ' + epithet
  }
  name = name || "Bob"
  return postfix("great")
}
```



### 其它语法

**class**

- public、private、protected
- readonly
- get | set
- static
- abstract class
- 用作 interface（声明一个 class，既创造了一个构造函数，也创造了一个类型）



**type**

Aliases

```ts
type name = xxx

type name = xxx & xxx

type Generic<T> = xxx
```


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


