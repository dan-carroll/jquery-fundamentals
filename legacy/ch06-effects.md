---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Effects
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter06
---

# {{ page.title }} <br> {{ page.sub-title }}

## Overview

jQuery makes it trivial to add simple effects to your page. Effects can use the built-in settings, or provide a customized duration. You can also create custom animations of arbitrary CSS properties.

For complete details on jQuery effects, visit [http://api.jquery.com/category/effects/](http://api.jquery.com/category/effects/).

## Built-in Effects

Frequently used effects are built into jQuery as methods:

$.fn.show
: Show the selected element.

$.fn.hide
: Hide the selected elements.

$.fn.fadeIn
: Animate the opacity of the selected elements to 100%.

$.fn.fadeOut
: Animate the opacity of the selected elements to 0%.

$.fn.slideDown
: Display the selected elements with a vertical sliding motion.

$.fn.slideUp
: Hide the selected elements with a vertical sliding motion.

$.fn.slideToggle
: Show or hide the selected elements with a vertical sliding motion, depending on whether the elements are currently visible.

###### Example 6.1: A basic use of a built-in effect

```js
$('h1').show();
```

### Changing the Duration of Built-in Effects

With the exception of `$.fn.show` and `$.fn.hide`, all of the built-in methods are animated over the course of 400ms by default. Changing the duration of an effect is simple.

###### Example 6.2: Setting the duration of an effect

```js
$('h1').fadeIn(300);      // fade in over 300ms
$('h1').fadeOut('slow');  // using a built-in speed definition
```

#### jQuery.fx.speeds

jQuery has an object at `jQuery.fx.speeds` that contains the default speed, as well as settings for `"slow"` and `"fast"`.

```js
speeds: {
    slow: 600,
    fast: 200,
    // Default speed
    _default: 400
}
```

It is possible to override or add to this object. For example, you may want to change the default duration of effects, or you may want to create your own effects speed.

###### Example 6.3: Augmenting jQuery.fx.speeds with custom speed definitions

```js
jQuery.fx.speeds.blazing = 100;
jQuery.fx.speeds.turtle = 2000;
```

### Doing Something when an Effect is Done

Often, you'll want to run some code once an animation is done — if you run it before the animation is done, it may affect the quality of the animation, or it may remove elements that are part of the animation. [Definition: *Callback functions* provide a way to register your interest in an event that will happen in the future.] In this case, the event we'll be responding to is the conclusion of the animation. Inside of the callback function, the keyword `this` refers to the element that the effect was called on; as we did inside of event handler functions, we can turn it into a jQuery object via `$(this)`.

###### Example 6.4: Running code when an animation is complete

```js
$('div.old').fadeOut(300, function() { $(this).remove(); });
```

Note that if your selection doesn't return any elements, your callback will never run! You can solve this problem by testing whether your selection returned any elements; if not, you can just run the callback immediately.

###### Example 6.5: Run a callback even if there were no elements to animate

```js
var $thing = $('#nonexistent');
 
var cb = function() {
    console.log('done!');
};
 
if ($thing.length) {
    $thing.fadeIn(300, cb);
} else {
    cb();
}
```

## Custom Effects with `$.fn.animate`

jQuery makes it possible to animate arbitrary CSS properties via the `$.fn.animate` method. The `$.fn.animate` method lets you animate to a set value, or to a value relative to the current value.

###### Example 6.6: Custom effects with $.fn.animate

```js
$('div.funtimes').animate(
    {
        left : "+=50",
        opacity : 0.25
    },
    300, // duration
    function() { console.log('done!'); // calback
});
```

> #### Note  
>   
> Color-related properties cannot be animated with `$.fn.animate` using jQuery out of the box. Color animations can easily be accomplished by including the [color plugin](http://plugins.jquery.com/files/jquery.color.js.txt). We'll discuss using plugins later in the book.

### Easing

[Definition: *Easing* describes the manner in which an effect occurs — whether the rate of change is steady, or varies over the duration of the animation.] jQuery includes only two methods of easing: swing and linear. If you want more natural transitions in your animations, various easing plugins are available.

As of jQuery 1.4, it is possible to do per-property easing when using the $.fn.animate method.

###### Example 6.7: Per-property easing

```js
$('div.funtimes').animate(
    {
        left : [ "+=50", "swing" ],
        opacity : [ 0.25, "linear" ]
    },
    300
);
```

For more details on easing options, see [http://api.jquery.com/animate/](http://api.jquery.com/animate/).

## Managing Effects

jQuery provides several tools for managing animations.

$.fn.stop
: Stop currently running animations on the selected elements.

$.fn.delay
: Wait the specified number of milliseconds before running the next animation.
: ```js
$('h1').show(300).delay(1000).hide(300);
```

jQuery.fx.off
: If this value is true, there will be no transition for animations; elements will immediately be set to the target final state instead. This can be especially useful when dealing with older browsers; you also may want to provide the option to your users.

## Exercises

### Reveal Hidden Text

Open the file /exercises/index.html in your browser. Use the file /exercises/js/blog.js. Your task is to add some interactivity to the blog section of the page. The spec for the feature is as follows:

Clicking on a headline in the #blog div should slide down the excerpt paragraph

Clicking on another headline should slide down its excerpt paragraph, and slide up any other currently showing excerpt paragraphs.

Hint: don't forget about the :visible selector!

Create Dropdown Menus
Open the file /exercises/index.html in your browser. Use the file /exercises/js/navigation.js. Your task is to add dropdowns to the main navigation at the top of the page.

Hovering over an item in the main menu should show that item's submenu items, if any.

Exiting an item should hide any submenu items.

To accomplish this, use the $.fn.hover method to add and remove a class from the submenu items to control whether they're visible or hidden. (The file at /exercises/css/styles.css includes the "hover" class for this purpose.)

Create a Slideshow
Open the file /exercises/index.html in your browser. Use the file /exercises/js/slideshow.js. Your task is to take a plain semantic HTML page and enhance it with JavaScript by adding a slideshow.

Move the #slideshow element to the top of the body.

Write code to cycle through the list items inside the element; fade one in, display it for a few seconds, then fade it out and fade in the next one.

When you get to the end of the list, start again at the beginning.

For an extra challenge, create a navigation area under the slideshow that shows how many images there are and which image you're currently viewing. (Hint: $.fn.prevAll will come in handy for this.)
