## fmover-slide-x.js

#### Overview

`finger-mover` horizontal slideshow plugin: lightweight and simple rotate animate solution on mobile device.

* <a href="https://fmover.hcysun.me/example/demo/fmover-slide-x.html" target="_blank">horizontal slideshow live demo</a>

<img src="../asset/qrcode/fmover-slide-x.png" width="200"/>

#### File Size

* Just 4.27KB after compression

#### Installation

```
npm install --save fmover-slide-x
```

`fmover-slide-x` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `fmoverSlideX` by serving as `<script>` tag.

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
        fmoverSlideX()
    ]
})
```

2 DOM layers:

* Wrapper layer: the `div`  with class name `slide-wrap`(custom class name) in above code
* Slideshow layer: each `div` element under wrapper layer. Note that each slide must be `div` element.

Slideshow layer's width is equal to the sum of the widths of all slides. What's the width of each slide? You need to know the difference between `fmover-slide-x` and other slideshow plugins or framework:

<p class="tip">
fmover-slide-x will automatically acquire the width of the parental element of the wrapper layer as the width of the slide.
</p>

You can create a layer of DOM elements outside the wrapper layer as a parent element, only need to control the width of the parent element to control the width of the slide.


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

* Default: `left`

* Description:

    Specifies the direction of slide, default value is `left`, if set it to `right` will be slide from left to right.

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
        fmoverSlideX({
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
        fmoverSlideX({
            onTouchStart: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
            onTouchMove: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
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
        fmoverSlideX({
            onTransStart: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
            onTransMove: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
            onTransEnd: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
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
        fmoverSlideX({
            onChange: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
            onChangeEnd: function (currentX) {
                console.log(currentX)
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
        fmoverSlideX({
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
        fmoverSlideX()
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
        fmoverSlideX()
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
        fmoverSlideX()
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
        fmoverSlideX()
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
        fmoverSlideX()
    ]
})

btn.addEventListener('click', function () {
    fm[0].slideTo(3, 3000)
}, false)
```

###### refresh([force])

* Description:

    When the number of slides changes, you need to manually call this method to make `fmover-slide-x` recalculate the number of slides and each slide size.

* Parameters:
    * `{Boolean} force` reset to initial state. If set it to `true`, will be shown specified slide by `startSlide` after refresh. There is no effect on the slide when refresh it if not passing this parameter or set it to `false`.

* Example:

```js
var fm = new Fmover({
    el: '.slide-wrap',
    plugins: [
        fmoverSlideX()
    ]
})

btn.addEventListener('click', function () {
    fm[0].refresh()
}, false)
```
