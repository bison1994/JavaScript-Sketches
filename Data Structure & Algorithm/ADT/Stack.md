栈可分为定容与不定容
<br>
### API
 - push(element)
 - pop()
 - isEmpty()
 - size()

基于数组的实现
```js
class Stack {
  constructor () {
    this.arr = [];
    this.size = 0;
  }

  push (el) {
    this.arr[this.size++] = el
  }

  pop () {
    return this.arr[--this.size]
  }

  isEmpty () {
    return this.size === 0
  }

  size () {
  
    return this.size
  }
}
```