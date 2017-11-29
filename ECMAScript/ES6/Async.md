async 声明一个异步函数，此函数需要返回一个 Promise 对象。await 等待一个 Promise 对象 resolve 或 reject，并拿到结果，因此无需再写 then 方法。

```js
async function fetch () {
  return new Promise ((resolve, reject) => {
    if (/* success */) {
      resolve(/* data */)
    } else {
      reject(/* data */)
    }
  })
} 
(async function () {
  var result = await fetch();
  /* function will not be invoked until the result is returned */
})();
```

> [javascript-es-2017-learn-async-await-by-example](https://codeburst.io/javascript-es-2017-learn-async-await-by-example-48acc58bad65)