---
nav: zh-cn
search: zh-cn
---

## 插件开发

编写插件前，你需要了解一下 [Fingerd(移动端以手指为单位的事件管理方案)](/zh-cn/package/fingerd) 和 [Moved(微型运动方案)](/zh-cn/package/moved)

#### 插件结构

`finger-mover` 的一个特点即插件化，写一个 `finger-mover` 插件很简单，首先让我们来看看如何使用一个插件：

```js
new Fmover({
    el: box,
    pulgins: [
        somePlugin({
            a: 1,
            b: 2
            ...
        })
    ]
})
```

上面代码中，使用了一个叫做 `somPlugin` 的插件，使用一个插件即调用插件所暴露的函数，并传递插件提供的相应选项即可。所以一个插件必须要满足以下几点：

1、暴露一个函数，我们称其为 `主函数`，`主函数`即为使用插件时调用的函数，你可以在`主函数`里接受用户传递的参数。

2、`主函数` 应该返回一个函数，我们称其为 `次函数`。`次函数`的形参为 `Fmover` 构造函数。

3、`次函数` 应该返回包含以下一个或多个方法的对象。

###### init(fmover)

* 描述：

    插件初始化时调用，你可以在里面处理一些初始化的工作。`fmover` 实例对象将作为参数传递。

###### start(fingerd)

* 描述：

    `touchstart` 事件触发时同步调用的函数，`fingerd` 实例对象将作为参数传递。

###### move(fingerd)

* 描述：

    `touchmove` 事件触发时同步调用的函数，`fingerd` 实例对象将作为参数传递。

###### end(fingerd)

* 描述：

    `touchend` 事件触发时同步调用的函数，`fingerd` 实例对象将作为参数传递。

###### cancel(fingerd)

* 描述：

    `touchcancel` 事件触发时同步调用的函数，`fingerd` 实例对象将作为参数传递。

<p class="tip">
除了以上方法外，你还可以在返回的对象上定义更多方法，不过这些方法并不会被 `finger-mover` 处理，不过你依然可以在组件内部使用。
</p>

#### Fmover 工具函数

上面介绍过：`次函数` 的形参为 `Fmover` 构造函数，在 `Fmover` 构造函数上，挂载了很多实用的方法，如：

```js
// ES6 结构
let {
    Moved,
    Fingerd,
    cssText,
    getPropFromMatrix,
    getRelativeRect,
    isInElement,
    extend,
    now,
    getStyle,
    throwError
} = Fmover
```

下面是每个工具函数的详细介绍：

###### Moved

* 描述：

    `Moved` 构造函数，请查看 [Moved](/zh-cn/package/moved)

###### Fingerd

* 描述：

    `Fingerd` 构造函数，请查看 [Fingerd](/zh-cn/package/fingerd)

###### cssText(el, cssText)

* 描述：

    设置元素的 `cssText`，不同于直接设置 `el.style.cssText`，`cssText()` 相当于:
    
    ```js
    el.style.cssText = el.style.cssText + ' ' + cssText
    ```

* 参数：

    * `{Element} el` 元素
    * `{String} cssText` css样式字符串

* 示例：

    ```js
    cssText(el, 'background: #000; color: red; transform: rotateX(30deg);')
    ```

###### getPropFromMatrix(el)

* 描述：

    获取元素的 `transform` 变换属性值，该方法具有事实性，底层根据 `Matrix` 计算

* 参数：

    * `{Element} el` 元素

* 返回值：

    * `{Object}` 
        * `{Number} translateX` 元素当前 `translateX` 值
        * `{Number} translateY` 元素当前 `translateY` 值

###### getRelativeRect(parent, child)

* 描述：

    获取 子元素 到指定 父代元素 上、左 的距离

* 参数：

    * `{Element} parent` 父代元素
    * `{Element} child` 子元素

* 返回值：

    * `{Object}` 
        * `{Number} top` 子元素距离父代元素顶部的距离
        * `{Number} left` 子元素距离父代元素左边的距离

###### isInElement(parent, child)

* 描述：

    判断 元素`parent` 是否包含 元素`child`

* 参数：

    * `{Element} parent` 元素
    * `{Element} child` 元素

* 返回值：

    * `{Boolean}` true 代表 `parent` 包含元素 `child`，反之为 `false`

###### extend(to, from1, from2, from3...)

* 描述：

    对象属性的继承，同名属性覆盖。
    将 `from1, from2, from3...` 这些对象的属性继承到 对象 `to`

* 参数：

    * `{Object} to` 目标对象
    * `{Object} from*` 源对象

* 返回值：

    * `{Object}` 继承后的对象

###### now()

* 描述：

    获取当前时间戳，等价于 `Date.now()`

* 返回值：

    * `{Number}` 时间戳

###### getStyle(el, prototype)

* 描述：

    获取元素的CSS属性值，对 `getComputedStyle` 的封装

* 参数：

    * `{Element} el` 元素
    * `{String} prototype*` 属性名

* 返回值：

    * `{String}` 属性值

###### throwError(text)

* 描述：

    对 throw new TypeError(text)` 的封装

* 参数：

    * `{String} text*` 报错文案

#### 组件示例

以下代码展示了 `simulation-scroll-y` 插件的代码结构

```js
// 主函数，options 为用户调用插件时传递的参数
export default function (options) {
    // 次函数，Fmover 构造函数作为参数
    return function (Fmover) {
        // 从 Fmover构造函数中解构出要使用的工具函数
        const {
            Moved,
            transform,
            getStyle,
            extend,
            cssText,
            getRelativeRect,
            isInElement,
            throwError
        } = Fmover

        // 次函数要返回一个对象
        return {
            // init、start、move、end、cancel 五个方法是 finger-mover 内建处理的约定方法
            init (fmover) {
                
            },
            start (fingerd) {
                
            },
            move (fingerd) {
                
            },
            end (fingerd) {

            },
            cancel (fingerd) {
                
            },
            // scrollTo、loadEnd、refresh、refreshSize 是非内建处理的约定方法，可以借此暴露插件接口
            scrollTo () {

            },
            loadEnd () {

            },
            refresh () {

            },
            refreshSize () {

            }

        }

    }

}
```
