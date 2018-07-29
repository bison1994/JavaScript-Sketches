### MutationObserver
[Detect-DOM-changes-with-Mutation-Observers](https://developers.google.com/web/updates/2012/02/Detect-DOM-changes-with-Mutation-Observers)
<br>
Back in 2000, the Mutation Events API was specified to make it easy for developers to react to changes in a DOM.
<br>
Mutation Events are useful, but at the same time they create some performance issues. The Events are slow and they are fired too frequently in a synchronous way.
<br>
Introduced in the DOM4 specification, DOM Mutation Observers will replace Mutation Events. Whereas Mutation Events fired slow events for every single change, Mutation Observers are faster using callback functions that can be delivered after multiple changes in the DOM.

```js
// 创建观察者对象
var observer = new MutationObserver(function (mutations) {
  mutations.forEach(function (mutation) {
    console.log(mutation.type);
  });    
});
// 传入目标节点和观察选项
observer.observe(element, config);
```

### PerformanceObserver


### IntersectionObserver
```js
const observer = new IntersectionObserver(callback, options);
observer.observe(element);
```

### ResizeObserver
Generally, ResizeObserver reports the content rectangle of an element. It only watches the contentRect.

```js
var ro = new ResizeObserver(entries => {
  for (let entry of entries) {
    const cr = entry.contentRect;
    console.log('Element:', entry.target);
    console.log(`Element size: ${cr.width}px x ${cr.height}px`);
    console.log(`Element padding: ${cr.top}px ; ${cr.left}px`);
  }
});

// Observe one or multiple elements
ro.observe(someElement);
```