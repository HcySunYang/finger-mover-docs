## finger-mover

#### Overview

`finger-mover` is a motion library integrates
[Fingerd (a development kit for finger unit event management in mobile development)](/package/fingerd)
and
[Moved (a micro movement framework)](/package/moved)
in mobile development. `finger-mover` provides many useful plugins, such as [Vertical scroll simulation (simulation-scroll-y.js)](/plugins/simulation-scroll-y), [Horizontal scroll simulation (simulation-scroll-x.js)](/plugins/simulation-scroll-x) and so on.

#### Features

* Just 9.75KB after compression

* Plugin support, motion components are available as plug-ins and you can also develop your own plugin, reference [Plugin Development Guide](/creating-plugins)

#### Installation

```
npm install --save finger-mover
```

`finger-mover` released as a `umd` module. You can use it in any way for your favorite. You can get global variable `Fmover` by serving as `<script>` tag.

#### Getting Started

```js
// Import finger-mover
import Fmover from 'finger-mover'
// Import vertical scroll simulation plugin simulation-scroll-y
import simulationScrollY from 'simulation-scroll-y'
// Import horizontal scroll simulation plugin simulation-scroll-x
import simulationScrollX from 'simulation-scroll-x'

// While using both simulation-scroll-y and simulation-scroll-x plugins to implement 2D scrolling
let fm = new Fmover({
    el: '#scroll-box',
    plugins: [
        simulationScrollX(),
        simulationScrollY()
    ]
})

```

* [2D scrolling live demo]()

#### Access the Plugin Object

`Fmover` can access the plugin object by the order of the plugin import. Take the above code as an example:

```js
// fm[0] access the first plugin object `simulationScrollX`
// Call the instance method of the object
fm[0].scrollTo(-300, 1000)
```

#### Plugins List

###### [Vertical scroll simulation](/plugins/simulation-scroll-y)

Description: There are many scroll simulation lib on mobile device, such as: iscroll, xscroll, better-scroll and so on. Why do you should be know to `simulation-scroll-y`？[The feature of simulation-scroll-y you should to know ](/plugins/simulation-scroll-y?id=%E4%B8%8E%E7%8E%B0%E6%9C%89%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E7%9A%84%E5%AF%B9%E6%AF%94)。

###### [Horizontal scroll simulation](/plugins/simulation-scroll-x)

Description: Use `simulation-scroll-x` simulat horizontal scroll on mobile device.

###### [2D scrolling](/plugins/2d-scroll)

Description: You just need to use the vertical scrolling plugin `simulation-scroll-y` and horizontal scrolling plugin `simulation-scroll-x` at the same time to implement 2D scroll.

###### [Horizontal slide show](/plugins/fmover-slide-x)

Description: Lightweight and simple horizontal slide show solution on mobile device.

###### [Vertical slide show](/plugins/fmover-slide-y)

Description: Lightweight and simple vertical slide show solution on mobile device.

#### Custom Plugins

You can develop your own plugin for `finger-mover`, reference [Plugin Development Guide](/creating-plugins)

#### Package

###### [Fingerd](/package/fingerd)

> `Fingerd` is a development kit for finger unit event management in mobile development

###### [Moved](/package/moved)

> `Moved` is a micro movement framework.