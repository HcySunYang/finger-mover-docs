## fmover-slide-x.js

#### Overview

`finger-mover` vertical slideshow plugin: lightweight and simple rotate animate solution on mobile device.

<p class="tip">Vertical and horizontal slideshow have exactly the same interface, just rotate side difference between theme.</p>

* <a href="https://fmover.hcysun.me/example/demo/fmover-slide-y.html" target="_blank">vertical slideshow live demo</a>

<img src="../asset/qrcode/fmover-slide-y.png" width="200"/>

#### File Size

* Just 4.48KB after compression

#### Installation

```
npm install --save fmover-slide-y
```

`fmover-slide-y` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `fmoverSlideY` by serving as `<script>` tag.

#### Getting Started

DOM Struct:

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

2 DOM layers:

* Wrapper layer: the `div`  with class name `slide-wrap`(custom class name) in above code
* Slideshow layer: each `div` element under wrapper layer. Note that each slide must be `div` element.

Slideshow layer's height is equal to the sum of the heights of all slides. What's the height of each slide? You need to know the difference between `fmover-slide-y` and other slideshow plugins or framework:

<p class="tip">
fmover-slide-y will automatically acquire the height of the parental element of the wrapper layer as the height of the slide.
</p>

You can create a layer of DOM elements outside the wrapper layer as a parent element, only need to control the height of the parent element to control the height of the slide.

```html
<!-- The control of #box belongs to the user -->
<div id="box">
    <div class="slide-wrap">
        <div>one</div>
        <div>two</div>
        <div>three</div>
    </div>
</div>
```

#### Options

###### startSlide

* Type: `Number`

* Default: `1`

* Description:

    Specifies the initial display slide.

###### autoplay

* Type: `Number`

* Default: `0`

* Description:

    Set the time for automatic slide, unit `ms`, if set it to `0`, it means that disable automatic slide.

###### autoplayDir

* Type: `String`

* Default: `top`

* Description:

    Specifies the direction of slide, default value is `top`, if set it to `bottom` will be slide from top to bottom.

###### loop

* Type: `Boolean`

* Default: `true`

* Description:

    Specifies the carousel mode.

###### times

* Type: `Number`

* Default: `500`

* Description:

    Time interval for each slide, unit `ms`.

###### bounce

* Type: `Boolean`

* Default: `true`

* Description:

    Specifies the boundary elasticity, only works when disable the carousel mode.

###### touchable

* Type: `Boolean`

* Default: `true`

* Description:

    Specifies the finger sliding slide, only works when only enable auto play slide.

#### Options/Event

###### onInit

* Type: `Function`

* Description:

    Synchronous call after initialized.

* Example:

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

* Type: `Function`

* Description:

    Synchronous calling when `touchstart` event is triggered and pass the current location information as a parameter to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Synchronous calling when `touchmove` event is triggered and pass the current location information as a parameter to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Synchronous calling when `touchend` event is triggered.

* Example:

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

* Type: `Function`

* Description:

    Trigger when inertial scrolling starts that nothing to do with switching slides, and pass the current vertical and horizontal coordinates to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Trigger when inertial scrolling starts and pass the current position coordinates to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Trigger when inertial scrolling stop and pass the current vertical and horizontal coordinates to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Trigger when switching slides starts and pass the following parameters to the callback function:

    * `{Number} index` Next slide index
    * `{String} dir` slide direction, same as `autoplayDir`

* Example:

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

* Type: `Function`

* Description:

    Continuously trigger when switching slides and pass the current vertical and horizontal coordinates to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Trigger after slide switched and pass the current coordinates to the callback function.

* Example:

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

* Type: `Function`

* Description:

    Trigger after refresh slide by calling `refresh()` and pass the number of slides to the callback function.

* Example:

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

#### Instance Property

###### slideIndex

* Type: `Number`

* Description:

    Current slide index.

* Example:

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

* Type: `Number`

* Description:

    Number of the slides.

* Example:

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

#### Instance Method

###### prev()

* Description:

    Switch to the previous slide.

* Example:

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

* Description:

    Switch to the next slide.

* Example:

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

* Description:

    Switch to the slide by given index.

* Parameters:
    * `{Number} index` Slide index.
    * `{Number} times` The time sliding to the specified slide (unit `ms`), if set it to `0` can be switches to the specified slide immediately. Should be use `times` if not passing this parameters.


* Example:

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

* Description:

    When the number of slides changes, you need to manually call this method to make `fmover-slide-y` recalculate the number of slides and each slide size.

* Parameters:
    * `{Boolean} force` reset to initial state. If set it to `true`, will be shown specified slide by `startSlide` after refresh. There is no effect on the slide when refresh it if not passing this parameter or set it to `false`.

* Example:

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
