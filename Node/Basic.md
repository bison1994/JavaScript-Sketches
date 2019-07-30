### 架构

node 除了自身架构，还引入了有限的一组第三方库，运行 process.versions 可以得到：

```js
// node v10.16.0
{
    http_parser: '2.8.0',
    node: '10.16.0',
    v8: '6.8.275.32-node.52',
    uv: '1.28.0',
    zlib: '1.2.11',
    brotli: '1.0.7',
    ares: '1.15.0',
    modules: '64',
    nghttp2: '1.34.0',
    napi: '4',
    openssl: '1.1.1b',
    icu: '64.2',
    unicode: '12.1',
    cldr: '35.1',
    tz: '2019a'
}
```


### 语言

- 新的数据类型：Buffer
- 全局变量：global
    + process
    + require
    + module
    + console
    + setTimeout
    + __dirname

> 某些涉及 script 文件的变量在 REPL 中不存在，例如 __dirname，[参考](https://stackoverflow.com/questions/8817423/node-dirname-not-defined)


### 官方标准库

- http
- util
- querystring
- url
- fs


### 调试

- [Debugger](https://nodejs.org/api/debugger.html)
- [node-inspector](https://github.com/node-inspector/node-inspector)
- IDE