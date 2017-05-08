## 2D Scroll

#### Overview

Can be rolling in the vertical, horizontal direction.

To implement 2D scroll doesn't need a extra plugin in `finger-mover` you just need to use the vertical scrolling plugin `simulation-scroll-y` and horizontal scrolling plugin `simulation-scroll-x` at the same time.

#### Installation

```
npm install --save simulation-scroll-x
npm install --save simulation-scroll-y
```

#### Getting Started

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

The above code will implement 2D scrolling.

* [2D Scroll live demo]()



