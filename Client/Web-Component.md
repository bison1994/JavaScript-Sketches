### 组成

- Custom Elements
- HTML templates
- Shadow DOM：css 和 js 的独立作用域
- HTML Modules

> 以上各个模块可以单独使用，也可以组合使用


### Custom Elements

```js
// js
class MyComponent extends HTMLElement {
    connectedCallback() {
        this.innerHTML = `<h1>Hello world</h1>`
    }
}

// customElements api
customElements.define('my-component', MyComponent)

// html
<my-component></my-component>
```

生命周期

- connectedCallback：每次组件 append 到 dom 中时执行
- disconnectedCallback
- adoptedCallback
- attributeChangedCallback：配合 `static get observedAttributes() { return ['c', 'l'] }` 使用


### HTML templates

用于定义可重用的组件

```html
// template 仅仅是定义，并不会自动执行，需要 js 调用
<template id="template">
    <script>
        const button = document.getElementById('click-me');
        button.addEventListener('click', event => alert(event));
    </script>
    <style>
        #click-me {
            all: unset;
            background: tomato;
            padding: .5rem 1rem;
        }
    </style>
    <button id="click-me">Log click event</button>
</template>
```

```js
const template = document.getElementById('template')

document.body.appendChild(
  document.importNode(template.content, true) // true 表示深拷贝，即包括所有子节点
)

// 通过 Custom Elements 的方式调用
class MyButton extends HTMLElement {
    connectedCallback () {
        const template = document.getElementById('template')

        this.appendChild(
            document.importNode(template.content, true)
        )
    }
}

// 在 html 中用 <my-button /> 即可调用组件
customElements.define('my-button', MyButton);

// 或者像是普通 html 元素一样，创建一个组件
const btn = document.createElement('my-button')
document.body.appendChild(btn)

// 自定义组件可以像普通元素一样被查询
console.log(document.querySelector('my-button'))
```


### Shadow DOM

HTML templates 的问题是没有独立作用域，外部的样式可以影响内部，内部的样式也能影响外部，js 也一样

Shadow DOM 的目的是创建隔离的、作用域独立的环境（isolated DOM fragment）

```js
// 这下 <my-button> 的样式不会影响外部，外部样式也不会影响 my-button
class MyButton extends HTMLElement {
    constructor () {
        super()
        this.attachShadow({ mode: 'open' })
    }

    connectedCallback () {
        const template = document.getElementById('template')

        this.shadowRoot.appendChild(
            document.importNode(template.content, true)
        )

        const button = this.shadowRoot.getElementById('click-me')
        button.addEventListener('click', event => alert(event))
    }
}
```

通过 slot 自定义内容

```js
class MyComponent extends HTMLElement {
    constructor () {
        super()
        this.attachShadow({ mode: 'open' })
    }

    connectedCallback () {
        this.shadowRoot.innerHTML = `
            <style>p { color: tomato }</style>
            <p>hello, <slot /></p>
        `
    }
}

customElements.define('my-component', MyComponent)

<my-component>dude</my-component>
```
