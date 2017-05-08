## Overview

`Fingerd` is a development kit for finger unit event management in mobile development without provides event listening function. `Fingerd` provides a collection of fingers event by injecting the native event object into it in your own event listening functions.

## Installation

```
npm install --save fingerd
```

`Fingerd` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `Fingerd` by serving as `<script>` tag.

## Getting Started

```js
let el = document.querySelector('#box')

// Create a Fingerd object by given element as the scope for the role of the finger, this element is same as listening event element in general.
f = new Fingerd({
    element: el
})

// After injecting the native event into `Fingerd`, you'll get a collection of fingers event by print object `f`.
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

We instantiated `Fingerd` object in the above code, and call the function `injectEvent(event)` in `touchstart`, `touchmove`, `touchend` and  `touchcancel`, injecting the native event into them. Attention:

<p class="warning">
You must call the function `injectEvent()` at least in these 3 event functions: `touchstart`, `touchmove` and `touchend`, that doesn't cause code redundancy because we need use at least these 3 events in almost all projects.
</p>

## Concept

There are some very simple concepts that will help you understand `Fingerd`.

#### Scope

<p class="tip">
We need to pass an element when the object `Fingerd` is instantiated. `Scope` is the area of that element. `Fingers Leave` is the finger draws the range or lifts.
</p>

#### Finger Order

<p class="tip">
To determine the fingers order by the touch order in scope. Use the first touch in scope as the first finger, and so on.
</p>

#### Finger State

<p class="tip">
Each finger will maintain a data state only for the finger, such as moving distance, instantaneous speed, direction of movement, etc., and data between different fingers will not affect each other.

</p>

#### Finger Inheritance

<p class="tip">
When a finger leaves the scope (the finger draws the range or lifts), by *Finger Order*, if there is a finger on the back of the finger, then the back of the finger should inherit the *Finger State* that lifts *Scope*, if there is no finger behind the finger, clear the *Finger State* of the finger.
</p>

## Options, Properties, Methods, Data Status

#### Options

###### element

* Type: `Element`

* Required: true

* Description:

    The range of elements will serve as scope.

###### transTime

* Type: `Number`

* Required: false

* Default: `3000`

* Description:

    Calculating the acceleration of the finger slip (`xa`、`ya`) via formula as follows:

    ```js
    Vt ＝ Vo + at
    a = (Vt - Vo) / t
    ```

    Where `t` represents the `transTime` parameter and this parameter will affect the Finger State value of `xa` and `ya`, that's will be introduced next.

    Calculate the inertia rolling distance (`transDistanceX`、`transDistanceY`) via formula as follows:

    ```
    X = Vot + at^2 / 2
    ```

    Where `a` obtained by calculating `transTime`, that will indirectly affect the value of `transDistanceX` and `transDistanceY`.

    Conclusion: The larger the value of `transTime`, the smaller the acceleration, the longer the inertia scroll time.

#### Instance Properties

###### element

* Type: `Element`

* Description:

    Return a DOM element, it's the scope element that is passed when creating the Fingerd object.

###### fingers

* Type: `Array`

* Description:

    Return an array, it's storage all the fingers in the scope, the 0th element of the array represents the first finger, and so on.

###### changeFingerIndex

* Type: `Number`

* Description:

    Return a number index points to `fingers` array, it's represented in which finger has changed, this has different meanings in different events. For example: it's represented of which finger triggered the `touchstart` event in the `touchstart`, and it's represented of which finger triggered the `touchmove` event in the `touchmove`.

###### fingerNumber

* Type: `Number`

* Description:

    Number of fingers in the scope

    As we know `fingers` storage all the fingers in the scope. What different between `fingers.length` and `fingerNumber`? The difference is that when the last finger leaves the scope, the value of `fingerNumber` is 0 and the length of `fingers` is 1, it storage the data state that moment of last finger lifts.

    Another difference between `fingerNumber` and native event `touches.length`: `fingerNumber` is the number of fingers in the real scope.

    * [Different between fingerNumber and touches.length live demo]()

#### Instance Method

###### injectEvent(event)

* Parameter:

    * `{Object} event`: Native event object

* Description:

    Injection of the native event object, Fingerd will use it for data operations

* Return value: `{Fingerd} Fingerd instance object itself`

###### getFingerPicth(f1, f2)

* Parameter:

    * `{fingers} f1`: Any of the finger in the array `fingers`
    * `{fingers} f2`: Any of the finger in the array `fingers`

* Description:

    Get the distance between any two fingers

* Return value: `{Number} the distance between two fingers`

###### getScale(f1, f2)

* Parameter:

    * `{fingers} f1`: Any of the finger in the array `fingers`
    * `{fingers} f2`: Any of the finger in the array `fingers`

* Description:

    Get the scaling ratio according to two fingers

* Return value: `{Number} scaling ratio`

* Example:

```js
box.addEventListener('touchstart', function (event) {
    fg.injectEvent(event)
    event.preventDefault()
}, false)

box.addEventListener('touchmove', function (event) {
    fg.injectEvent(event)
    // Get the scaling ratio according to two fingers when the number of fingers on the active element is greater than 1
    if (fg.fingerNumber > 1) {
        var dis = fg.getScale(fg.fingers[0], fg.fingers[1])
        console.log('Scaling Ratio is: ' + dis)
    }
    event.preventDefault()
}, false)

box.addEventListener('touchend', function (event) {
    fg.injectEvent(event)
    event.preventDefault()
}, false)
```

* [live demo ---- Scaling]()

#### The data status of each finger (Tevent)

In addition to the generic attributes associated with the finger state in the instance attribute, more data status associated with each finger is stored in a Tevent object. We can access in `touchmove` like this:

```js
el.addEventListener('touchmove', function (event) {
    f.injectEvent(event)

    // Gets an array containing all the fingers
    let fingers = f.fingers
    // Get the data state of the first finger
    let f1Data = fingers[0]

    // Print data
    console.log(f1Data)
})
```

The `fingerData` will contain the following information:

###### native

* Type: `Object`

* Description:

    Native `event` object

###### startX

* Type: `Number`

* Description:

    The X coordinate when the finger touches the element, it equivalent to native `event` object's `event.pageX`.

###### startY

* Type: `Number`

* Description:

     The Y coordinate when the finger touches the element, it equivalent to native `event` object's `event.pageY`.

###### startTime

* Type: `Number`

* Description:

    The timestamp when the finger touches the element.

###### lastX

* Type: `Number`

* Description:

    The initial value is equivalent to startX, it will storage the X-axis of the finger at the last touchmove event triggers when the finger moves.

###### lastY

* Type: `Number`

* Description:

    he initial value is equivalent to startY, it will storage the Y-axis of the finger at the last touchmove event triggers when the finger moves.

###### lastTime

* Type: `Number`

* Description:

    The initial value is equivalent to startTime, it will storage the event timestamp at the last touchmove event triggers when the finger moves.

###### xs

* Type: `Number`

* Description:

    The instantaneous displacement of the finger in the lateral direction.

###### ys

* Type: `Number`

* Description:

    The instantaneous displacement of the finger in the longitudinal direction.

###### xt

* Type: `Number`

* Description:

    The instantaneous time of the finger in the lateral direction.

###### yt

* Type: `Number`

* Description:

    The instantaneous time of the finger in the longitudinal direction.

###### xv

* Type: `Number`

* Description:

    The instantaneous speed of the finger in the lateral direction.

###### yv

* Type: `Number`

* Description:

    The instantaneous speed of the finger in the longitudinal direction.

###### xa

* Type: `Number`

* Description:

    The acceleration of the finger in the lateral direction.

###### ya

* Type: `Number`

* Description:

    The acceleration of the finger in the longitudinal direction.

###### distanceX

* Type: `Number`

* Description:

    Horizontal sliding distance of the finger.

###### distanceY

* Type: `Number`

* Description:

    Vertical sliding distance of the finger.

###### transDistanceX

* Type: `Number`

* Description:

    The property preserves the system's recommended distance that the element should slide horizontally in inertia when the finger slides out of the screen.

###### transDistanceY

* Type: `Number`

* Description:

    The property preserves the system's recommended distance that the element should slide longitudinal in inertia when the finger slides out of the screen.

###### transTime

* Type: `Number`

* Description:

    The time at which the element should slide in inertia.

###### fingerInElement

* Type: `Boolean`

* Description:

    Whether the finger is in the scope, true or false.

###### direction

* Type: `Object`

* Description:

    The finger slides in the direction of 8 directions, and the boolean value indicates whether or not to slide in that direction.

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







