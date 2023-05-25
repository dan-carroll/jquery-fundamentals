---
layout: default
title: jQuery Fundamentals, legacy edition
project: legacy-book
page-type: book-index
page: legacy-book-index
---

# {{ page.title }}

### by [Rebecca Murphey](http://rmurphey.com/ "The author's website")

### [http://jqfundamentals.com/legacy/](http://jqfundamentals.com/legacy/ "The original version of this book")

### [http://github.com/rmurphey/jqfundamentals](http://github.com/rmurphey/jqfundamentals "The author's GitHub repository for her book")

### With contributions by James Padolsey, Paul Irish, and others. See the [GitHub repository](http://github.com/rmurphey/jqfundamentals) for a complete history of contributions.

Copyright © 2012

Licensed by Rebecca Murphey under the [Creative Commons Attribution-Share Alike 3.0 United States license](http://creativecommons.org/licenses/by-sa/3.0/us/). You are free to copy, distribute, transmit, and remix this work, provided you attribute the work to Rebecca Murphey as the original author and reference [the GitHub repository for the work](http://github.com/rmurphey/jqfundamentals). If you alter, transform, or build upon this work, you may distribute the resulting work only under the same, similar or a compatible license. Any of the above conditions can be waived if you get permission from the copyright holder. For any reuse or distribution, you must make clear to others the license terms of this work. The best way to do this is with [a link to the license](http://creativecommons.org/licenses/by-sa/3.0/us/).

## Contents

<ul id="contents-list">
  <li>[Chapter 1: Welcome](#)</li>
  <li>[Chapter 2: JavaScript Basics](#)</li>
  <li>[Chapter 3: jQuery Basics](#)</li>
  <li>[Chapter 4: jQuery Core](#)</li>
  <li>[Chapter 5: Events](#)</li>
  <li>[Chapter 6: Effects](#)</li>
  <li>[Chapter 7: Ajax](#)</li>
  <li>[Chapter 8: Plugins](#)</li>
  <li>[Chapter 9: Performance Best Practices](#)</li>
  <li>[Chapter 10: Code Organization](#)</li>
  <li>[Chapter 11: Custom Events](#)</li>
</ul>

## List of Examples

<ul id="examples-list">
  <li>[Example 1.1: An example of inline JavaScript](#)</li>
  <li>[Example 1.2: An example of including external JavaScript](#)</li>
  <li>[Example 1.3: Example of an example](#)</li>
  <li>[Example 2.1: A simple variable declaration](#)</li>
  <li>[Example 2.2: Whitespace has no meaning outside of quotation marks](#)</li>
  <li>[Example 2.3: Parentheses indicate precedence](#)</li>
  <li>[Example 2.4: Tabs enhance readability, but have no special meaning](#)</li>
  <li>[Example 2.5: Concatenation](#)</li>
  <li>[Example 2.6: Multiplication and division](#)</li>
  <li>[Example 2.7: Incrementing and decrementing](#)</li>
  <li>[Example 2.8: Addition vs. concatenation](#)</li>
  <li>[Example 2.9: Forcing a string to act as a number](#)</li>
  <li>[Example 2.10: Forcing a string to act as a number (using the unary-plus operator)](#)</li>
  <li>[Example 2.11: Logical AND and OR operators](#)</li>
  <li>[Example 2.12: Comparison operators](#)</li>
  <li>[Example 2.13: Flow control](#)</li>
  <li>[Example 2.14: Values that evaluate to true](#)</li>
  <li>[Example 2.15: Values that evaluate to false](#)</li>
  <li>[Example 2.16: The ternary operator](#)</li>
  <li>[Example 2.17: A switch statement](#)</li>
  <li>[Example 2.18: Loops](#)</li>
  <li>[Example 2.19: A typical for loop](#)</li>
  <li>[Example 2.20: A typical while loop](#)</li>
  <li>[Example 2.21: A while loop with a combined conditional and incrementer](#)</li>
  <li>[Example 2.22: A do-while loop](#)</li>
  <li>[Example 2.23: Stopping a loop](#)</li>
  <li>[Example 2.24: Skipping to the next iteration of a loop](#)</li>
  <li>[Example 2.25: A simple array](#)</li>
  <li>[Example 2.26: Accessing array items by index](#)</li>
  <li>[Example 2.27: Testing the size of an array](#)</li>
  <li>[Example 2.28: Changing the value of an array item](#)</li>
  <li>[Example 2.29: Adding elements to an array](#)</li>
  <li>[Example 2.30: Working with arrays](#)</li>
  <li>[Example 2.31: Creating an "object literal"](#)</li>
  <li>[Example 2.32: Function Declaration](#)</li>
  <li>[Example 2.33: Named Function Expression](#)</li>
  <li>[Example 2.34: A simple function](#)</li>
  <li>[Example 2.35: A function that returns a value](#)</li>
  <li>[Example 2.36: A function that returns another function](#)</li>
  <li>[Example 2.37: A self-executing anonymous function](#)</li>
  <li>[Example 2.38: Passing an anonymous function as an argument](#)</li>
  <li>[Example 2.39: Passing a named function as an argument](#)</li>
  <li>[Example 2.40: Testing the type of various variables](#)</li>
  <li>[Example 2.41: A function invoked using Function.call](#)</li>
  <li>[Example 2.42: A function created using Function.bind](#)</li>
  <li>[Example 2.43: A function being attached to an object at runtime](#)</li>
  <li>[Example 2.44: Functions have access to variables defined in the same scope](#)</li>
  <li>[Example 2.45: Code outside the scope in which a variable was defined does not have access to the variable](#)</li>
  <li>[Example 2.46: Variables with the same name can exist in different scopes with different values](#)</li>
  <li>[Example 2.47: Functions can "see" changes in variable values after the function is defined](#)</li>
  <li>[Example 2.48: Scope insanity](#)</li>
  <li>[Example 2.49: How to lock in the value of i?](#)</li>
  <li>[Example 2.50: Locking in the value of i with a closure](#)</li>
  <li>[Example 2.51: Using a closure to access inner and outer object instances simultaneously](#)</li>
  <li>[Example 3.1: A $(document).ready() block](#)</li>
  <li>[Example 3.2: Shorthand for $(document).ready()](#)</li>
  <li>[Example 3.3: Passing a named function instead of an anonymous function](#)</li>
  <li>[Example 3.4: Selecting elements by ID](#)</li>
  <li>[Example 3.5: Selecting elements by class name](#)</li>
  <li>[Example 3.6: Selecting elements by attribute](#)</li>
  <li>[Example 3.7: Selecting elements by compound CSS selector](#)</li>
  <li>[Example 3.8: Pseudo-selectors](#)</li>
  <li>[Example 3.9: Testing whether a selection contains elements](#)</li>
  <li>[Example 3.10: Storing selections in a variable](#)</li>
  <li>[Example 3.11: Refining selections](#)</li>
  <li>[Example 3.12: Using form-related pseduo-selectors](#)</li>
  <li>[Example 3.13: Chaining](#)</li>
  <li>[Example 3.14: Formatting chained code](#)</li>
  <li>[Example 3.15: Restoring your original selection using $.fn.end](#)</li>
  <li>[Example 3.16: The $.fn.html method used as a setter](#)</li>
  <li>[Example 3.17: The html method used as a getter](#)</li>
  <li>[Example 3.18: Getting CSS properties](#)</li>
  <li>[Example 3.19: Setting CSS properties](#)</li>
  <li>[Example 3.20: Working with classes](#)</li>
  <li>[Example 3.21: Basic dimensions methods](#)</li>
  <li>[Example 3.22: Setting attributes](#)</li>
  <li>[Example 3.23: Getting attributes](#)</li>
  <li>[Example 3.24: Moving around the DOM using traversal methods](#)</li>
  <li>[Example 3.25: Iterating over a selection](#)</li>
  <li>[Example 3.26: Changing the HTML of an element](#)</li>
  <li>[Example 3.27: Moving elements using different approaches](#)</li>
  <li>[Example 3.28: Making a copy of an element](#)</li>
  <li>[Example 3.29: Creating new elements](#)</li>
  <li>[Example 3.30: Creating a new element with an attribute object](#)</li>
  <li>[Example 3.31: Getting a new element on to the page](#)</li>
  <li>[Example 3.32: Creating and adding an element to the page at the same time](#)</li>
  <li>[Example 3.33: Manipulating a single attribute](#)</li>
  <li>[Example 3.34: Manipulating multiple attributes](#)</li>
  <li>[Example 3.35: Using a function to determine an attribute's new value](#)</li>
  <li>[Example 4.1: Checking the type of an arbitrary value](#)</li>
  <li>[Example 4.2: Storing and retrieving data related to an element](#)</li>
  <li>[Example 4.3: Storing a relationship between elements using $.fn.data](#)</li>
  <li>[Example 4.4: Putting jQuery into no-conflict mode](#)</li>
  <li>[Example 4.5: Using the $ inside a self-executing anonymous function](#)</li>
  <li>[Example 5.1: Event binding using a convenience method](#)</li>
  <li>[Example 5.2: Event biding using the $.fn.bind method](#)</li>
  <li>[Example 5.3: Event binding using the $.fn.bind method with data](#)</li>
  <li>[Example 5.4: Switching handlers using the $.fn.one method](#)</li>
  <li>[Example 5.5: Unbinding all click handlers on a selection](#)</li>
  <li>[Example 5.6: Unbinding a particular click handler](#)</li>
  <li>[Example 5.7: Namespacing events](#)</li>
  <li>[Example 5.8: Binding Multiple Events](#)</li>
  <li>[Example 6.1: A basic use of a built-in effect](#)</li>
  <li>[Example 6.2: Setting the duration of an effect](#)</li>
  <li>[Example 6.3: Augmenting jQuery.fx.speeds with custom speed definitions](#)</li>
  <li>[Example 6.4: Running code when an animation is complete](#)</li>
  <li>[Example 6.5: Run a callback even if there were no elements to animate](#)</li>
  <li>[Example 6.6: Custom effects with $.fn.animate](#)</li>
  <li>[Example 6.7: Per-property easing](#)</li>
  <li>[Example 7.1: Using the core $.ajax method](#)</li>
  <li>[Example 7.2: Using jQuery's Ajax convenience methods](#)</li>
  <li>[Example 7.3: Using $.fn.load to populate an element](#)</li>
  <li>[Example 7.4: Using $.fn.load to populate an element based on a selector](#)</li>
  <li>[Example 7.5: Turning form data into a query string](#)</li>
  <li>[Example 7.6: Creating an array of objects containing form data](#)</li>
  <li>[Example 7.7: Using YQL and JSONP](#)</li>
  <li>[Example 7.8: Setting up a loading indicator using Ajax Events](#)</li>
  <li>[Example 8.1: Creating a plugin to add and remove a class on hover](#)</li>
  <li>[Example 8.2: The Mike Alsup jQuery Plugin Development Pattern](#)</li>
  <li>[Example 8.3: A simple, stateful plugin using the jQuery UI widget factory](#)</li>
  <li>[Example 8.4: Passing options to a widget](#)</li>
  <li>[Example 8.5: Setting default options for a widget](#)</li>
  <li>[Example 8.6: Creating widget methods](#)</li>
  <li>[Example 8.7: Calling methods on a plugin instance](#)</li>
  <li>[Example 8.8: Responding when an option is set](#)</li>
  <li>[Example 8.9: Providing callbacks for user extension](#)</li>
  <li>[Example 8.10: Binding to widget events](#)</li>
  <li>[Example 8.11: Adding a destroy method to a widget](#)</li>
  <li>[Example 10.1: An object literal](#)</li>
  <li>[Example 10.2: Using an object literal for a jQuery feature](#)</li>
  <li>[Example 10.3: The module pattern](#)</li>
  <li>[Example 10.4: Using the module pattern for a jQuery feature](#)</li>
  <li>[Example 10.5: Using RequireJS: A simple example](#)</li>
  <li>[Example 10.6: A simple JavaScript file with dependencies](#)</li>
  <li>[Example 10.7: Defining a RequireJS module that has no dependencies](#)</li>
  <li>[Example 10.8: Defining a RequireJS module with dependencies](#)</li>
  <li>[Example 10.9: Defining a RequireJS module that returns a function](#)</li>
</ul>
