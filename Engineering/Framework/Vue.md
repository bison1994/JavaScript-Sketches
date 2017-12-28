### 最佳实践

- 组件命名：首字母大写驼峰式或短横线式，名称本身可以添加元信息或描述信息，如 `BaseButton`，`NavBarForSomewhere`
- props 应该具有详细的定义，props 名称在 js 中用 camelCase，在 html 中用 kebab-case
- 经常变化的数据不要放在顶层组件
- 对于只需渲染一次而无须响应式绑定的模板，使用 v-once
- 无须响应式绑定的数据，用 object.freeze() 冻结
- 使用 webpack 代码拆分