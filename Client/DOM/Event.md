### 事件类型

[搜索“event”有82个结果](https://developer.mozilla.org/zh-CN/docs/Web/API)

- 鼠标
  + click
  + dblclick
  + mousedown
  + mouseup
  + mouseenter 不冒泡
  + mouseleave 不冒泡
  + mouseover
  + mouseout
  + mousemove
  + contextmenu
  + mousewheel [参考](https://github.com/itouzigithub/experience-pc/issues/1)

- 键盘
  + keydown
  + keypress
  + keyup

- 表单
  + textInput
  + input
  + focus
  + blur
  + change
  + submit

- 拖放
  + dragstart
  + drag
  + dragend
  + dragenter
  + dragover
  + dragleave
  + drop

- 触摸
  + touchstart
  + touchmove
  + touchend
  + gesturestart
  + gesturechange
  + gestureend

- 动画
  + transitionEnd
  + animationstart
  + animationend
  + animationiteration

- window
  + load
  + unload
  + beforeunload
  + resize
  + scroll
  + hashchange
  + popstate

- XMLHttpRequest
- 设备
  + Notification
  + DeviceMotionEvent
  + DeviceOrientationEvent
  + ...
- DOM mutation
- Media/Video/Audio/WebRTC
- Service Worker

### 事件对象

公共属性/方法

- bubbles
- cancelable
- target
- currentTarget
- type
- preventDefault()
- stopPropagation()
- stopImmediatePropagation()


### 事件流 Flow
1. 捕获（capture phase）：从 Window 对象查询到目标节点父元素
2. 目标（target phase）：在目标节点上触发
3. 冒泡（bubbling phase）：从目标节点父元素传导至 Window 对象


### 事件代理 Delegation
- 作用
  + 减少事件绑定数量
  + 使用根元素代理事件，避免捕获、冒泡等繁琐流程，减少节点查询路径，可提高事件触发的灵敏度

> “捕获阶段提供了在事件查询传播到目标之前将其拦截（捕获）的机会” ———— 《JavaScript 权威指南》

- 存在的问题
  + 可能增加事件系统复杂度，捕获阶段还是冒泡阶段？子元素事件是否屏蔽父元素代理？
  + 目标对象 target 如果 DOM 结构比较复杂，那么 target 的取值可能不精确，因而不适用


### 注册事件
- `<div id= "hello" onclick= "alert(id)"></div>`
- `el.onclick`
- `addEventListener() | removeEventListener()`
- `event = new Event(name, option)` [参考](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/Event)


### 触发事件
- `dispatchEvent()`