
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

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的：

```js
new Vue({
  // 选项
})
```

源码：[v2.6.14](https://github.com/vuejs/vue/tree/v2.6.14)

目录结构：

```
```

首先，找到 **Vue** 构造函数的定义，位于 `src/core/instance/index.js`。

```js
function Vue (options) {
  if (process.env.NODE_ENV !== 'production' && !(this instanceof Vue)) {
    // Vue是一个构造函数应该使用new关键字调用
    warn('Vue is a constructor and should be called with the `new` keyword')
  }
  // 初始化
  this._init(options)
}

initMixin(Vue)
stateMixin(Vue)
eventsMixin(Vue)
lifecycleMixin(Vue)
renderMixin(Vue)

export default Vue
```

定义了 Vue 之后，调用了一系列的 mixin 方法，这些方法主要用来给 Vue 函数添加一些原型属性和方法。

`src/core/instance/init.js` 看下 **initMixin** 方法，先是在 Vue 的原型上添加 _init 方法，然后调用了一些初始化函数，最后将 Vue 实例挂载到 DOM 上。

```js
let uid = 0

export function initMixin(Vue: Class<Component>) {
  // 在Vue的原型上添加_init方法，构造Vue实例的时候会调用这个_init方法来初始化Vue实例
  Vue.prototype._init = function (options?: Object) {
    const vm: Component = this
    // a uid
    // uid用来区分不同的Vue实例
    vm._uid = uid++

    let startTag, endTag
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      startTag = `vue-perf-start:${vm._uid}`
      endTag = `vue-perf-end:${vm._uid}`
      mark(startTag)
    }

    // a flag to avoid this being observed
    // 一个标志位防止Vue实例自身被观察
    vm._isVue = true
    // merge options
    if (options && options._isComponent) {
      // optimize internal component instantiation
      // since dynamic options merging is pretty slow, and none of the
      // internal component options needs special treatment.
      initInternalComponent(vm, options)
    } else {
      vm.$options = mergeOptions(
        resolveConstructorOptions(vm.constructor),
        options || {},
        vm
      )
    }
    /* istanbul ignore else */
    if (process.env.NODE_ENV !== 'production') {
      initProxy(vm)
    } else {
      vm._renderProxy = vm
    }
    // expose real self
    vm._self = vm
    // 初始化生命周期
    initLifecycle(vm)
    initEvents(vm)
    initRender(vm)
    callHook(vm, 'beforeCreate')
    initInjections(vm) // resolve injections before data/props
    initState(vm)
    initProvide(vm) // resolve provide after data/props
    callHook(vm, 'created')

    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
      vm._name = formatComponentName(vm, false)
      mark(endTag)
      measure(`vue ${vm._name} init`, startTag, endTag)
    }

    // 将Vue实例挂载到DOM上
    if (vm.$options.el) {
      vm.$mount(vm.$options.el)
    }
  }
}
```

在 `flow/component.js` 中声明了 **Component** 类型。

```js
declare interface Component {
  // constructor information
  static cid: number;
  static options: Object;
  // extend
  static extend: (options: Object) => Function;
  static superOptions: Object;
  static extendOptions: Object;
  static sealedOptions: Object;
  static super: Class<Component>;
  // assets
  static directive: (id: string, def?: Function | Object) => Function | Object | void;
  static component: (id: string, def?: Class<Component> | Object) => Class<Component>;
  static filter: (id: string, def?: Function) => Function | void;
  // functional context constructor
  static FunctionalRenderContext: Function;

  // public properties
  $el: any; // so that we can attach __vue__ to it
  $data: Object;
  $props: Object;
  $options: ComponentOptions;
  $parent: Component | void;
  $root: Component;
  $children: Array<Component>;
  $refs: { [key: string]: Component | Element | Array<Component | Element> | void };
  $slots: { [key: string]: Array<VNode> };
  $scopedSlots: { [key: string]: () => VNodeChildren };
  $vnode: VNode; // the placeholder node for the component in parent's render tree
  $attrs: { [key: string] : string };
  $listeners: { [key: string]: Function | Array<Function> };
  $isServer: boolean;

  // public methods
  $mount: (el?: Element | string, hydrating?: boolean) => Component;
  $forceUpdate: () => void;
  $destroy: () => void;
  $set: <T>(target: Object | Array<T>, key: string | number, val: T) => T;
  $delete: <T>(target: Object | Array<T>, key: string | number) => void;
  $watch: (expOrFn: string | Function, cb: Function, options?: Object) => Function;
  $on: (event: string | Array<string>, fn: Function) => Component;
  $once: (event: string, fn: Function) => Component;
  $off: (event?: string | Array<string>, fn?: Function) => Component;
  $emit: (event: string, ...args: Array<mixed>) => Component;
  $nextTick: (fn: Function) => void | Promise<*>;
  $createElement: (tag?: string | Component, data?: Object, children?: VNodeChildren) => VNode;

  // private properties
  _uid: number | string;
  _name: string; // this only exists in dev mode
  _isVue: true;
  _self: Component;
  _renderProxy: Component;
  _renderContext: ?Component;
  _watcher: Watcher;
  _watchers: Array<Watcher>;
  _computedWatchers: { [key: string]: Watcher };
  _data: Object;
  _props: Object;
  _events: Object;
  _inactive: boolean | null;
  _directInactive: boolean;
  _isMounted: boolean;
  _isDestroyed: boolean;
  _isBeingDestroyed: boolean;
  _vnode: ?VNode; // self root node
  _staticTrees: ?Array<VNode>; // v-once cached trees
  _hasHookEvent: boolean;
  _provided: ?Object;
  // _virtualComponents?: { [key: string]: Component };

  // private methods

  // lifecycle
  _init: Function;
  _mount: (el?: Element | void, hydrating?: boolean) => Component;
  _update: (vnode: VNode, hydrating?: boolean) => void;

  // rendering
  _render: () => VNode;

  __patch__: (
    a: Element | VNode | void,
    b: VNode,
    hydrating?: boolean,
    removeOnly?: boolean,
    parentElm?: any,
    refElm?: any
  ) => any;

  // createElement

  // _c is internal that accepts `normalizationType` optimization hint
  _c: (
    vnode?: VNode,
    data?: VNodeData,
    children?: VNodeChildren,
    normalizationType?: number
  ) => VNode | void;

  // renderStatic
  _m: (index: number, isInFor?: boolean) => VNode | VNodeChildren;
  // markOnce
  _o: (vnode: VNode | Array<VNode>, index: number, key: string) => VNode | VNodeChildren;
  // toString
  _s: (value: mixed) => string;
  // text to VNode
  _v: (value: string | number) => VNode;
  // toNumber
  _n: (value: string) => number | string;
  // empty vnode
  _e: () => VNode;
  // loose equal
  _q: (a: mixed, b: mixed) => boolean;
  // loose indexOf
  _i: (arr: Array<mixed>, val: mixed) => number;
  // resolveFilter
  _f: (id: string) => Function;
  // renderList
  _l: (val: mixed, render: Function) => ?Array<VNode>;
  // renderSlot
  _t: (name: string, fallback: ?Array<VNode>, props: ?Object) => ?Array<VNode>;
  // apply v-bind object
  _b: (data: any, tag: string, value: any, asProp: boolean, isSync?: boolean) => VNodeData;
  // apply v-on object
  _g: (data: any, value: any) => VNodeData;
  // check custom keyCode
  _k: (eventKeyCode: number, key: string, builtInAlias?: number | Array<number>, eventKeyName?: string) => ?boolean;
  // resolve scoped slots
  _u: (scopedSlots: ScopedSlotsData, res?: Object) => { [key: string]: Function };

  // SSR specific
  _ssrNode: Function;
  _ssrList: Function;
  _ssrEscape: Function;
  _ssrAttr: Function;
  _ssrAttrs: Function;
  _ssrDOMProps: Function;
  _ssrClass: Function;
  _ssrStyle: Function;

  // allow dynamic method registration
  [key: string]: any
};
```

挂载函数 **$mount** 位于 `src/platforms/web/runtime/index.js`。

```js
// public mount method
Vue.prototype.$mount = function (
  el?: string | Element,
  hydrating?: boolean
): Component {
  el = el && inBrowser ? query(el) : undefined
  return mountComponent(this, el, hydrating)
}
```

**mountComponent** 位于 `src/core/instance/lifecycle.js`。

```js
export function mountComponent (
  vm: Component,
  el: ?Element,
  hydrating?: boolean
): Component {
  vm.$el = el
  if (!vm.$options.render) {
    vm.$options.render = createEmptyVNode
    if (process.env.NODE_ENV !== 'production') {
      /* istanbul ignore if */
      if ((vm.$options.template && vm.$options.template.charAt(0) !== '#') || vm.$options.el || el) {
        warn(
          'You are using the runtime-only build of Vue where the template ' +
          'compiler is not available. Either pre-compile the templates into ' +
          'render functions, or use the compiler-included build.',
          vm
        )
      } else {
        warn(
          'Failed to mount component: template or render function not defined.',
          vm
        )
      }
    }
  }
  callHook(vm, 'beforeMount')

  let updateComponent
  /* istanbul ignore if */
  if (process.env.NODE_ENV !== 'production' && config.performance && mark) {
    updateComponent = () => {
      const name = vm._name
      const id = vm._uid
      const startTag = `vue-perf-start:${id}`
      const endTag = `vue-perf-end:${id}`

      mark(startTag)
      const vnode = vm._render()
      mark(endTag)
      measure(`vue ${name} render`, startTag, endTag)

      mark(startTag)
      vm._update(vnode, hydrating)
      mark(endTag)
      measure(`vue ${name} patch`, startTag, endTag)
    }
  } else {
    updateComponent = () => {
      vm._update(vm._render(), hydrating)
    }
  }

  // we set this to vm._watcher inside the watcher's constructor
  // since the watcher's initial patch may call $forceUpdate (e.g. inside child
  // component's mounted hook), which relies on vm._watcher being already defined
  new Watcher(vm, updateComponent, noop, {
    before () {
      if (vm._isMounted && !vm._isDestroyed) {
        callHook(vm, 'beforeUpdate')
      }
    }
  }, true /* isRenderWatcher */)
  hydrating = false

  // manually mounted instance, call mounted on self
  // mounted is called for render-created child components in its inserted hook
  if (vm.$vnode == null) {
    vm._isMounted = true
    callHook(vm, 'mounted')
  }
  return vm
}
```

**_render** 方法位于 `src/core/instance/render.js`。

```js
Vue.prototype._render = function (): VNode {
  const vm: Component = this
  const { render, _parentVnode } = vm.$options

  if (_parentVnode) {
    vm.$scopedSlots = normalizeScopedSlots(
      _parentVnode.data.scopedSlots,
      vm.$slots,
      vm.$scopedSlots
    )
  }

  // set parent vnode. this allows render functions to have access
  // to the data on the placeholder node.
  vm.$vnode = _parentVnode
  // render self
  let vnode
  try {
    // There's no need to maintain a stack because all render fns are called
    // separately from one another. Nested component's render fns are called
    // when parent component is patched.
    currentRenderingInstance = vm
    vnode = render.call(vm._renderProxy, vm.$createElement)
  } catch (e) {
    handleError(e, vm, `render`)
    // return error render result,
    // or previous vnode to prevent render error causing blank component
    /* istanbul ignore else */
    if (process.env.NODE_ENV !== 'production' && vm.$options.renderError) {
      try {
        vnode = vm.$options.renderError.call(vm._renderProxy, vm.$createElement, e)
      } catch (e) {
        handleError(e, vm, `renderError`)
        vnode = vm._vnode
      }
    } else {
      vnode = vm._vnode
    }
  } finally {
    currentRenderingInstance = null
  }
  // if the returned array contains only a single node, allow it
  if (Array.isArray(vnode) && vnode.length === 1) {
    vnode = vnode[0]
  }
  // return empty vnode in case the render function errored out
  if (!(vnode instanceof VNode)) {
    if (process.env.NODE_ENV !== 'production' && Array.isArray(vnode)) {
      warn(
        'Multiple root nodes returned from render function. Render function ' +
        'should return a single root node.',
        vm
      )
    }
    vnode = createEmptyVNode()
  }
  // set parent
  vnode.parent = _parentVnode
  return vnode
}
```

**VNode** 的定义位于 `src/core/vdom/vnode.js`。

```js
export default class VNode {
  tag: string | void;
  data: VNodeData | void;
  children: ?Array<VNode>;
  text: string | void;
  elm: Node | void;
  ns: string | void;
  context: Component | void; // rendered in this component's scope
  key: string | number | void;
  componentOptions: VNodeComponentOptions | void;
  componentInstance: Component | void; // component instance
  parent: VNode | void; // component placeholder node

  // strictly internal
  raw: boolean; // contains raw HTML? (server only)
  isStatic: boolean; // hoisted static node
  isRootInsert: boolean; // necessary for enter transition check
  isComment: boolean; // empty comment placeholder?
  isCloned: boolean; // is a cloned node?
  isOnce: boolean; // is a v-once node?
  asyncFactory: Function | void; // async component factory function
  asyncMeta: Object | void;
  isAsyncPlaceholder: boolean;
  ssrContext: Object | void;
  fnContext: Component | void; // real context vm for functional nodes
  fnOptions: ?ComponentOptions; // for SSR caching
  devtoolsMeta: ?Object; // used to store functional render context for devtools
  fnScopeId: ?string; // functional scope id support

  constructor (
    tag?: string,
    data?: VNodeData,
    children?: ?Array<VNode>,
    text?: string,
    elm?: Node,
    context?: Component,
    componentOptions?: VNodeComponentOptions,
    asyncFactory?: Function
  ) {
    this.tag = tag
    this.data = data
    this.children = children
    this.text = text
    this.elm = elm
    this.ns = undefined
    this.context = context
    this.fnContext = undefined
    this.fnOptions = undefined
    this.fnScopeId = undefined
    this.key = data && data.key
    this.componentOptions = componentOptions
    this.componentInstance = undefined
    this.parent = undefined
    this.raw = false
    this.isStatic = false
    this.isRootInsert = true
    this.isComment = false
    this.isCloned = false
    this.isOnce = false
    this.asyncFactory = asyncFactory
    this.asyncMeta = undefined
    this.isAsyncPlaceholder = false
  }

  // DEPRECATED: alias for componentInstance for backwards compat.
  /* istanbul ignore next */
  get child (): Component | void {
    return this.componentInstance
  }
}
```
