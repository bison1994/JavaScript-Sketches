## how npm works

### pachage / module

A package is a file or directory that is described by a package.json

A module is any file or directory that can be loaded by Node.js' require()

<br>

### Dependencies

在树状结构的依赖关系下，包如何存储？如何处理版本冲突问题？

npm 2 完全按照依赖关系进行安装，dependency graph 和 dependency tree 是一致的。这会导致大量重复的包，使得 node_modules 变得特别重

npm 3 做了一定的优化：所有的依赖都以扁平的方式安装在顶层（root level），如果同一个包已经占据顶层位置，但出现不同版本依赖时，则继续安装在下一层 node_modules（[参考](https://npm.github.io/how-npm-works-docs/npm3/duplication.html)）

在这种规则下，谁先占据顶层，会影响到整个依赖的文件结构。决定谁占领顶层完全取决于**安装顺序**。不同的安装顺序，哪怕所有的包都一样，得到的 dependency tree 也不同

依赖包拓扑结构的不确定性，意味着任何时候都不应该用相对路径去引入依赖模块

假设后续执行了升级版本操作，会看顶层的包是否还有被依赖的情况，如果没有，该包会被替换掉

`npm dedupe` 用于去重，顶层以下的 node_modules 里，如果一个包和顶层中的包重复，则删除之

按照 npm 的工作方式，依赖中会存在【同一个包的不同版本】，还可能存在【同一个包同一个版本的多个 copy】，这两种形式的重复，都可能带来一些问题

此外 npm 3 的 hoist 策略，虽然缓解了包重复的问题，但也带来了新的问题：假设 C 依赖 B，那么 require('B') 本来引用的是 v1.0，提升后引用模块变成了 v2.0

```
// before hoist
A
 | - B 1.0
 | - C
B 2.0

// after hoist
A
 | - B 1.0
C
B 2.0
```

可见多版本共存是个很麻烦的问题，不过要想统一限定单个版本已经很难了，因为依赖还有依赖，一层套一层，早已积重难返

<br>

### Addressing

npm module loader 的寻址原则是逐级上溯递归查找各个 node_modules 目录

由此带来的一个副作用是：即便没有在 package.json 里声明，应用层仍然可以引用任何一个 node_modules 里的模块（隐式依赖，Phantom dependency），这显然会出问题

<br>

### Versioning

版本需要遵守 [semver](https://docs.npmjs.com/misc/semver) 规范，基本形态：[major].[minor].[patch]

由于这种规范只是一种约定，而非约束，由此也带来了一些因未遵守规范导致的问题

- 假设某三方包在 patch 升级时引入了 breaking changes，那么所有用 ~x.x.x 形式引入该包的地方，都可能出问题
- 如果上层应用本身就按照某个包的 bug 实现，那么对 bug 的修复就变成了 breaking change

如何保证项目依赖，以及依赖的依赖，其版本是固定的呢，解决办法是：lock 文件

不过 lock 文件也没办法完全保证不出问题，因为首次安装和全局安装的场景，就不存在 lock 文件，所以两台裸机，仅仅因为时间不同，安装出来的产物可能都不一样

一种解决办法是用 resolution 强制设定版本（针对依赖的依赖）

<br>

### Configuration

> [configuring your npmrc for an optimal node js environment](https://node.dev/post/configuring-your-npmrc-for-an-optimal-node-js-environment)

<br>

## Reference

> [11 simple npm tricks](https://node.dev/post/11-simple-npm-tricks-that-will-knock-your-wombat-socks-off)

> [An abbreviated history of JavaScript package managers](https://medium.com/javascript-in-plain-english/an-abbreviated-history-of-javascript-package-managers-f9797be7cf0e)
