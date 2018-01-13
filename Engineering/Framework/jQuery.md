### Structure


### Tricks

```js
// execute 
function DOMEval (code) {
  var script = document.createElement("script");
  script.text = code;
  document.head.appendChild(script).parentNode.removeChild(script);
}

```