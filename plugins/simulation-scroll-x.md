## simulation-scroll-x.js

#### Overview

Horizontal scroll simulation solution on mobile device.

#### Installation

```
npm install --save simulation-scroll-x
```

`simulation-scroll-x` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `simulationScrollX` by serving as `<script>` tag.

#### Getting Started

```html
<div id="box">
    <div id="scroll-box">

    </div>
</div>

<!-- Import finger-mover -->
<script src="finger-mover.js"></script>
<!-- Import simulation-scroll-x -->
<script src="simulation-scroll-x.js"></script>
<script>
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX()
        ]
    })
</script>
```

The above code will implement a simple horizontal simulation scrolling.

* <a href="https://fmover.hcysun.me/example/demo/simulation-x-demo.html" target="_blank">Substantially horizontal scrolling simulation live demo</a>

<img src="../asset/qrcode/simulation-x-demo.png" width="200"/>

#### Features

* Just 3.11KB after compression

* Scrolling element movement is not affected when multi-finger touch, the scrolling convergence very smooth powered by [Fingerd](/package/fingerd)

* Scrolling element movement is not affected when finger swipe out the screen edge. Scrolling element movement will be stuck when finger swipe out the screen edge on many browsers in a mostly open-source solution.

* 2D scrolling implemention with [simulation-scroll-y.js](/plugins/simulation-scroll-y) plugin

* Scroll bar

#### Options

###### scrollBar

* Type: `Boolean`

* Default: `true`

* Description:

    Specifies the display scroll bar.

###### bounce

* Type: `Boolean`

* Default: `true`

* Description:

    Specifies the boundary elasticity.

#### Options/Event

<p class="tip">
    Note that the scope of the callback function for the following event option is the plug-in instance object.
</p>

###### onTouchMove

* Type: `Function`

* Description:

    Continuously trigger when finger drag the scroll element to scroll and pass the current position coordinates to the callback function.

* Example:

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTouchMove: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

###### onTransMove

* Type: `Function`

* Description:

    Continuously trigger whe inertial scrolling and pass the current position coordinates to the callback function.

* Example:

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTransMove: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

###### onTransMoveEnd

* Type: `Function`

* Description:

    Trigger when inertial scrolling stopped and pass the current position coordinates to the callback function.

* Example:

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX({
                onTransMoveEnd: function (currentX) {
                    console.log(currentX)
                }
            })
        ]
    })
    ```

#### Instance Method

###### scrollTo(target, time)

* Description:

    Scroll to target point.

* Parameters:

    * `{Number | Element | String} target` Scroll to the target point, target element or target element selector.
    * `{Number} time` The time of scrolling: milliseconds

* Example:

    ```js
    var fm = new Fmover({
        el: '#scroll-box',
        plugins: [
            simulationScrollX()
        ]
    })

    // Scroll to the position -300px in 1 second.
    fm[0].scrollTo(-300, 1000)
    ```

    Scroll to the target point immediately by set time to 0.

    ```js
    fm[0].scrollTo(-300, 0)
    ```

    The method scrollTo support that scrolling by given DOM element in addition to setting the digital target points directly. You can use the DOM element or the CSS selector of the element as the first parameter.

    ```js
    let p = document.querySelector('p')
    fm[0].scrollTo(p, 1000)
    // 或
    fm[0].scrpllTo('.wrap', 1000)
    ```

    * <a href="https://fmover.hcysun.me/example/demo/scroll-to-x.html" target="_blank">Scroll to the target point live demo</a>

    <img src="../asset/qrcode/scroll-to-x.png" width="200"/>

###### refreshSize()

* Description:

    Recalculate the size of scrolling elements, scroll restrictions and so on.

