---
nav: zh-cn
search: zh-cn
---

## 2d滚动

#### 概述

纵向、横向两个方向可滚动。

在 `finger-mover` 中，2d滚动并不需要单独的插件，你只需要同时使用纵向滚动插件 `simulation-scroll-y` 和 横向滚动插件 `simulation-scroll-x` 即可。

#### 安装

```
npm install --save simulation-scroll-x
npm install --save simulation-scroll-y
```

#### 快速使用

```html
<div id="box">
    <div id="scroll-box">

    </div>
</div>

<script src="finger-mover.js"></script>
<script src="simulation-scroll-x.js"></script>
<script src="simulation-scroll-y.js"></script>
<script>
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX(),
            simulationScrollY()
        ]
    })
</script>
```

以上代码将实现2d滚动

* [2d滚动 live demo]()



