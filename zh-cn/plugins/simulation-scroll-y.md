---
nav: zh-cn
search: zh-cn
---

## simulation-scroll-y.js

#### 概述

移动端纵向模拟滚动

#### 安装

```
npm install --save simulation-scroll-y
```

`simulation-scroll-y` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `simulationScrollY`。

#### 快速使用

```html
<div id="box">
    <div id="scroll-box">

    </div>
</div>

<!-- 引入 finger-mover -->
<script src="finger-mover.js"></script>
<!-- 引入 simulation-scroll-y -->
<script src="simulation-scroll-y.js"></script>
<script>
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollY()
        ]
    })
</script>
```

以上代码将实现基本的纵向模拟滚动

* <a href="https://fmover.hcysun.me/example/demo/simulation-y-demo.html" target="_blank">基本的纵向模拟滚动 live demo</a>

<img src="../asset/qrcode/simulation-y-demo.png" width="200"/>

#### 特性

* 压缩后只有 4kb

* 多手指触摸滚动元素时，并不会阻止滚动，且手指间的滚动衔接流畅，该功能的实现得益于[Fingerd](/zh-cn/package/fingerd)

* 当手指划出屏幕时不会影响运行。在很多手机浏览器中，当手指划出屏幕时，现有开源解决方案中的运动会卡住

* 支持下拉刷新，且给你完全自定义的机会

* 支持无限滚动(加载更多)

* 可以配合 [simulation-scroll-x.js](/zh-cn/plugins/simulation-scroll-x) 实现 2d 滚动

* 支持滚动条

#### 与现有解决方案的对比

移动端模拟滚动的库有很多，例如：iscroll、xscroll、better-scroll 等等，那为什么还要一个 `simulation-scroll-y` 呢？答案是：这并不是一个轮子。不妨做如下对比：

###### 一、库体积

库体积很大一部分是由框架所提供的功能决定的，但是在完成相同功能的前提下，simulation-scroll-y 应该算是最小的。

simulation-scroll-y 作为 finger-mover 的插件，其体积只有 4.42kb，即使加上 finger-mover 本身的 11.12kb 也才只有 15.54kb，关键是 finger-mover 还提供了其他移动的插件，供你选择。

如果你仅仅要实现一个模拟滚动已经下拉刷新、无限滚动的功能，那么使用 simulation-scroll-y 要比你一次性的引入其他框架如 iscroll 等要小的多。如果你要实现轮播图，完全可以使用 finger-mover 的轮播图插件，这比你使用 swiper 要划算的多。

###### 二、IOS 系统部分浏览器，手指划出屏幕，运动卡住

在UC浏览器，手机QQ内的webview 都会出现此类问题。

看下图：

![运动卡住](../asset/othergif/out-stop.gif)

而 `simulation-scroll-y` 则不会出现这样的问题：

![运动不会卡住](../asset/fm/out-no-stop.gif)

###### 三、多手指操作问题

大多数的实现中，当多指操作时，会阻止运动，如下图：

![运动卡住](../asset/othergif/more-finger-stop.gif)

另外一些不阻止滚动的实现中，虽然能滚动，但是也会出现奇葩的bug，如下图所示：

在这个实现中，虽然当第二根手指触摸屏幕时依然能够滚动，但是当拿开第一个手指时，连续滑动第二根手指，第二根手指并不能操作滚动元素，且在第二根手指滑动的过程中，连续使用第一根手指点按屏幕，会出现奇葩滚动动作。

![多指操作奇葩bug](../asset/othergif/qipa-bug.gif)

以上问题，在 `simulation-scroll-y` 则不会出现，无论有几根手指触摸屏幕，`simulation-scroll-y` 都将控制权交给第一根手指，如果第一个手指拿开，那么第二根手指将继承第一根手指的状态，完美衔接滚动，此功能的实现得益于 [Fingerd](/zh-cn/package/fingerd)，如下图：

![运动不会卡住](../asset/fm/more-finger-no-stop.gif)

###### 四、下拉刷新的问题

一些现有实现中，在下拉刷新后，迅速上滑，然后再滑动下来，你会发现下拉挂掉了，永远收不回去，如下图：

![多指操作奇葩bug](../asset/othergif/pulldown-bug.gif)

另外的一些实现则规定了下拉刷新的形式：下拉后是停留等待 还是 不停留等待

而 `simulation-scroll-y` 则是完全给你自定义下拉操作的机会，你可以实现任何形式的下拉刷新操作，另外 `simulation-scroll-y` 在你滚动页面的过程中完全不会影响下拉操作，详情请查看 `pullDown` 选项。

#### 选项

###### scrollBar

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否显示滚动条

###### unidirectional

* 类型：`Boolean`

* 默认值：`false`

* 详细：

    是否单向滑动，单向滑动顾名思义，指的是单方向的滑动，即：如果左右滑动了，那么就不允许上下滑动，如果上下滑动了，就不允许左右滑动。该选项用来配合 `fmover-slide-x` 插件编写选项卡组件时非常有用。请查看Demo：

    * <a href="https://fmover.hcysun.me/example/demo/slide-with-scroll.html" target="_blank">选项卡组件</a>

    * <img src="../asset/qrcode/fingerd-scale-demo.png" width="200"/>

###### bounce

* 类型：`Boolean`

* 默认值：`true`

* 详细：

    是否开启边界弹性

###### pullDown

* 类型：`Object`

* 默认值：

    ```js
    {
        use: false,
        distance: 50,
        onBegin: function(currentY){},
        onActive: function(){},
        onAfter: function(currentY){}
    }
    ```

* 详细：

    下拉刷新配置

    ```js
    {
        // 是否启用下拉刷新，默认不启用
        use: false,
        // 设置下拉距离，达到该距离则触发下拉动作，触发 onActive 事件
        distance: 50,
        // 从开始下拉 到 触发 onActive 事件 之间持续触发，并传递滚动元素实时位置作为参数
        onBegin: function(currentY){},
        // 下拉距离大于等于 distance 所设置的距离时触发
        onActive: function(){},
        // 有两种情况会触发 onAfter 事件：
        // 1、下拉动作未达到 distance 即停止，此时滚动元素将缩回，在缩回的过程中持续触发。
        // 2、调用 this.refresh() 时，在滚动元素缩回的过程中持续触发
        // 触发 onAfter 的时，会传递滚动元素实时位置作为参数
        onAfter: function(currentY){}
    }
    ```

    <p class="tip">
    注意：`onBegin`、`onActive` 以及 `onAfter` 事件函数的 `this` 指向为 插件实例对象。
    </p>

* 示例：

    `simulation-scroll-y` 与其他框架另外一个不同的地方就是，将下拉的控制权完全交给使用者。这样的好处是，你可以实现任意形式的下拉刷新动作，比如下拉后是停留等待还是不停留等待，以及你想要的DOM元素结构样式，一切都交由你控制，实现起来并不难：

    假设我们有如下DOM结构：

    ```html
    <div id="box">
        <div id="scroll-box">
            <div class="pull-down-dom">下拉刷新</div>

            <div class="content">省略 100 个li</div>
        </div>
    </div>
    ```

    通过下面的js代码，很容易实现一个显示下拉进度的刷新功能

    ```js
    <script src="finger-mover.js"></script>
    <script src="simulation-scroll-y.js"></script>
    <script>
    // 获取下拉刷新DOM
    var pullDownDom = document.querySelector('.pull-down-dom')
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollY({
                pullDown: {
                    use: true,
                    distance: 50,
                    onBegin: function (currentY) {
                        // 使用 currentY 计算出下拉百分比
                        var proportion = parseInt(currentY / 50 * 100)

                        if (proportion >= 100) {
                            proportion = 100
                            pullDownDom.innerHTML = '释放加载'
                        } else {
                            pullDownDom.innerHTML = proportion + '%'
                        }

                    },
                    onActive: function () {
                        var that = this
                        pullDownDom.innerHTML = '加载中...'
                        
                        // 模拟发送网络请求
                        setTimeout(function () {
                            // 请求数据成功后，调用 refresh() 方法
                            that.refresh(function () {
                                console.log('更新完成')
                                pullDownDom.innerHTML = '下拉刷新'
                            })
                        }, 2000)
                    },
                    onAfter: function (currentY) {
                        if (currentY < 50) {
                            pullDownDom.innerHTML = '下拉刷新'
                        }
                    }
                }
            })
        ]
    })
    </script>
    ```

    <p class="tip">
        注意：请求数据成功后的更新操作，一定要在 refresh() 的回调函数中完成：
    </p>

    ```js
    this.refresh(function () {
        console.log('更新完成')
    })
    ```

    * <a href="https://fmover.hcysun.me/example/demo/pull-down-demo.html" target="_blank">下拉刷新 live demo</a>

    <img src="../asset/qrcode/pull-down-demo.png" width="200"/>

###### loadMore

* 类型：`Object`

* 默认值：

    ```js
    {
        distance: 0,
        onLoadMore: function(){}
    }
    ```

* 详细：

    无限滚动配置

    ```js
    {
        // distance 代表距离底部的距离，默认为0，即当滚动到最底部时触发 onLoadMore 事件
        distance: 0,
        // 滚动元素滚动至距底部的距离小于等于 distance 时触发
        onLoadMore: function(){}
    }
    ```

    <p class="tip">注意：`onLoadMore` 事件函数的 `this` 指向为 插件对象。</p>

* 示例：

    与下拉刷新一样，`loadMore` 也将控制权完全交给开发者，假设我们有如下DOM结构

    ```html
    <div id="box">
        <div id="scroll-box">
            <div class="content"></div>

            <div class="load-more">加载更多</div>
        </div>
    </div>
    ```

    通过下面的js代码，很容易实现一个无限滚动的功能：

    ```
    <script src="finger-mover.js"></script>
    <script src="simulation-scroll-y.js"></script>
    <script>
    // 获取加载更多DOM
    var loadMoreDom = document.querySelector('.load-more')
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollY({
                loadMore: {
                    distance: 0,
                    onLoadMore: function () {
                        var that = this
                        loadMoreDom.innerHTML = '加载中...'

                        // 模拟发送网络请求
                        setTimeout(function () {
                            loadMoreDom.innerHTML = '加载更多'
                            // 调用 loadEnd() 方法重置加载更多操作
                            that.loadEnd()
                        }, 2000)
                    }

                }
            })
      ]
    })
    </script>
    ```

    <p class="tip">
    注意：在加载更多请求数据成功后，要调用 `fm.loadEnd()` 方法重置加载更多操作
    </p>

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
            simulationScrollY({
                onTouchMove: function (currentY) {
                    console.log(currentY)
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
            simulationScrollY({
                onTransMove: function (currentY) {
                    console.log(currentY)
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
            simulationScrollY({
                onTransMoveEnd: function (currentY) {
                    console.log(currentY)
                }
            })
        ]
    })
    ```

#### 实例方法

###### scrollTo(target, time, limit)

* 描述：

    滚动至目标点

* 参数：

    * `{Number | Element | String} target` 滚动至的目标点 或 目标元素 或 目标元素选择器
    * `{Number} time` 滚动的时间，毫秒数
    * `{Boolean} limit` 是否开启边界限制，默认不开启，开启限制后，使用 `scrollTo` 方法滚动元素到任何位置不受边界限制

* 示例：

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollY()
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

    * <a href="https://fmover.hcysun.me/example/demo/scroll-to-y.html" target="_blank">滚动至目标点 live demo</a>

    <img src="../asset/qrcode/scroll-to-y.png" width="200"/>

###### loadEnd()

* 描述：

    重置 加载更多(无限滚动)

    当加载更多触发后，即 `onLoadMore` 事件触发后，`simulation-scroll-y` 会锁住该事件，以防止上一次网络请求未完成后的重复触发，所以当网络请求成功后，需要手动调用该方法对加载更多进行重置，另外 `loadEnd` 方法也会重新计算滚动尺寸。


###### refresh(callBack)

* 描述：

    类似 `loadEnd`，`refresh` 方法用来重置下拉刷新，不同的是，下拉刷新后的数据更新操作，需要在 `callBack` 中完成，这是因为：触发下拉刷新事件后，在加载数据的过程中，用户很有可能对页面进行了滚动，为了防止不必要的bug，真正的更新操作是在用户停止对页面进行操作的时候完成的，这就需要一个缓冲的机制，而 `callBack` 就是为了实现这一缓冲机制，所以更新数据的操作要写在 `callBack` 内。

    同时，`refresh` 方法也会重新计算滚动尺寸

* 参数：

    * `{Function} callBack` refresh 操作的回调函数

###### refreshSize()

* 描述：

    重新计算滚动元素的尺寸，滚动限制等等

    由于 `refresh` 方法和 `loadEnd` 方法已经自动包含了重新计算尺寸的工作，所以一般情况下，我们不需要手动调用 `refreshSize` 去重新计算。

#### demo 汇总

基于 `simulation-scroll-y` 插件可以实现很多效果及功能，查看：[Full demo](/zh-cn/full-demo)













