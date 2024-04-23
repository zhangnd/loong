# Vue2源码

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<div id="app">
  {{ message }}
</div>
```

```js
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 Vue **实例**开始的：

```js
new Vue({
  // 选项
})
```
