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


- 尺寸与坐标


- 动画
  + requestAnimationFrame


- 注册事件
  + `<div id= "hello" onclick= "alert(id)"></div>`
  + `el.onclick`
  + `addEventListener() | removeEventListener()`

- 触发事件
  + `dispatchEvent()`


