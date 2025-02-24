# Animate Display Property
A JavaScript micro plugin to add transition effects for the CSS display property. This plugin uses CSS3 animations to acheive transition effects when an element's display property changes.

As you all know that CSS3 transitions doesn't work for the display property and we switch between `display: block/inline/flex` and `display: none` to show and hide elements. I found it difficult in my projects to show and hide elements with transition effects. So I wrote this micro plugin to hide and show elements with effects.

Ofcourse, there are many libraries out there to acheive this and there are some CSS tricks as well like changing element's height, changing opacity, etc. But I wanted a precise solution with minimal code.

You can view the demo at the official [webiste](https://bilalshareef.github.io/animate-display-property/) of the plugin.

## Installation

You can download the latest version of the plugin from [Github Releases](https://github.com/bilalshareef/animate-display-property/releases/latest) or you can install from npm using the following command.

```bash
npm install animate-display-property --save
```

## Quick Start

To use the plugin, you need to include the js and css files of the plugin in your html document.

```html
<link type="text/css" rel="stylesheet" href="path/to/adp.css">
<script type="text/javascript" src="path/to/adp.js"></script>
```

Typically, to hide an element, we will get the element object and update display property with a value `none`. Here, use the hide method of the plugin which takes two mandatory arguments(element and animation effect).

```js
var elemToHide = document.querySelector('.box');
ADP.hide(elemToHide, 'fade');
```

To show an element, use the show method of the plugin which is similar to the hide method.

```js
var elemToShow = document.querySelector('.box');
ADP.show(elemToShow, 'fade');
```

The functionality of the hide method is that it first adds the animation effect for the element and when the animation completes `adp-hide` class is added which has display property with value `none`.

The functionality of the show method is that it first removes the `adp-hide` class from the element and then adds the animation effect. This plugin makes use of the `animationend` event to fire the callbacks which we will see in the next section.

There is also a toggle method which can be used to toggle between show and hide. The usage is same as the show and hide methods.

```js
var elemToToggle = document.querySelector('.box');
ADP.toggle(elemToToggle, 'fade');
```

If an element is currently being animated to hide and show method is invoked for the element, then hide animation for the element will be cancelled and show animation will begin. This is same in vice versa as well.

Similarly, if an element is already visible and show method is called for the element, then animation will not be performed. This is same for the vice versa case as well.

## Callbacks

The show, hide and toggle methods of the plugin also take an optional third parameter which is a callback function. This callback function will be fired on completion of show/hide.


```js
var elemToShow = document.querySelector('.box');
ADP.show(elemToShow, 'fade', function (state) {
    console.log('Callback fired with ' + state);
});
```

The callback function takes an argument called state. The state represents what happened to the element when show/hide was called. The different states are as follows.

* **animation-complete** - The intended action(show/hide) has completed with proper animation.

* **already-visible** - This state will be returned when the element is already visible and show method is called for the element.

* **show-already-in-progress** - This state will be returned when show animation is already in progress and show method is invoked for the element.

* **already-hidden** - This state will be returned when the element is already hidden and hide method is called for the element.

* **hide-already-in-progress** - This state will be returned when hide animation is already in progress and hide method is invoked for the element.

* **animation-cancelled** - This state will be returned when an animation is cancelled. Cancellation of animation happens when an animation is in progress for showing/hiding the element and the opposite action is performed on the element. **Example** - Hide method is called for an element and when the hiding animation is in progress, show method is called for the same element. In this case, hide animation will be cancelled midway and callback of hide method will be invoked with animation-cancelled state. Once hide aimation is cancelled, show animation will be performed on the element.

## Effects

You can apply any kind of animation effect using this plugin and `adp.css` file contains some basic animation effects.

You can also create your own animation effects and use them with the plugin. The following is the CSS for a sample fade animation effect.

```css
@keyframes fade-adp-show {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}

@keyframes fade-adp-hide {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0;
    }
}

.fade-adp-show, .fade-adp-hide {
    animation-duration: 0.5s;
    animation-fill-mode: both;
    animation-timing-function: cubic-bezier(0.7,0,0.3,1);
}

.fade-adp-show {
    animation-name: fade-adp-show;
}

.fade-adp-hide {
    animation-name: fade-adp-hide;
}
```

In the above code, the name of the animation is `fade` and you need to add two keyframe rules(one for the hiding effect and other for showing effect). The syntax for the animation names are `[animation-name]-adp-show` and `[animation-name]-adp-hide`.

Then, you need to write animation styles for two classes(`[animation-name]-adp-show` and `[animation-name]-adp-hide`) which will be used by the `adp.js` file to perform the animations.

If you are not using any of the animation effects provided in the `adp.css` file, then feel free to ignore the file completely. In that case, you have to add the following CSS code somewhere in your styles.

```css
.adp-hide {
    display: none !important;
}
```

You can also use two different animation effects for showing and hiding the element.

```js
var elemToHide = document.querySelector('.box');
ADP.hide(elemToHide, 'fade'); // fade effect is used when hiding the element.
```

```js
var elemToShow = document.querySelector('.box');
ADP.show(elemToShow, 'slide-left'); // slide-left effect is used when showing up the element.
```

## License

[GPL v3](LICENSE.md)
