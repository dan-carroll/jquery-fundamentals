---
layout: default
title: jQuery Fundamentals, legacy
sub-title: jQuery Core
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter04
---

# {{ page.title }} <br> {{ page.sub-title }}

## $ vs $()

Until now, we’ve been dealing entirely with methods that are called on a jQuery object. For example:

```js
$('h1').remove();
```

Most jQuery methods are called on jQuery objects as shown above; these methods are said to be part of the `$.fn` namespace, or the “jQuery prototype,” and are best thought of as jQuery object methods.

However, there are several methods that do not act on a selection; these methods are said to be part of the jQuery namespace, and are best thought of as core jQuery methods.

This distinction can be incredibly confusing to new jQuery users. Here’s what you need to remember:

- Methods called on jQuery selections are in the `$.fn` namespace, and automatically receive and return the selection as this.
- Methods in the `$` namespace are generally utility-type methods, and do not work with selections; they are not automatically passed any arguments, and their return value will vary.

There are a few cases where object methods and core methods have the same names, such as `$.each` and `$.fn.each`. In these cases, be extremely careful when reading the documentation that you are exploring the correct method.

## Utility Methods

jQuery offers several utility methods in the `$` namespace. These methods are helpful for accomplishing routine programming tasks. Below are examples of a few of the utility methods; for a complete reference on jQuery utility methods, visit [http://api.jquery.com/category/utilities/](http://api.jquery.com/category/utilities/).

**$.trim**
: Removes leading and trailing whitespace.
: ```js
$.trim('    lots of extra whitespace    ');
// returns 'lots of extra whitespace'
```

**$.each**
: Iterates over arrays and objects.
: ```js
$.each([ 'foo', 'bar', 'baz' ], function(idx, val) {
    console.log('element ' + idx + 'is ' + val);
});
 
$.each({ foo : 'bar', baz : 'bim' }, function(k, v) {
    console.log(k + ' : ' + v);
});
```
: > #### **Note**  
>   
> There is also a method `$.fn.each`, which is used for iterating over a selection of elements.

**$.inArray**
: Returns a value's index in an array, or -1 if the value is not in the array.
: ```js
var myArray = [ 1, 2, 3, 5 ];
 
if ($.inArray(4, myArray) !== -1) {
    console.log('found it!');
}
```

**$.extend**
: Changes the properties of the first object using the properties of subsequent objects.
: ```js
var firstObject = { foo : 'bar', a : 'b' };
var secondObject = { foo : 'baz' };
 
var newObject = $.extend(firstObject, secondObject);
console.log(firstObject.foo); // 'baz'
console.log(newObject.foo);   // 'baz'
```
: If you don't want to change any of the objects you pass to `$.extend`, pass an empty object as the first argument.
: ```js
var firstObject = { foo : 'bar', a : 'b' };
var secondObject = { foo : 'baz' };
 
var newObject = $.extend({}, firstObject, secondObject);
console.log(firstObject.foo); // 'bar'
console.log(newObject.foo);   // 'baz'
```

**$.proxy**
: Returns a function that will always run in the provided scope — that is, sets the meaning of `this` inside the passed function to the second argument.
: ```js
var myFunction = function() { console.log(this); };
var myObject = { foo : 'bar' };
 
myFunction(); // logs window object
 
var myProxyFunction = $.proxy(myFunction, myObject);
myProxyFunction(); // logs myObject object
```
: If you have an object with methods, you can pass the object and the name of a method to return a function that will always run in the scope of the object.
: ```js
var myObject = {
    myFn : function() {
        console.log(this);
    }
};
 
$('#foo').click(myObject.myFn); // logs DOM element #foo
$('#foo').click($.proxy(myObject, 'myFn')); // logs myObject
```

## Checking types

As mentioned in the "JavaScript basics" section, jQuery offers a few basic utility methods for determining the type of a specific value.

###### Example 4.1: Checking the type of an arbitrary value

```js
var myValue = [1, 2, 3];
 
// Using JavaScript's typeof operator to test for primative types
typeof myValue == 'string'; // false
typeof myValue == 'number'; // false
typeof myValue == 'undefined'; // false
typeof myValue == 'boolean'; // false
 
// Using strict equality operator to check for null
myValue === null; // false
 
// Using jQuery's methods to check for non-primative types
jQuery.isFunction(myValue); // false
jQuery.isPlainObject(myValue); // false
jQuery.isArray(myValue); // true
```

## Data Methods

As your work with jQuery progresses, you'll find that there's often data about an element that you want to store with the element. In plain JavaScript, you might do this by adding a property to the DOM element, but you'd have to deal with memory leaks in some browsers. jQuery offers a straightforward way to store data related to an element, and it manages the memory issues for you.

###### Example 4.2: Storing and retrieving data related to an element

```js
$('#myDiv').data('keyName', { foo : 'bar' });
$('#myDiv').data('keyName'); // { foo : 'bar' }
```

You can store any kind of data on an element, and it's hard to overstate the importance of this when you get into complex application development. For the purposes of this class, we'll mostly use `$.fn.data` to store references to other elements.

For example, we may want to establish a relationship between a list item and a div that's inside of it. We could establish this relationship every single time we interact with the list item, but a better solution would be to establish the relationship once, and then store a pointer to the div on the list item using `$.fn.data`:

###### Example 4.3: Storing a relationship between elements using $.fn.data

```js
$('#myList li').each(function() {
    var $li = $(this), $div = $li.find('div.content');
    $li.data('contentDiv', $div);
});
 
// later, we don't have to find the div again;
// we can just read it from the list item's data
var $firstLi = $('#myList li:first');
$firstLi.data('contentDiv').html('new content');
```

In addition to passing `$.fn.data` a single key-value pair to store data, you can also pass an object containing one or more pairs.

## Feature & Browser Detection

Although jQuery eliminates most JavaScript browser quirks, there are still occasions when your code needs to know about the browser environment.

jQuery offers the `$.support object`, as well as the deprecated `$.browser object`, for this purpose. For complete documentation on these objects, visit [http://api.jquery.com/jQuery.support/](http://api.jquery.com/jQuery.support/) and [http://api.jquery.com/jQuery.browser/](http://api.jquery.com/jQuery.support/).

The `$.support` object is dedicated to determining what features a browser supports; it is recommended as a more “future-proof” method of customizing your JavaScript for different browser environments.

The `$.browser` object was deprecated in favor of the `$.support` object, but it will not be removed from jQuery anytime soon. It provides direct detection of the browser brand and version.

## Avoiding Conflicts with Other Libraries

If you are using another JavaScript library that uses the `$` variable, you can run into conflicts with jQuery. In order to avoid these conflicts, you need to put jQuery in no-conflict mode immediately after it is loaded onto the page and before you attempt to use jQuery in your page.

When you put jQuery into no-conflict mode, you have the option of assigning a variable name to replace `$`.

###### Example 4.4: Putting jQuery into no-conflict mode

```js
<script src="prototype.js"></script>
<script src="jquery.js"></script>
<script>var $j = jQuery.noConflict();</script>
```

You can continue to use the standard `$` by wrapping your code in a self-executing anonymous function; this is a standard pattern for plugin authoring, where the author cannot know whether another library will have taken over the `$`.

###### Example 4.5: Using the $ inside a self-executing anonymous function

```js
<script src="prototype.js"></script>
<script src="jquery.js"></script>
<script>
jQuery.noConflict();
 
(function($) {
   // your code here, using the $
})(jQuery);
</script>
```
