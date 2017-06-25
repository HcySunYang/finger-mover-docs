---
nav: zh-cn
search: zh-cn
---

## simulation-scroll-y.vue

#### 概述

`simulation-scroll-y.vue` 是一个 `Vue` 单文件组件，该组件提供了 `finger-mover` 的 `simulation-scroll-y` 插件的封装。既然是封装，那么用法与 `simulation-scroll-y` 几乎相同。

源码不到 `90` 行代码，你可以很容易就能够理解，源码请查看[simulation-scroll-y.vue](https://github.com/HcySunYang/finger-mover/blob/dev/src/vue-component/simulation-scroll-y.vue)

以下是对该组件接口的文档说明，包括：`Props`、`Slots`、`Events` 以及 `Methods`。

#### Props

| name              | type          | default  | description  |
| -------------     |:-------------:| --------:| ------------:|
| scroll-bar        | `Boolean`     | `true`   | 是否显示滚动条  |
| unidirectional    | `Boolean`     | `false`  | 是否单向滑动，详情查看[unidirectional](/zh-cn/plugins/simulation-scroll-y?id=unidirectional) |
| bounce            | `Boolean`     | `true`   | 是否开启边界弹性 |
| pullDown          | `Object`      | 查看[pullDown](/zh-cn/plugins/simulation-scroll-y?id=pulldown)   | 下拉刷新配置 |
| loadMore          | `Object`      | 查看[loadMore](/zh-cn/plugins/simulation-scroll-y?id=loadmore)   | 无限滚动配置 |

#### Slots

| name              | description  |
| -------------     | ------------:|
| pulldown          | 下拉动作(如下拉刷新)自定义的DOM内容插槽  |
| loadmore          | 无限滚动自定义DOM内容插槽  |

##### 解释

`simulation-scroll-y` Vue 组件并没有提供内置的 `pulldown` 和 `loadmore` DOM结构以及布局，原因是一直以来大家在开发这块相关的内容时，基本都会有自己自定义的下拉刷新和无限滚动的样式。

比如有的产品要求下拉刷新的DOM结构要跟随可滚动元素一起动，有的产品则设计为下拉刷新DOM隐藏在可滚动元素后面，当拖动下拉时会展示下拉刷新DOM，而不是与可滚动元素一起滚动，出于此，组件的思想是：你可以完全自定义你想要的刷新DOM的模式以及样式。

如果你需要一些思路可以查看：[pulldown 的示例部分](/zh-cn/plugins/simulation-scroll-y?id=pulldown) 以及 [loadmore 的示例部分](/zh-cn/plugins/simulation-scroll-y?id=loadmore)

#### Events

| name              | parameters          | description  |
| -------------     |:-------------------:| ------------:|
| on-touchmove      | `(currentY)`        | 滚动元素当前的位置  |
| on-transmove      | `(currentY)`        | 滚动元素当前的位置  |
| on-transmove-end  | `(currentY)`        | 滚动元素当前的位置  |

#### Methods

通过给 `<simulation-scroll-y></simulation-scroll-y>` 组件定义 `ref` 属性：

```html
<simulation-scroll-y ref="ssy"></simulation-scroll-y>
```

可以在父组件中方法该组件，从而调用组件的方法：

```js
mounted () {
    let ssy = this.$refs.ssy
    ssy.scrollTo(-300, 1000)
}
```

可调用的组件方法以及方法的调用方式请参考：[simulation-scroll-y 的实例方法](/zh-cn/plugins/simulation-scroll-y?id=%E5%AE%9E%E4%BE%8B%E6%96%B9%E6%B3%95)