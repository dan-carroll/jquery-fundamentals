---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Performance Best Practices
chapter: 9
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter09
---

# {{ page.title }} <br> Chapter {{ page.chapter }}: {{ page.sub-title }}

This chapter covers a number of jQuery and JavaScript best practices, in no particular order. Many of the best practices in this chapter are based on the [jQuery Anti-Patterns for Performance](http://paulirish.com/perf) presentation by Paul Irish.

## Cache length during loops

In a for loop, don't access the length property of an array every time; cache it beforehand.

```js
var myLength = myArray.length;
 
for (var i = 0; i < myLength; i++) {
    // do stuff
}
```

## Append new content outside of a loop

Touching the DOM comes at a cost; if you're adding a lot of elements to the DOM, do it all at once, not one at a time.

```js
// this is bad
$.each(myArray, function(i, item) {
   var newListItem = '<li>' + item + '</li>';
   $('#ballers').append(newListItem);
});
 
// better: do this
var frag = document.createDocumentFragment();
 
$.each(myArray, function(i, item) {
    var newListItem = '<li>' + item + '</li>';
    frag.appendChild(newListItem);
});
$('#ballers')[0].appendChild(frag);
 
// or do this
var myHtml = '';
 
$.each(myArray, function(i, item) {
    html += '<li>' + item + '</li>';
});
$('#ballers').html(myHtml);
```

## Keep things DRY

Don't repeat yourself; if you're repeating yourself, you're doing it wrong.

```js
// BAD
if ($eventfade.data('currently') != 'showing') {
    $eventfade.stop();
}
 
if ($eventhover.data('currently') != 'showing') {
    $eventhover.stop();
}
 
if ($spans.data('currently') != 'showing') {
    $spans.stop();
}
 
// GOOD!!
var $elems = [$eventfade, $eventhover, $spans];
$.each($elems, function(i,elem) {
    if (elem.data('currently') != 'showing') {
        elem.stop();
    }
});
```

## Beware anonymous functions

Anonymous functions bound everywhere are a pain. They're difficult to debug, maintain, test, or reuse. Instead, use an object literal to organize and name your handlers and callbacks.

```js
// BAD
$(document).ready(function() {
    $('#magic').click(function(e) {
        $('#yayeffects').slideUp(function() {
            // ...
        });
    });
 
    $('#happiness').load(url + ' #unicorns', function() {
        // ...
    });
});
 
// BETTER
var PI = {
    onReady : function() {
        $('#magic').click(PI.candyMtn);
        $('#happiness').load(PI.url + ' #unicorns', PI.unicornCb);
    },
 
    candyMtn : function(e) {
        $('#yayeffects').slideUp(PI.slideCb);
    },
 
    slideCb : function() { ... },
 
    unicornCb : function() { ... }
};
 
$(document).ready(PI.onReady);
```

## Optimize Selectors

Selector optimization is less important than it used to be, as more browsers implement `document.querySelectorAll()` and the burden of selection shifts from jQuery to the browser. However, there are still some tips to keep in mind.

### ID-Based Selectors

Beginning your selector with an ID is always best.

```js
// fast
$('#container div.robotarm');
 
// super-fast
$('#container').find('div.robotarm');
```

The `$.fn.find` approach is faster because the first selection is handled without going through the Sizzle selector engine — ID-only selections are handled using `document.getElementById()`, which is extremely fast because it is native to the browser.

### Specificity

Be specific on the right-hand side of your selector, and less specific on the left.

```js
// unoptimized
$('div.data .gonzalez');
 
// optimized
$('.data td.gonzalez');
```

Use `tag.class` if possible on your right-most selector, and just `tag` or just `.class` on the left.

Avoid excessive specificity.

```js
$('.data table.attendees td.gonzalez');
 
// better: drop the middle if possible
$('.data td.gonzalez');
```

A "flatter" DOM also helps improve selector performance, as the selector engine has fewer layers to traverse when looking for an element.

### Avoid the Universal Selector

Selections that specify or imply that a match could be found anywhere can be very slow.

```js
$('.buttons > *');  // extremely expensive
$('.buttons').children();  // much better
 
$('.gender :radio');  // implied universal selection
$('.gender *:radio'); // same thing, explicit now
$('.gender input:radio'); // much better
```

## Use Event Delegation

Event delegation allows you to bind an event handler to one container element (for example, an unordered list) instead of multiple contained elements (for example, list items). jQuery makes this easy with $.fn.live and $.fn.delegate. Where possible, you should use `$.fn.delegate` instead of `$.fn.live`, as it eliminates the need for an unnecessary selection, and its explicit context (vs. `$.fn.live`'s context of `document`) reduces overhead by approximately 80%.

In addition to performance benefits, event delegation also allows you to add new contained elements to your page without having to re-bind the event handlers for them as they're added.

```js
// bad (if there are lots of list items)
$('li.trigger').click(handlerFn);
 
// better: event delegation with $.fn.live
$('li.trigger').live('click', handlerFn);
 
// best: event delegation with $.fn.delegate
// allows you to specify a context easily
$('#myList').delegate('li.trigger', 'click', handlerFn);
```

## Detach Elements to Work With Them

The DOM is slow; you want to avoid manipulating it as much as possible. jQuery introduced `$.fn.detach` in version 1.4 to help address this issue, allowing you to remove an element from the DOM while you work with it.

```js
var $table = $('#myTable');
var $parent = $table.parent();
 
$table.detach();
// ... add lots and lots of rows to table
$parent.append(table);
```

## Use Stylesheets for Changing CSS on Many Elements

If you're changing the CSS of more than 20 elements using `$.fn.css`, consider adding a style tag to the page instead for a nearly 60% increase in speed.

```js
// fine for up to 20 elements, slow after that
$('a.swedberg').css('color', '#asd123');
$('<style type="text/css">a.swedberg { color : #asd123 }</style>')
    .appendTo('head');
```

## Use `$.data` Instead of `$.fn.data`

Using $.data on a DOM element instead of calling $.fn.data on a jQuery selection can be up to 10 times faster. Be sure you understand the difference between a DOM element and a jQuery selection before doing this, though.

```js
// regular
$(elem).data(key,value);
 
// 10x faster
$.data(elem,key,value);
```

## Don't Act on Absent Elements

jQuery won't tell you if you're trying to run a whole lot of code on an empty selection — it will proceed as though nothing's wrong. It's up to you to verify that your selection contains some elements.

```js
// BAD: this runs three functions
// before it realizes there's nothing
// in the selection
$('#nosuchthing').slideUp();
 
// Better
var $mySelection = $('#nosuchthing');
if ($mySelection.length) { $mySelection.slideUp(); }
 
// BEST: add a doOnce plugin
jQuery.fn.doOnce = function(func){
    this.length && func.apply(this);
    return this;
}
 
$('li.cartitems').doOnce(function(){
    // make it ajax! \o/
});
```

This guidance is especially applicable for jQuery UI widgets, which have a lot of overhead even when the selection doesn't contain elements.

## Variable Definition

Variables can be defined in one statement instead of several.

```js
// old & busted
var test = 1;
var test2 = function() { ... };
var test3 = test2(test);
 
// new hotness
var test = 1,
    test2 = function() { ... },
    test3 = test2(test);
```

In self-executing functions, variable definition can be skipped all together.

```js
(function(foo, bar) { ... })(1, 2);
```

## Conditionals

```js
// old way
if (type == 'foo' || type == 'bar') { ... }
 
// better
if (/^(foo|bar)$/.test(type)) { ... }
 
// object literal lookup
if (({ foo : 1, bar : 1 })[type]) { ... }
```

## Don't Treat jQuery as a Black Box

Use the source as your documentation — bookmark [http://bit.ly/jqsource](http://bit.ly/jqsource) and refer to it often.
