## Overview

`Moved` is a micro movement framework.

## Feature

* Small size, just 3.17KB after compression

* Only include transform attribute of the movement in the CSS3

* Using `requestAnimationFrame` and automatically downgrade to `setTimeout(fn, 1000 / 60)` in unsupported browsers

## Installation

```
npm install --save moved
```

`Moved` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `Moved` by serving as `<script>` tag.

## Getting Started

Let an element move in 2 seconds and make its `translateX` and `translateY` attribute to 200px respectively:

```js
// Get the element
var box = document.querySelector('.box')
// Instantiate the Moved object
var moved = new Moved()

// Movement
moved.start({
    el: box,
    target: {
        translateX: 200,
        translateY: 200,
    },
    time: 2000
})
```

* <a href="https://fmover.hcysun.me/example/demo/mover-easy-demo.html" target="_blank">Basic Movement live demo</a>

<img src="../asset/qrcode/mover-easy-demo.png" width="200"/>

## Interface

#### Instance Properties

###### moveStatus

* Type: `String`

* Default: `stop`

* Description:

    The movement state of the element, `stop` represents the element in a stopped state and `start` represents element is in motion.

#### Instance Method

###### start(options)

* Description:

    Start a movement.

* Parameters:

    * `{Object} options` Scroll to the target point, target element or target element selector
        * `{Element} el` The element to move
        * `{String} type` Type of operation, using Tween algorithm, built two operational forms:
            * `easeOutStrong`
            * `backOut`
            * Default value is `easeOutStrong`, scalable
        * `{Object} target` Moving to the target point
            * `{Number} translateX` X direction displacement, unit `px`
            * `{Number} translateY` Y direction displacement, unit `px`
            * `{Number} translateZ` Z direction displacement, unit `px`
            * `{Number} scaleX` X direction scaling, unit `deg`
            * `{Number} scaleY` Y direction scaling, unit `deg`
            * `{Number} scaleZ` Z direction scaling, unit `deg`
            * `{Number} rotateX` X direction rotating, unit `deg`
            * `{Number} rotateY` Y direction rotating, unit `deg`
            * `{Number} rotateZ` Z direction rotating, unit `deg`
            * `{Number} skewX` X direction skewing, unit `deg`
            * `{Number} skewY` Y direction skewing, unit `deg`
        * `{Number} time` Movement time
        * `{Function} inCallBack` The callback function that continues to trigger when the movement is in progress with the current location information as a parameter.
        * `{Function} endCallBack` The callback function is triggered at the end of the movement with the current position information as a parameter.

###### stop(callback)

* Description:

    Stop a movement.

* Parameters:

    * `{Function} callback` The callback function is triggered at the end of the movement with the current position information as a parameter.

###### transform(el, attr, val)

* Description:

    Sets the transform CSS property of the element.

* Parameters:

    * `{Element} el` Element
    * `{String} attr` The transform CSS property of the element. Such as: `translateX`, `translateY` and etc.,
    * `{Number} val` Property value

* <a href="https://fmover.hcysun.me/example/demo/mover-demo.html" target="_blank">start()、stop()、transform() live demo</a>

<img src="../asset/qrcode/mover-demo.png" width="200"/>

## Extended Movement Type

Call the method `start` to start the movement with a movement type `type`, `Moved` built-in two types of movement:

* `easeOutStrong`
* `backOut`

Extend more types of motion by add the runtime type to `Moved.Tween`:

```js
Moved.Tween.linear = function (t, b, c, d) {
    return c * t / d + b
}
```

Now we can use the type `type: linear`:

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