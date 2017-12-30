### SPA 的基本原理
1. “伪造” url 变化，避免页面跳转刷新，同时保证不破坏浏览器的 history 机制
2. 监听 url 变化，根据变化发送 ajax 请求，更新页面内容

### 实现手段

```js
function go (path) {
  location.hash = path;
}
window.onhashchange = function (event) {
  console.log(event.oldURL);
  console.log(event.newURL);
}
go('/list');
```

HTML5 的 history API 中定义了 pushState 方法，提供了一种更高级的伪造 url 的手段

```js
/**
 * pushState(状态对象, 标题（现在被忽略了）, 可选的 URL 地址)
 */
function go (state, path) {
  console.log(state.id)
  // 地址栏 url 会发生改变，但浏览器不会加载这个 url
  history.pushState(state, null, path);
  // 调用该方法并不会触发 window 的 popstate 事件
  // 因此在这里使用 ajax 更新页面数据
  doAjax(state);
}
go({id: 1}, '/list')
```

假设用户点击了浏览器的前进/后退按钮，就会触发 popstate 事件，并且对于通过 pushState 添加的记录，popstate 事件可以获得其 state 参数

```js
window.addEventListener('popstate', function (event) {
  console.log(JSON.stringify(event.state));
  doAjax(event.state);
})
```

