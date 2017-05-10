---
nav: zh-cn
search: zh-cn
---

## 概述

`Moved` 是一个微型运动框架

## 特性

* 体积小，压缩后 2.55kb

* 只做 css3 中 transform 属性的运动

* 使用 `requestAnimationFrame` 并在不支持的浏览器中自动降级到 `setTimeout(fn, 1000 / 60)`

## 安装

```
npm install --save moved
```

`Moved` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `Moved`。

## 快速开始

让一个元素以 2 两秒钟的时间运动其 `translateX`、`translateY` 属性分别至 200px 的位置：

```js
// 获取元素
var box = document.querySelector('.box')
// 实例化 Moved 对象
var moved = new Moved()

// 运动
moved.start({
    el: box,
    target: {
        translateX: 200,
        translateY: 200,
    },
    time: 2000
})
```

* <a href="https://fmover.hcysun.me/example/demo/mover-easy-demo.html" target="_blank">基本运动 live demo</a>

<img src="../asset/qrcode/mover-easy-demo.png" width="200"/>

## 接口明细

#### 实例属性

###### moveStatus

* 类型：`String`

* 默认值：`stop`

* 详细：

    元素的运动状态，`stop` 代表元素处于停止状态，`start` 代表元素处于正在运动的状态。

#### 实例方法

###### start(options)

* 描述：

    开始一个运动

* 参数：

    * `{Object} options` 滚动至的目标点 或 目标元素 或 目标元素选择器
        * `{Element} el` 要运动的元素
        * `{String} type` 运行类型，采用 Tween 算法，内置两种运行形式：
            * `easeOutStrong`
            * `backOut`
            * 默认值 `easeOutStrong`，可扩展
        * `{Object} target` 运动至的目标点
            * `{Number} translateX` X 方向位移，单位 `px`
            * `{Number} translateY` Y 方向位移，单位 `px`
            * `{Number} translateZ` Z 方向位移，单位 `px`
            * `{Number} scaleX` X 方向缩放，单位 `deg`
            * `{Number} scaleY` Y 方向缩放，单位 `deg`
            * `{Number} scaleZ` Z 方向缩放，单位 `deg`
            * `{Number} rotateX` Z 方向缩放，单位 `deg`
            * `{Number} rotateY` Z 方向缩放，单位 `deg`
            * `{Number} rotateZ` Z 方向缩放，单位 `deg`
            * `{Number} skewX` Z 方向缩放，单位 `deg`
            * `{Number} skewY` Z 方向缩放，单位 `deg`
        * `{Number} time` 运动的时间
        * `{Function} inCallBack` 运动进行时持续触发的回调函数，并传递当前位置信息作为参数
        * `{Function} endCallBack` 运动结束时触发的回调函数，并传递当前位置信息作为参数

###### stop(callback)

* 描述：

    停止运动

* 参数：

    * `{Function} callback` 停止运动时的回调函数，并传递停止运动那一刻的位置信息作为参数

###### transform(el, attr, val)

* 描述：

    设置元素的transform CSS属性

* 参数：

    * `{Element} el` 元素
    * `{String} attr` 元素的 transform 属性，如：`translateX`、`translateY`... 等等
    * `{Number} val` 属性值

* <a href="https://fmover.hcysun.me/example/demo/mover-demo.html" target="_blank">start()、stop()、transform() live demo</a>

<img src="../asset/qrcode/mover-demo.png" width="200"/>

## 扩展运动类型

调用 `start` 方法开始运动时，会传递一个运动类型 `type`，`Moved` 内置两种运动类型：

* `easeOutStrong`
* `backOut`

扩展更多运动类型很简单，只需要将该运行类型添加到 `Moved.Tween` 属性即可：

```js
Moved.Tween.linear = function (t, b, c, d) {
    return c * t / d + b
}
```

接下来，就可以使用类型 `type: linear` 了：

```
moved.start({
    el: box,
    type: 'linear',
    target: {
        translateX: 200,
        translateY: 200,
    },
    time: 2000
})
```