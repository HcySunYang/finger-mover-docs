---
nav: zh-cn
search: zh-cn
---

## 概述

`Fingerd` 是一个移动端以手指为单位的事件管理方案，`Fingerd` 并不会为你做事件监听，你只需要在相应的事件监听函数中将原生事件对象(event)注入给 `Fingerd` 即可，`Fingerd` 将为你提供一个包含许多有用信息的手指集合。

## 安装

```
npm install --save fingerd
```

`fingerd` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `Fingerd`。

## 快速开始

```js
let el = document.querySelector('#box')

// 创建 Fingerd 对象，并传入一个元素，作为手指的作用范围，通常这个元素与你监听事件的元素相同即可
f = new Fingerd({
    element: el
})

// 在相应的事件中，将原生的事件对象注入即可。打印对象 `f` 你将得到 `Fingerd` 为你提供的诸多信息
el.addEventListener('touchstart', function (event) {
    f.injectEvent(event)

    console.log(f)
})

el.addEventListener('touchmove', function (event) {
    f.injectEvent(event)

    console.log(f)
})

el.addEventListener('touchend', function (event) {
    f.injectEvent(event)

    console.log(f)
})

el.addEventListener('touchcancel', function (event) {
    f.injectEvent(event)

    console.log(f)
})

```

上面代码中，我们首先实例化了一个 `Fingerd` 对象，然后分别在四个事件 `touchstart`、`touchmove`、`touchend` 以及 `touchcancel` 中调用 `injectEvent(event)` 方法，将原生事件对象注入。需要注意一点：

<p class="warning">
必须至少在 `touchstart`、`touchmove` 以及 `touchend` 这三个事件函数中调用 `injectEvent()` 方法，这样 `Fingerd` 才能正确的帮你管理手指，这么做并不会让你写多余的代码，因为适合使用 `Fingerd` 开发的项目中，99.99% 的项目需要至少这三个事件来完成。
</p>

## 简单的概念

我们有一些很简单的概念，这对你理解 `Fingerd` 会有所帮助

#### 作用范围

<p class="tip">
在实例化 `Fingerd` 对象时，我们需要传递一个元素，该元素的区域即是作用范围，当手指划出该范围或抬起手指时，视为手指离开
</p>

#### 手指顺序

<p class="tip">
以触摸作用范围的先后顺序决定手指的顺序，即第一个触摸作用范围的手指为第一个手指，以此类推
</p>

#### 手指状态

<p class="tip">
每个手指都会维护一个仅针对该手指的数据状态，例如移动距离，瞬时速度，移动方向等，且不同手指间数据状态不会相互影响
</p>

#### 手指继承

<p class="tip">
当某一个手指离开作用区域(划出作用范围 或 抬起手指)时，按照*手指顺序*，如果该手指的后面还有手指的话，那么其后面的手指应该继承离开*作用范围*的手指的*数据状态*，如果该手指后面没有手指，那么将该手指的*数据状态*清除
</p>

## 选项、属性、方法、数据状态

#### 选项

###### element

* 类型：`Element`

* 必须：参数必传

* 详细：

    该元素的范围将作为手指的作用范围

###### transTime

* 类型：`Number`

* 必须：否

* 默认值：`3000`

* 详细：

    在计算手指滑动的加速度时(`xa`、`ya`)，使用到了物理公式如下：

    ```js
    Vt ＝ Vo + at
    a = (Vt - Vo) / t
    ```

    其中 `t` 所代表的即 `transTime` 参数，也就是说该参数将会影响 `xa`、`ya` 的值，`xa`、`ya` 是手指的数据状态，下面有介绍。

    在计算惯性滚动距离时(`transDistanceX`、`transDistanceY`)，使用到如下物理公式：

    ```
    X = Vot + at^2 / 2
    ```

    其中 `a` 即通过 `transTime` 运算得出的，也就是说 `transTime` 会间接影响 `transDistanceX`、`transDistanceY` 的值。

    总结：`transTime` 值越大，加速度越小，惯性滚动时间越长。

#### 实例属性

###### element

* 类型：`Element`

* 详细：

    返回一个DOM元素，该DOM元素为创建 Fingerd 对象时传递的 作用范围 元素

###### fingers

* 类型：`Array`

* 详细：

    返回一个数组，存储作用范围内所有的手指，数组的第0个元素即代表第一根手指，以此类推

###### changeFingerIndex

* 类型：`Number`

* 详细：

    返回一个数字索引，该索引指向 `fingers` 数组，代表第几个手指发生了改变，在不同的事件中意义不同，如：在 `touchstart` 中代表着第几个手指触发了 `touchstart` 事件，在 `touchmove` 中则代表第几个手指触发了 `touchmove` 事件。

###### fingerNumber

* 类型：`Number`

* 详细：

    作用范围内的手指数量

    前面讲过 `fingers` 将存储作用范围内的所有手指，那么 `fingers.length` 和 `fingerNumber` 有什么不同呢？区别在于当最后一根手指离开作用范围时，`fingerNumber` 的值为 0，而 `fingers` 的长度为 1，因为其中保存着最后一根手里离开那一刻的数据状态

    另外 `fingerNumber` 相对于原生 `event` 事件对象的 `touches.length` 的另外一个不同之处是：`fingerNumber` 是真正的作用范围内的手指数量，查看这个demo即可：

    * [fingerNumber 与 touches.length 的区别 live demo]()

#### 实例方法

###### injectEvent(event)

* 参数：

    * `{Object} event`：原生 event 事件对象

* 描述：

    注入原生event对象，Fingerd 将用其进行数据运算

* 返回值：`{Fingerd} Fingerd 实例对象本身`

###### getFingerPicth(f1, f2)

* 参数：

    * `{fingers} f1`：`fingers` 数组中的任意一个手指
    * `{fingers} f2`：`fingers` 数组中的任意一个手指

* 描述：

    获取任意两个手指间的距离

* 返回值：`{Number} 两个手指间的距离`

###### getScale(f1, f2)

* 参数：

    * `{fingers} f1`：`fingers` 数组中的任意一个手指
    * `{fingers} f2`：`fingers` 数组中的任意一个手指

* 描述：

    根据两个手指，获取缩放比例

* 返回值：`{Number} 缩放比例`

* 示例：

```js
box.addEventListener('touchstart', function (event) {
    fg.injectEvent(event)
    event.preventDefault()
}, false)

box.addEventListener('touchmove', function (event) {
    fg.injectEvent(event)
    // 当作用元素上的手指数量大于 1 时，获取第一根和第二根手指的缩放比例
    if (fg.fingerNumber > 1) {
        var dis = fg.getScale(fg.fingers[0], fg.fingers[1])
        console.log('缩放比例为：' + dis)
    }
    event.preventDefault()
}, false)

box.addEventListener('touchend', function (event) {
    fg.injectEvent(event)
    event.preventDefault()
}, false)
```

* [live demo ---- 缩放]()
    
#### 每个手指的数据状态(Tevent)

除了实例属性中几个和手指状态有关的通用属性外，更多的与每个手指有关的数据状态存储在一个 Tevent 对象中，如我们在 `touchmove` 中可以这样访问：

```js
el.addEventListener('touchmove', function (event) {
    f.injectEvent(event)

    // 获取包含所有手指的数组
    let fingers = f.fingers
    // 读取第一个手指的数据状态
    let f1Data = fingers[0]

    // 打印数据
    console.log(f1Data)
})
```

在打印的 `fingerData` 数据中将包含以下信息：

###### native

* 类型：`Object`

* 详细：

    原生 `event` 事件对象

###### startX

* 类型：`Number`

* 详细：

    手指触摸作用元素那一刻其X坐标，等价于原生 `event` 对象的 `event.pageX`

###### startY

* 类型：`Number`

* 详细：

    手指触摸作用元素那一刻其Y坐标，等价于原生 `event` 对象的 `event.pageY`

###### startTime

* 类型：`Number`

* 详细：

    手指触摸作用范围那一刻的时间戳

###### lastX

* 类型：`Number`

* 详细：

    初始值等同于 startX，当手指移动时，其将保存上一次 touchmove 事件触发时手指的位置横坐标

###### lastY

* 类型：`Number`

* 详细：

    初始值等同于 startY，当手指移动时，其将保存上一次 touchmove 事件触发时手指的位置纵坐标

###### lastTime

* 类型：`Number`

* 详细：

    初始值等同于 startTime，当手指移动时，其将保存上一次 touchmove 事件触发时的时间戳

###### xs

* 类型：`Number`

* 详细：

    保存着手指横向移动的瞬时位移

###### ys

* 类型：`Number`

* 详细：

    保存着手指纵向移动的瞬时位移

###### xt

* 类型：`Number`

* 详细：

    保存着手指横向移动的瞬时时间

###### yt

* 类型：`Number`

* 详细：

    保存着手指纵向移动的瞬时时间

###### xv

* 类型：`Number`

* 详细：

    保存着手指横向移动的瞬时速度

###### yv

* 类型：`Number`

* 详细：

    保存着手指纵向移动的瞬时速度

###### xa

* 类型：`Number`

* 详细：

    保存着手指横向移动的加速度

###### ya

* 类型：`Number`

* 详细：

    保存着手指纵向移动的加速度

###### distanceX

* 类型：`Number`

* 详细：

    手指横向滑动的距离

###### distanceY

* 类型：`Number`

* 详细：

    手指纵向滑动的距离

###### transDistanceX

* 类型：`Number`

* 详细：

    当手指滑动后离开屏幕时，该属性保存着系统推荐该元素应该横向惯性滑动的距离

###### transDistanceY

* 类型：`Number`

* 详细：

    当手指滑动后离开屏幕时，该属性保存着系统推荐该元素应该纵向惯性滑动的距离

###### transTime

* 类型：`Number`

* 详细：

    该属性保存着元素应该惯性滑动的时间

###### fingerInElement

* 类型：`Boolean`

* 详细：

    手指是否在作用元素内，true 在，false 不在

###### direction

* 类型：`Object`

* 详细：

    手指滑动方向，保存8个方向，使用boolean值表示是否向该方向滑动

    ```
    {
        top,
        bottom,
        left,
        right,
        topLeft,
        topRight,
        bottomLeft,
        bottomRight
    }
    ```







