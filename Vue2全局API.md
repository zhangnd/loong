# Vue2全局API

## Vue.nextTick

用于在 DOM 更新循环结束之后执行延迟回调。Vue 在更新 DOM 时是`异步`执行的，为了在数据变化之后等待 Vue 完成更新 DOM，可以在数据变化之后立即调用 `Vue.nextTick(callback)`。这样回调函数将在 DOM 更新完成后被调用。

```js
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(() => {
  // DOM 更新了
})
```

## Vue.set

向响应式对象中添加一个 property，并确保这个新 property 同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新 property，因为 Vue 无法探测普通的新增 property。

## Vue.directive

注册全局指令。除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，Vue 也允许注册自定义指令。

```js
// 注册
Vue.directive('my-directive', {
  bind: () => {},
  inserted: () => {},
  update: () => {},
  componentUpdated: () => {},
  unbind: () => {}
})

// 注册 (指令函数)
Vue.directive('my-directive', () => {
  // 这里将会被 `bind` 和 `update` 调用
})
```

## Vue.filter

注册全局过滤器。Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示。

```js
// 注册
Vue.filter('my-filter', value => {
  // 返回处理后的值
})
```

## Vue.component

注册全局组件。组件是可复用的 Vue 实例。

```js
// 注册组件，传入一个扩展过的构造器
Vue.component('my-component', Vue.extend({ /* ... */ }))

// 注册组件，传入一个选项对象 (自动调用 Vue.extend)
Vue.component('my-component', { /* ... */ })
```

## Vue.use

安装 Vue.js 插件。如果插件是一个对象，必须提供 `install` 方法。如果插件是一个函数，它会被作为 install 方法。install 方法调用时，会将 Vue 作为参数传入。

该方法需要在调用 `new Vue()` 之前被调用。

当 install 方法被同一个插件多次调用，插件将只会被安装一次。

插件通常用来为 Vue 添加全局功能。插件的功能范围没有严格的限制——一般有下面几种：

1. 添加全局方法或者 property。如：[vue-custom-element](https://github.com/karol-f/vue-custom-element)

2. 添加全局资源：指令/过滤器/过渡等。如 [vue-touch](https://github.com/vuejs/vue-touch)

3. 通过全局混入来添加一些组件选项。如 [vue-router](https://github.com/vuejs/vue-router)

4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。

5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 [vue-router](https://github.com/vuejs/vue-router)

## Vue.mixin

全局注册一个混入，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混入，向组件注入自定义的行为。

混入也可以进行全局注册。使用时格外小心！一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。
