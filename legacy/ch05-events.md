---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Events
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter05
---

# {{ page.title }} <br> {{ page.sub-title }}

## Overview

jQuery provides simple methods for attaching event handlers to selections. When an event occurs, the provided function is executed. Inside the function, this refers to the element that was clicked.

For details on jQuery events, visit [http://api.jquery.com/category/events/](http://api.jquery.com/category/events/).

The event handling function can receive an event object. This object can be used to determine the nature of the event, and to prevent the event’s default behavior.

For details on the event object, visit [http://api.jquery.com/category/events/event-object/](http://api.jquery.com/category/events/event-object/).

## Connecting Events to Elements

jQuery offers convenience methods for most common events, and these are the methods you will see used most often. These methods — including `$.fn.click`, `$.fn.focus`, `$.fn.blur`, `$.fn.change`, etc. — are shorthand for jQuery's `$.fn.bind` method. The bind method is useful for binding the same handler function to multiple events, when you want to provide data to the event hander, when you are working with custom events, or when you want to pass an object of multiple events and handlers.

###### Example 5.1: Event binding using a convenience method

```js
$('p').click(function() {
    console.log('click');
});
```

###### Example 5.2: Event biding using the $.fn.bind method

```js
$('p').bind('click', function() {
    console.log('click');
});
```

###### Example 5.3: Event binding using the $.fn.bind method with data

```js
$('input').bind(
    'click change',  // bind to multiple events
    { foo : 'bar' }, // pass in data
 
    function(eventObject) {
        console.log(eventObject.type, eventObject.data);
        // logs event type, then { foo : 'bar' }
    }
);
```

### Connecting Events to Run Only Once

Sometimes you need a particular handler to run only once — after that, you may want no handler to run, or you may want a different handler to run. jQuery provides the `$.fn.one` method for this purpose.

###### Example 5.4: Switching handlers using the $.fn.one method

```js
$('p').one('click', function() {
    console.log('You just clicked this for the first time!');
    $(this).click(function() { console.log('You have clicked this before!'); });
});
```

The `$.fn.one` method is especially useful if you need to do some complicated setup the first time an element is clicked, but not subsequent times.

### Disconnecting Events

To disconnect an event handler, you use the `$.fn.unbind` method and pass in the event type to unbind. If you attached a named function to the event, then you can isolate the unbinding to that named function by passing it as the second argument.

###### Example 5.5: Unbinding all click handlers on a selection

```js
$('p').unbind('click');
```

###### Example 5.6: Unbinding a particular click handler

```js
var foo = function() { console.log('foo'); };
var bar = function() { console.log('bar'); };
 
$('p').bind('click', foo).bind('click', bar);
$('p').unbind('click', bar); // foo is still bound to the click event
```

### Namespacing Events

For complex applications and for plugins you share with others, it can be useful to namespace your events so you don't unintentionally disconnect events that you didn't or couldn't know about.

###### Example 5.7: Namespacing events

```js
$('p').bind('click.myNamespace', function() { /* ... */ });
$('p').unbind('click.myNamespace');
$('p').unbind('.myNamespace'); // unbind all events in the namespace
```

### Binding Multiple Events

Quite often elements in your application will be bound to multiple events, each having a different function for handing the event. In these cases you can pass an object into $.fn.bind with one or more key/value pairs, with the key being the event name and the value being the function to handle the event.

###### Example 5.8: Binding Multiple Events

```js
$('p').bind({
  'click': function() { console.log('clicked!'); },
  'mouseover': function() { console.log('hovered!'); }
});
```

> #### Note  
>   
> The option to pass an object of multiple events and handlers to `$.fn.bind` was introduced in jQuery 1.4.4.

## Inside the Event Handling Function

As mentioned in the overview, the event handling function receives an event object, which contains many properties and methods. The event object is most commonly used to prevent the default action of the event via the preventDefault method. However, the event object contains a number of other useful properties and methods, including:

pageX, pageY
: The mouse position at the time the event occurred, relative to the top left of the page.

type
: The type of the event (e.g. "click").

which
: The button or key that was pressed.

data
: Any data that was passed in when the event was bound.

target
: The DOM element that initiated the event.

preventDefault()
: Prevent the default action of the event (e.g. following a link).

stopPropagation()
: Stop the event from bubbling up to other elements.

In addition to the event object, the event handling function also has access to the DOM element that the handler was bound to via the keyword `this`. To turn the DOM element into a jQuery object that we can use jQuery methods on, we simply do `$(this)`, often following this idiom:

```js
var $this = $(this);
```

##### Preventing a link from being followed

```js
$('a').click(function(e) {
    var $this = $(this);
    if ($this.attr('href').match('evil')) {
        e.preventDefault();
        $this.addClass('evil');
    }
});
```

## Triggering Event Handlers

jQuery provides a way to trigger the event handlers bound to an element without any user interaction via the `$.fn.trigger` method. While this method has its uses, it should not be used simply to call a function that was bound as a click handler. Instead, you should store the function you want to call in a variable, and pass the variable name when you do your binding. Then, you can call the function itself whenever you want, without the need for `$.fn.trigger`.

##### Triggering an event handler the right way

```js
var foo = function(e) {
    if (e) {
        console.log(e);
    } else {
        console.log('this didn\'t come from an event!');
    }
};
 
 
$('p').click(foo);
 
foo(); // instead of $('p').trigger('click')
```

Increasing Performance with Event Delegation
You'll frequently use jQuery to add new elements to the page, and when you do, you may need to bind events to those new elements — events you already bound to similar elements that were on the page originally. Instead of repeating your event binding every time you add elements to the page, you can use event delegation. With event delegation, you bind your event to a container element, and then when the event occurs, you look to see which contained element it occurred on. If this sounds complicated, luckily jQuery makes it easy with its $.fn.live and $.fn.delegate methods.

While most people discover event delegation while dealing with elements added to the page later, it has some performance benefits even if you never add more elements to the page. The time required to bind event handlers to hundreds of individual elements is non-trivial; if you have a large set of elements, you should consider delegating related events to a container element.

Note
The $.fn.live method was introduced in jQuery 1.3, and at that time only certain event types were supported. As of jQuery 1.4.2, the $.fn.delegate method is available, and is the preferred method.

Event delegation using $.fn.delegate

1
$('#myUnorderedList').delegate('li', 'click', function(e) {
2
    var $myListItem = $(this);
3
    // ...
4
});
Event delegation using $.fn.live

1
$('#myUnorderedList li').live('click', function(e) {
2
    var $myListItem = $(this);
3
    // ...
4
});
Unbinding Delegated Events
If you need to remove delegated events, you can't simply unbind them. Instead, use $.fn.undelegate for events connected with $.fn.delegate, and $.fn.die for events connected with $.fn.live. As with bind, you can optionally pass in the name of the bound function.

Unbinding delegated events

1
$('#myUnorderedList').undelegate('li', 'click');
2
$('#myUnorderedList li').die('click');
Event Helpers
jQuery offers two event-related helper functions that save you a few keystrokes.

$.fn.hover
The $.fn.hover method lets you pass one or two functions to be run when the mouseenter and mouseleave events occur on an element. If you pass one function, it will be run for both events; if you pass two functions, the first will run for mouseenter, and the second will run for mouseleave.

Note
Prior to jQuery 1.4, the $.fn.hover method required two functions.

The hover helper function

1
$('#menu li').hover(function() {
2
    $(this).toggleClass('hover');
3
});
$.fn.toggle
The $.fn.toggle method is triggered by the "click" event and accepts two or more functions. Each time the click event occurs, the next function in the list is called. Generally, $.fn.toggle is used with just two functions; however, it will accept an unlimited number of functions. Be careful, though: providing a long list of functions can be difficult to debug).

The toggle helper function

1
$('p.expander').toggle(
2
    function() {
3
        $(this).prev().addClass('open');
4
    },
5
    function() {
6
        $(this).prev().removeClass('open');
7
    }
8
);
Exercises
Create an Input Hint
Open the file /exercises/index.html in your browser. Use the file /exercises/js/inputHint.js or work in Firebug. Your task is to use the text of the label for the search input to create "hint" text for the search input. The steps are as follows:

Set the value of the search input to the text of the label element

Add a class of "hint" to the search input

Remove the label element

Bind a focus event to the search input that removes the hint text and the "hint" class

Bind a blur event to the search input that restores the hint text and "hint" class if no search text was entered

What other considerations might there be if you were creating this functionality for a real site?

Add Tabbed Navigation
Open the file /exercises/index.html in your browser. Use the file /exercises/js/tabs.js. Your task is to create tabbed navigation for the two div.module elements. To accomplish this:

Hide all of the modules.

Create an unordered list element before the first module.

Iterate over the modules using $.fn.each. For each module, use the text of the h2 element as the text for a list item that you add to the unordered list element.
Bind a click event to the list item that:

Shows the related module, and hides any other modules

Adds a class of "current" to the clicked list item

Removes the class "current" from the other list item

Finally, show the first tab.
