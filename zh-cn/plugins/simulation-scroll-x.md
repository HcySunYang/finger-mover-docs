---
nav: zh-cn
search: zh-cn
---

## simulation-scroll-x.js

#### 概述

移动端横向模拟滚动

#### 安装

```
npm install --save simulation-scroll-x
```

`simulation-scroll-x` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `simulationScrollX`。

#### 快速使用

```html
<div id="box">
    <div id="scroll-box">

    </div>
</div>

<!-- 引入 finger-mover -->
<script src="finger-mover.js"></script>
<!-- 引入 simulation-scroll-x -->
<script src="simulation-scroll-x.js"></script>
<script>
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX()
        ]
    })
</script>
```

以上代码将实现基本的横向模拟滚动

* [基本的横向模拟滚动 live demo]()

#### 特性

* 压缩后只有 3.11kb

* 多手指触摸滚动元素时，并不会阻止滚动，且手指间的滚动衔接流畅，该功能的实现得益于[Fingerd](/package/fingerd)

* 当手指划出屏幕时不会影响运行。在很多手机浏览器中，当手指划出屏幕时，现有开源解决方案中的运动会卡住

* 可以配合 [simulation-scroll-y.js](/plugins/simulation-scroll-y) 实现 2d 滚动

* 支持滚动条

#### 选项

###### scrollBar

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否显示滚动条

###### bounce

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否开启边界弹性

#### 选项/事件

<p class="tip">
    注意：以下事件选项的回调函数的作用域为 插件实例对象
</p>

###### onTouchMove

* 类型：`Function`

* 详细：

    当手指拖动滚动元素滚动时持续触发，并将滚动元素当前的位置作为参数传递给回调函数。

* 示例：

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTouchMove: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

###### onTransMove

* 类型：`Function`

* 详细：

    惯性滚动时持续触发。并将滚动元素当前的位置作为参数传递给回调函数。

* 示例：

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTransMove: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

###### onTransMoveEnd

* 类型：`Function`

* 详细：

    惯性滚动结束时触发。并将滚动元素当前的位置作为参数传递给回调函数。

* 示例：

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTransMoveEnd: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

#### 实例方法

###### scrollTo(target, time)

* 描述：

    滚动至目标点

* 参数：

    * `{Number | Element | String} target` 滚动至的目标点 或 目标元素 或 目标元素选择器
    * `{Number} time` 滚动的时间，毫秒数

* 示例：

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX()
        ]
    })

    // 用 1 秒的时间滚动到 -300px
    fm[0].scrollTo(-300, 1000)
    ```

    将时间设置为 0，即可瞬间到达目标点

    ```js
    fm[0].scrollTo(-300, 0)
    ```

    除了直接设置数字目标点外，scrollTo支持滚动至指定的DOM元素，只需要将DOM元素 或 该元素的CSS选择器作为第一个参数即可：

    ```js
    let p = document.querySelector('p')
    fm[0].scrollTo(p, 1000)
    // 或
    fm[0].scrpllTo('.wrap', 1000)
    ```

    * [滚动至目标点 live demo]()

###### refreshSize()

* 描述：

    重新计算滚动元素的尺寸，滚动限制等等

