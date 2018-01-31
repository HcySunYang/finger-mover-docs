---
nav: zh-cn
search: zh-cn
---

## fmover-slide-x.js

#### 概述

`finger-mover` 垂直轮播插件：移动端轻量的、基本的垂直轮播实现

<p class="tip">垂直轮播拥有和水平轮播完全一致的接口，唯一不同的就是与水平轮播的轮播方向不同。</p>

* <a href="https://fmover.hcysun.me/example/demo/fmover-slide-y.html" target="_blank">垂直轮播 live demo</a>

<img src="../asset/qrcode/fmover-slide-y.png" width="200"/>

#### 体积

* 压缩后体积：4.48KB

#### 安装

```
npm install --save fmover-slide-y
```

`fmover-slide-y` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `fmoverSlideY`。

#### 快速使用

DOM 结构：

```html
<div class="slide-wrap">
    <div>one</div>
    <div>two</div>
    <div>three</div>
</div>
```

JavaScript：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})
```

DOM 结构需要两层：

* 包裹层：上面代码中，class 名为 `slide-wrap`(此 class 名称为自定义) 的 `div` 就是包裹层
* 轮播块层：包裹层下面的每一个 `元素子节点` 都将作为一个轮播块

包裹层之所以叫包裹层，是因为其高度等于所有轮播块的高度之和，那么问题来了，每一个轮播块的高度是多少？

这就涉及到 `fmover-slide-y` 与其他轮播框架或库的不同之处：

<p class="tip">
fmover-slide-y 会自动获取包裹层的父级元素的高度作为轮播块的高度
</p>

所以你可以在 包裹层 之外再加一层 DOM 元素作为父级元素，然后只需要控制该父级元素的高度即可控制轮播块的高度：

```html
<!-- #box 的控制权是用户的 -->
<div id="box">
    <div class="slide-wrap">
        <div>one</div>
        <div>two</div>
        <div>three</div>
    </div>
</div>
```

#### 选项

###### startSlide

* 类型：`Number`

* 默认值：`1`

* 详细：

    初始展示第几个轮播块

###### autoplay

* 类型：`Number`

* 默认值：`0`

* 详细：

    设置自动轮播的时间，单位 `ms`，如果设置为 `0`，代表不启用自动轮播

###### autoplayDir

* 类型：`String`

* 默认值：`top`

* 详细：

    设置轮播的方向，默认 `top` 代表 自下向上 轮播，如果设置为 `bottom`，则 自上向下轮播

###### loop

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否启用无限轮播

###### times

* 类型：`Number`

* 默认值：`500`

* 详细：

    每次轮播的时间间隔，单位 `ms`

###### bounce

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否启用边界弹性，该选项仅对不启用无限轮播时生效

###### touchable

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否开启手指滑动轮播，该属性的作用是在那些仅允许自动轮播，不允许手动轮播的情况下使用

#### 选项/事件

###### onInit

* 类型：`Function`

* 详细：

    初始化完成后同步调用

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onInit: function () {

            }
        })
    ]
})
```

###### onTouchStart

* 类型：`Function`

* 详细：

    `touchstart` 事件触发时同步调用，并将当前位置横坐标作为参数传递给回调函数

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTouchStart: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onTouchMove

* 类型：`Function`

* 详细：

    `touchmove` 事件触发时同步调用，并将当前位置横坐标作为参数传递给回调函数

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTouchMove: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onTouchEnd

* 类型：`Function`

* 详细：

    `touchend` 事件触发时同步调用

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTouchEnd: function () {
                
            }
        })
    ]
})
```

###### onTransStart

* 类型：`Function`

* 详细：

    惯性滚动开始时触发，无论是否切换轮播块，只要惯性滚动开始即触发，并将当前位置横坐标作为参数传递给回调函数。

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTransStart: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onTransMove

* 类型：`Function`

* 详细：

    惯性滚动时持续触发，并将当前位置横坐标作为参数传递给回调函数。

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTransMove: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onTransEnd

* 类型：`Function`

* 详细：

    惯性滚动结束时触发，并将当前位置横坐标作为参数传递给回调函数。

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onTransEnd: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onChangeStart

* 类型：`Function`

* 详细：

    轮播块开始切换时触发，将传递给回调函数两个参数：
    * `{Number} index` 即将显示的轮播块的索引
    * `{String} dir` 轮播的方向，与 `autoplayDir` 相同

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onChangeStart: function (index, dir) {
                console.log(index)
                console.log(dir)
            }
        })
    ]
})
```

###### onChange

* 类型：`Function`

* 详细：

    轮播块在切换过程中持续触发，并将当前位置横坐标作为参数传递给回调函数

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onChange: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onChangeEnd

* 类型：`Function`

* 详细：

    轮播块在切换完成后触发，并将当前位置横坐标作为参数传递给回调函数

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onChangeEnd: function (currentY) {
                console.log(currentY)
            }
        })
    ]
})
```

###### onRefresh

* 类型：`Function`

* 详细：

    调用 `refresh()` 方法刷新轮播后同步触发，并将刷新后轮播块的数量作为参数传递给回调函数

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY({
            onRefresh: function (slideNumber) {
                console.log(slideNumber)
            }
        })
    ]
})
```

#### 实例属性

###### slideIndex

* 类型：`Number`

* 详细：

    当前轮播块的索引值

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    alert(fm[0].slideIndex)
}, false)
```

###### slideNumber

* 类型：`Number`

* 详细：

    轮播块的数量

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    alert(fm[0].slideNumber)
}, false)
```

#### 实例方法

###### prev()

* 描述：

    切换到前一个轮播块

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    fm[0].prev()
}, false)
```

###### next()

* 描述：

    切换到后一个轮播块

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    fm[0].next()
}, false)
```

###### slideTo(index[, times])

* 描述：

    切换到指定轮播块

* 参数：
    * `{Number} index` 轮播块的索引
    * `{Number} times` 滑动到指定轮播块的时间，单位 `ms`，如果传递 `0`，代表立即切换到该轮播块，不传递该参数则使用选项中的 `times`

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    fm[0].slideTo(3, 3000)
}, false)
```

###### refresh([force])

* 描述：

    当轮播块的数量有变化时，需要手动调用该方法，让 `fmover-slide-x` 重新计算轮播块的数量，以及尺寸。

* 参数：
    * `{Boolean} force` 否是刷新至初始状态，如果为 `true`，刷新后轮播会跳到选项 `startSlide` 所指定的位置，不传或传递 `false`，则刷新的同时对轮播动作无任何影响

* 示例：

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideY()
    ]
})

btn.addEventListener('click', function () {
    fm[0].refresh()
}, false)
```





