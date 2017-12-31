- 选取元素
  + document.getElementById()
  + document.getElementsByName()
  + document.getElementsByTagName()
  + document.getElementsByClassName()
  + document.querySelectorAll(selector)
  + document.querySelector(selector)
  + document.forms / links / images /...
  + document.body / head /...

- 节点信息
  + .nodeType
  + .nodeName

- 节点关系
  + .childNodes
  + .children
  + .firstChild
  + .firstElementChild
  + .lastChild
  + .lastElementChild
  + .childElementCount
  + .parentNode
  + .nextSibling
  + .nextElementSibling
  + .previousSibling
  + .previousElementSbling
  + .ownerDocument
  + .hasChildNodes()
  + .contains()

- 操作节点
  + document.createElement()
  + document.createTextNode()
  + document.createDocumentFragment()
  + .appendChild()
  + .insertBefore()
  + .replaceChild()
  + .removeChild()
  + .cloneNode()
  + .textContent /.innerText（IE）
  + .innerHTML

- 操作文本节点
  + .nodeValue / .data
  + .length
  + .appendData(text)
  + .deleteData(offset, count)
  + .insertData(offset, text)
  + .replaceData(offset, count, text)
  + .splitText(offset)
  + .substringData(offset, count)
  + .normalize()

- 操作属性
  + .id / .className / ……
  + .style
  + .attributes
  + .classList
  + .getAttribute()
  + .hasAttribute()
  + .removeAttribute()
  + .setAttribute()

- 操作表单


- 操作样式
  + window.getComputedStyle(element)
  + element.style
  + document.styleSheets

```js
var sheet = document.styleSheets[0];
var rules = sheet.cssRules || sheet.rules; // 返回数组
rules[0].selectorText // 获取选择器
rules[0].style.cssText // 获取 css 内容文本

sheet.insertRule("body { background-color: silver }", 0) // 非 IE
sheet.addRule("body", "background-color: silver", 0) // IE

el.style.color = "blue"
el.style.cssText = "color: blue"
```

- 遍历节点
  + TreeWalker
  + NodeIterator

- 尺寸与坐标
  + 元素尺寸
    - .offsetWidth /.offsetHeight
    - .clientWidth /.clientHeight
    - .scrollWidth/.scrollHeight
  + 窗口尺寸
    - window.innerWidth/innerHeight
  + 元素坐标
    - .offsetLeft /.offsetTop
    - .getBoundingClientRect()
  + 滚动
    - .scrollLeft/.scrollTop
    - .scrollIntoView()
    - window.scrollTo()

- 动画
  + requestAnimationFrame

- 富文本
  + contenteditable 属性
  + document.designmode = 'on'
  + execCommand()

- 获取选中文本
  + window.getSelection()