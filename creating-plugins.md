## Plugin Development Guide

You should read [Fingerd (a development kit for finger unit event management in mobile development)](/package/fingerd) and [Moved (a micro movement framework)](/package/moved) before create your first plugin.

#### Plugin Structure

`finger-mover` support plugins, there is an example about how to use a plugin in `finger-mover`:

```js
new Fmover({
    el: box,
    pulgins: [
        somePlugin({
            a: 1,
            b: 2
            ...
        })
    ]
})
```

Plugin `somPlugin` comply with the following rules:

1. Having a exported function we call that `main function` and it can be calling and pass the parameters for it;

2. The `main function` must be have a return value we call `inner layer function` and the construct function of `Fmover` was parameter of it;

3、`Inner layer function` should return an object that contains one or more of the following methods.

###### init(fmover)

* Description:

    Calling on initialized the plugin, you can doing somthing at here and the instance of `fmover` will be passed as an argument.

###### start(fingerd)

* Description:

    The function called synchronously when `touchstart` event is triggered and the instance of `fmover` will be passed as an argument.

###### move(fingerd)

* Description:

    The function called synchronously when `touchmove` event is triggered and the instance of `fmover` will be passed as an argument.

    Note that `return false` can prevent event bubbling in the call back function `move`.

###### end(fingerd)

* Description:

    The function called synchronously when `touchend` event is triggered and the instance of `fmover` will be passed as an argument.

###### cancel(fingerd)

* Description:

    The function called synchronously when `touchcancel` event is triggered and the instance of `fmover` will be passed as an argument.

<p class="tip">
In addition to the above methods, you can also define more methods on the returned object, but these methods will not be handled by `finger-mover`, but you can still use it inside the component.
</p>

#### Fmover Tool Functions

The construct function of `Fmover` was parameter of `inner layer function`. There are many useful methods in the construct function of `Fmover`, such as:

```js
// ES6 Structure
let {
    Moved,
    Fingerd,
    cssText,
    getPropFromMatrix,
    getRelativeRect,
    isInElement,
    extend,
    now,
    getStyle,
    throwError
} = Fmover
```

The following is a detailed description of each tool function:

###### Moved

* Description:

    `Moved` construct function. Reference [Moved](/package/moved)

###### Fingerd

* Description:

    `Fingerd` construct function. Reference [Fingerd](/package/fingerd)

###### cssText(el, cssText)

* Description:

    Set `cssText` for element, unlike `el.style.cssText` it's same as:

    ```js
    el.style.cssText = el.style.cssText + ' ' + cssText
    ```

* Parameters:

    * `{Element} el` element
    * `{String} cssText` CSS style string

* Description:

    ```js
    cssText(el, 'background: #000; color: red; transform: rotateX(30deg);')
    ```

###### getPropFromMatrix(el)

* Description:

    Get the `transform` transform attribute value of the element, the method is factual and  calculated from` Matrix`.

* Parameters:

    * `{Element} el` element

* Return Value:

    * `{Object}`
        * `{Number} translateX` The `translateX` value of current element
        * `{Number} translateY` The `translateY` value of current element

###### getRelativeRect(parent, child)

* Description:

    Get the left and top distance from the child element to the specified parent element

* Parameters:

    * `{Element} parent` Parent element
    * `{Element} child` Child element

* Return Value:

    * `{Object}`
        * `{Number} top` The distance of the child element from the top of the parent element
        * `{Number} left` The distance of the child element from the left of the parent element

###### isInElement(parent, child)

* Description:

    Determine whether the element `parent` contains the element` child`

* Parameters:

    * `{Element} parent` element
    * `{Element} child` element

* Return Value:

    * `{Boolean}` The `true` on behalf `parent` contains element `child`, otherwise `false`.

###### extend(to, from1, from2, from3...)

* Description:

    Object attribute inheritance, the same name attribute override.
    Inheritance properities of object `from1, from2, from3 ...` to the object `to`

* Parameters:

    * `{Object} to` Target object
    * `{Object} from*` Source object

* Return Value:

    * `{Object}` Inherited object

###### now()

* Description:

    Get the current timestamp, same as `Date.now()`

* Return Value:

    * `{Number}` Timestamp

###### getStyle(el, prototype)

* Description:

    Get the CSS properity of the element, packaging for `getComputedStyle`

* Parameters:

    * `{Element} el` element
    * `{String} prototype*` Attribute name

* Return Value:

    * `{String}` Attribute value

###### throwError(text)

* Description:

    Packaging for `throw new TypeError(text)`

* Parameters:

    * `{String} text*` Exception notice

#### Component Example

The following codes show the structure of the `simulation-scroll-y` plugin:

```js
// The main function, options is the parameter passed when the user calls the plugin
export default function (options) {
    // The construct function of `Fmover` was parameter of inner layer function
    return function (Fmover) {
        // The tool function to be used is deconstructed from the Fmover construct function
        const {
            Moved,
            transform,
            getStyle,
            extend,
            cssText,
            getRelativeRect,
            isInElement,
            throwError
        } = Fmover

        // Inner layer function return a object
        return {
            // The method init, start, move, end and cancel are build-in approach of finger-mover
            init (fmover) {

            },
            start (fingerd) {

            },
            move (fingerd) {

            },
            end (fingerd) {

            },
            cancel (fingerd) {

            },
            // The method scrollTo, loadEnd, refresh and refreshSize are non-built-in approach, you can export plug-in interfaces at here
            scrollTo () {

            },
            loadEnd () {

            },
            refresh () {

            },
            refreshSize () {

            }

        }

    }

}
```
