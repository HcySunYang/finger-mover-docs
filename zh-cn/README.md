---
nav: zh-cn
search: zh-cn
---

## finger-mover

#### 概述

`finger-mover` 是一个集成
[Fingerd(移动端以手指为单位的事件管理方案)](/zh-cn/package/fingerd)
和
[Moved(微型运动方案)](/zh-cn/package/moved)
为一体的移动端动效平台，`finger-mover` 本身并不能为你做什么，但是附带的提供了很多插件，例如：
[纵向模拟滚动(simulation-scroll-y.js)](/zh-cn/plugins/simulation-scroll-y)、
[横向模拟滚动(simulation-scroll-x.js)](/zh-cn/plugins/simulation-scroll-x)等等，配合这些插件，你可以轻松的实现移动端的一些运动组件。

#### 特性

* 体积小，压缩后：11.12kb

* 插件化，运动组件均以插件的形式提供，当然，你也可以开发你自己插件，请查看 [插件开发](/zh-cn/creating-plugins)

#### 安装

```
npm install --save finger-mover
```

`finger-mover` 以 `umd` 模块发布，所以你可以使用任何你想用的方式来使用它。如果直接使用 `<script>` 标签引用的话，那么将注册全局变量 `Fmover`。

#### 快速开始

```js
// 导入 finger-mover
import Fmover from 'finger-mover'
// 导入 纵向模拟滚动插件 simulation-scroll-y
import simulationScrollY from 'simulation-scroll-y'
// 导入 横向模拟滚动插件 simulation-scroll-x
import simulationScrollX from 'simulation-scroll-x'

// 同时使用 simulation-scroll-y 和 simulation-scroll-x 这两个插件，即可实现 2d 滚动
let fm = new Fmover({
    el: '#scroll-box',
    plugins: [
        simulationScrollX(),
        simulationScrollY()
    ]
})
```

* <a href="https://fmover.hcysun.me/example/demo/2d-scroll-demo.html" target="_blank">2d 滚动 live demo</a>

<img src="../asset/qrcode/2d-scroll-demo.png" width="200"/>

#### 访问插件对象

通过 `Fmover` 可以根据引入插件的顺序访问插件对象，以上面的代码为例：

```js
// fm[0] 访问第 1 个插件对象，即 `simulationScrollX`
// 调用该对象的实例方法
fm[0].scrollTo(-300, 1000)
```

#### 插件列表

###### [纵向模拟滚动](/zh-cn/plugins/simulation-scroll-y)

简介：移动端模拟滚动的库有很多，例如：iscroll、xscroll、better-scroll 等等，那为什么还要一个 `simulation-scroll-y` 呢？答案是：这并不是一个轮子。详情请查看：[对比现有解决方案，simulation-scroll-y的优势所在](/zh-cn/plugins/simulation-scroll-y?id=%E4%B8%8E%E7%8E%B0%E6%9C%89%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E7%9A%84%E5%AF%B9%E6%AF%94)。

###### [横向模拟滚动](/zh-cn/plugins/simulation-scroll-x)

简介：与 `simulation-scroll-y` 类似，不同的是 `simulation-scroll-x` 将创建横向的模拟滚动。

###### [2d滚动](/zh-cn/plugins/2d-scroll)

简介：2d滚动是上下左右四个方向都可以进行滚动操作，2d滚动实际上并不是一个插件，因为要实现2d滚动，你只需要同时使用 `simulation-scroll-x` 插件和 `simulation-scroll-y` 插件即可。

###### [水平轮播](/zh-cn/plugins/fmover-slide-x)

简介：移动端轻量的、基本的轮播图实现(水平方向)。

###### [垂直轮播](/zh-cn/plugins/fmover-slide-y)

简介：移动端轻量的、基本的轮播图实现(垂直方向)。

#### 自定义插件

你可以自己开发一个 `finger-mover` 插件，请查看 [插件开发](/zh-cn/creating-plugins)

#### Package

###### [Fingerd](/zh-cn/package/fingerd)

> 移动端以手指为单位的事件管理方案

###### [Moved](/zh-cn/package/moved)

> 微型运动方案