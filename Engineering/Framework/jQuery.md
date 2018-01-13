### Structure

```js
(function () {
  var jQuery = function (selector, context) {
    return new jQuery.fn.init(selector, context);
  }

  jQuery.fn = jQuery.prototype = {
    constructor: jQuery
    // ...
  }

  jQuery.fn.init = function (selector, context, root) {
    // ...
  }

  jQuery.extend = jQuery.fn.extend = function () {
    // ...
  }

  // set prototype properties
  jQuery.fn.extend({
    // ...
  })

  // set static properties
  jQuery.someMethod = function () {}
  jQuery.extend({
    // ...
  })

  window.jQuery = window.$ = jQuery;

  return jQuery
})();
```

### Tricks

```js
// execute a piece of script
function DOMEval (code) {
  var script = document.createElement("script");
  script.text = code;
  document.head.appendChild(script).parentNode.removeChild(script);
}

```