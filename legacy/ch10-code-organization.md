---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Code Organization
chapter: 10
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter10
---

# {{ page.title }} <br> Chapter {{ page.chapter }}: {{ page.sub-title }}

## Overview

When you move beyond adding simple enhancements to your website with jQuery and start developing full-blown client-side applications, you need to consider how to organize your code. In this chapter, we'll take a look at various code organization patterns you can use in your jQuery application and explore the RequireJS dependency management and build system.

### Key Concepts

Before we jump into code organization patterns, it's important to understand some concepts that are common to all good code organization patterns.

- Your code should be divided into units of functionality — modules, services, etc. Avoid the temptation to have all of your code in one huge `$(document).ready()` block. This concept, loosely, is known as encapsulation.

- Don't repeat yourself. Identify similarities among pieces of functionality, and use inheritance techniques to avoid repetitive code.

- Despite jQuery's DOM-centric nature, JavaScript applications are not all about the DOM. Remember that not all pieces of functionality need to — or should — have a DOM representation.

- Units of functionality should be [loosely coupled](http://en.wikipedia.org/wiki/Loose_coupling) — a unit of functionality should be able to exist on its own, and communication between units should be handled via a messaging system such as custom events or pub/sub. Stay away from direct communication between units of functionality whenever possible.

The concept of loose coupling can be especially troublesome to developers making their first foray into complex applications, so be mindful of this as you're getting started.

## Encapsulation

The first step to code organization is separating pieces of your application into distinct pieces; sometimes, even just this effort is sufficient to lend

### The Object Literal

An object literal is perhaps the simplest way to encapsulate related code. It doesn't offer any privacy for properties or methods, but it's useful for eliminating anonymous functions from your code, centralizing configuration options, and easing the path to reuse and refactoring.

###### Example 10.1: An object literal

```js
var myFeature = {
myProperty : 'hello',
 
myMethod : function() {
    console.log(myFeature.myProperty);
},
 
init : function(settings) {
    myFeature.settings = settings;
},
 
readSettings : function() {
    console.log(myFeature.settings);
}
};
 
myFeature.myProperty; // 'hello'
myFeature.myMethod(); // logs 'hello'
myFeature.init({ foo : 'bar' });
myFeature.readSettings(); // logs { foo : 'bar' }
```

The object literal above is simply an object assigned to a variable. The object has one property and several methods. All of the properties and methods are public, so any part of your application can see the properties and call methods on the object. While there is an init method, there's nothing requiring that it be called before the object is functional.

How would we apply this pattern to jQuery code? Let's say that we had this code written in the traditional jQuery style:

```js
// clicking on a list item loads some content
// using the list item's ID and hides content
// in sibling list items
$(document).ready(function() {
$('#myFeature li')
.append('<div/>')
.click(function() {
  var $this = $(this);
  var $div = $this.find('div');
  $div.load('foo.php?item=' +
    $this.attr('id'),
    function() {
      $div.show();
      $this.siblings()
        .find('div').hide();
    }
  );
});
});
```

If this were the extent of our application, leaving it as-is would be fine. On the other hand, if this was a piece of a larger application, we'd do well to keep this functionality separate from unrelated functionality. We might also want to move the URL out of the code and into a configuration area. Finally, we might want to break up the chain to make it easier to modify pieces of the functionality later.

###### Example 10.2: Using an object literal for a jQuery feature

```js
var myFeature = {
init : function(settings) {
    myFeature.config = {
        $items : $('#myFeature li'),
        $container : $('<div class="container"></div>'),
        urlBase : '/foo.php?item='
    };
 
    // allow overriding the default config
    $.extend(myFeature.config, settings);
 
    myFeature.setup();
},
 
setup : function() {
    myFeature.config.$items
        .each(myFeature.createContainer)
        .click(myFeature.showItem);
},
 
createContainer : function() {
    var $i = $(this),
        $c = myFeature.config.$container.clone()
                 .appendTo($i);
 
    $i.data('container', $c);
},
 
buildUrl : function() {
    return myFeature.config.urlBase +
           myFeature.$currentItem.attr('id');
},
 
showItem : function() {
    var myFeature.$currentItem = $(this);
    myFeature.getContent(myFeature.showContent);
},
 
getContent : function(callback) {
    var url = myFeature.buildUrl();
    myFeature.$currentItem
        .data('container').load(url, callback);
},
 
showContent : function() {
    myFeature.$currentItem
        .data('container').show();
    myFeature.hideContent();
},
 
hideContent : function() {
    myFeature.$currentItem.siblings()
        .each(function() {
            $(this).data('container').hide();
        });
}
};
 
$(document).ready(myFeature.init);
```

The first thing you'll notice is that this approach is obviously far longer than the original — again, if this were the extent of our application, using an object literal would likely be overkill. Assuming it's not the extent of our application, though, we've gained several things:

- We've broken our feature up into tiny methods. In the future, if we want to change how content is shown, it's clear where to change it. In the original code, this step is much harder to locate.

- We've eliminated the use of anonymous functions.

- We've moved configuration options out of the body of the code and put them in a central location.

- We've eliminated the constraints of the chain, making the code easier to refactor, remix, and rearrange.

For non-trivial features, object literals are a clear improvement over a long stretch of code stuffed in a $(document).ready() block, as they get us thinking about the pieces of our functionality. However, they aren't a whole lot more advanced than simply having a bunch of function declarations inside of that $(document).ready() block.

### The Module Pattern

The module pattern overcomes some of the limitations of the object literal, offering privacy for variables and functions while exposing a public API if desired.

###### Example 10.3: The module pattern

```js
var feature =(function() {
 
// private variables and functions
var privateThing = 'secret',
    publicThing = 'not secret',
 
    changePrivateThing = function() {
        privateThing = 'super secret';
    },
 
    sayPrivateThing = function() {
        console.log(privateThing);
        changePrivateThing();
    };
 
// public API
return {
    publicThing : publicThing,
    sayPrivateThing : sayPrivateThing
}
 
})();
 
feature.publicThing; // 'not secret'
 
feature.sayPrivateThing();
// logs 'secret' and changes the value
// of privateThing
```

In the example above, we self-execute an anonymous function that returns an object. Inside of the function, we define some variables. Because the variables are defined inside of the function, we don't have access to them outside of the function unless we put them in the return object. This means that no code outside of the function has access to the `privateThing` variable or to the `changePrivateThing` function. However, `sayPrivateThing` does have access to `privateThing` and `changePrivateThing`, because both were defined in the same scope as sayPrivateThing.

This pattern is powerful because, as you can gather from the variable names, it can give you private variables and functions while exposing a limited API consisting of the returned object's properties and methods.

Below is a revised version of the previous example, showing how we could create the same feature using the module pattern while only exposing one public method of the module, `showItemByIndex()`.

###### Example 10.4: Using the module pattern for a jQuery feature

```js
$(document).ready(function() {
var feature = (function() {
 
    var $items = $('#myFeature li'),
        $container = $('<div class="container"></div>'),
        $currentItem,
 
        urlBase = '/foo.php?item=',
 
        createContainer = function() {
            var $i = $(this),
                $c = $container.clone().appendTo($i);
 
            $i.data('container', $c);
        },
 
        buildUrl = function() {
            return urlBase + $currentItem.attr('id');
        },
 
        showItem = function() {
            var $currentItem = $(this);
            getContent(showContent);
        },
 
        showItemByIndex = function(idx) {
            $.proxy(showItem, $items.get(idx));
        },
 
        getContent = function(callback) {
            $currentItem.data('container').load(buildUrl(), callback);
        },
 
        showContent = function() {
            $currentItem.data('container').show();
            hideContent();
        },
 
        hideContent = function() {
            $currentItem.siblings()
                .each(function() {
                    $(this).data('container').hide();
            });
        };
 
    $items
        .each(createContainer)
        .click(showItem);
 
    return { showItemByIndex : showItemByIndex };
})();
 
feature.showItemByIndex(0);
});
```

## Managing Dependencies

> #### Note  
>   
> This section is based heavily on the excellent RequireJS documentation at [http://requirejs.org/docs/jquery.html](http://requirejs.org/docs/jquery.html), and is used with the permission of RequireJS author James Burke.

When a project reaches a certain size, managing the script modules for a project starts to get tricky. You need to be sure to sequence the scripts in the right order, and you need to start seriously thinking about combining scripts together into a bundle for deployment, so that only one or a very small number of requests are made to load the scripts. You may also want to load code on the fly, after page load.

RequireJS, a dependency management tool by James Burke, can help you manage the script modules, load them in the right order, and make it easy to combine the scripts later via the RequireJS optimization tool without needing to change your markup. It also gives you an easy way to load scripts after the page has loaded, allowing you to spread out the download size over time.

RequireJS has a module system that lets you define well-scoped modules, but you do not have to follow that system to get the benefits of dependency management and build-time optimizations. Over time, if you start to create more modular code that needs to be reused in a few places, the module format for RequireJS makes it easy to write encapsulated code that can be loaded on the fly. It can grow with you, particularly if you want to incorporate internationalization (i18n) string bundles, to localize your project for different languages, or load some HTML strings and make sure those strings are available before executing code, or even use JSONP services as dependencies.

### Getting RequireJS

The easiest way to use RequireJS with jQuery is to [download a build of jQuery that has RequireJS built in](http://requirejs.org/docs/download.html). This build excludes portions of RequireJS that duplicate jQuery functionality. You may also find it useful to download [a sample jQuery project that uses RequireJS](http://requirejs.org/docs/release/0.11.0/jquery-require-sample.zip).

### Using RequireJS with jQuery

Using RequireJS in your page is simple: just include the jQuery that has RequireJS built in, then require your application files. The following example assumes that the jQuery build, and your other scripts, are all in a `scripts/` directory.

###### Example 10.5: Using RequireJS: A simple example

```html
<!DOCTYPE html>
<html>
<head>
    <title>jQuery+RequireJS Sample Page</title>
    <script src="scripts/require-jquery.js"></script>
    <script>require(["app"]);</script>
</head>
<body>
    <h1>jQuery+RequireJS Sample Page</h1>
</body>
</html>
```

The call to `require(["app"])` tells RequireJS to load the `scripts/app.js` file. RequireJS will load any dependency that is passed to `require()` without a `.js` extension from the same directory as `require-jquery.js`, though this can be configured to behave differently. If you feel more comfortable specifying the whole path, you can also do the following:

```html
<script>require(["scripts/app.js"]);</script>
```

What is in `app.js`? Another call to `require.js` to load all the scripts you need and any init work you want to do for the page. This example `app.js` script loads two plugins, `jquery.alpha.js` and `jquery.beta.js` (not the names of real plugins, just an example). The plugins should be in the same directory as `require-jquery.js`:

###### Example 10.6: A simple JavaScript file with dependencies

```js
require(["jquery.alpha", "jquery.beta"], function() {
//the jquery.alpha.js and jquery.beta.js plugins have been loaded.
$(function() {
    $('body').alpha().beta();
});
});
```

### Creating Reusable Modules with RequireJS

RequireJS makes it easy to define reusable modules via `require.def()`. A RequireJS module can have dependencies that can be used to define a module, and a RequireJS module can return a value — an object, a function, whatever — that can then be consumed by yet other modules.

If your module does not have any dependencies, then just specify the name of the module as the first argument to `require.def()`. The second argument is just an object literal that defines the module's properties. For example:

###### Example 10.7: Defining a RequireJS module that has no dependencies

```js
require.def("my/simpleshirt",
{
    color: "black",
    size: "unisize"
}
);
```

This example would be stored in a my/simpleshirt.js file.

If your module has dependencies, you can specify the dependencies as the second argument to `require.def()` (as an array) and then pass a function as the third argument. The function will be called to define the module once all dependencies have loaded. The function receives the values returned by the dependencies as its arguments (in the same order they were required in the array), and the function should return an object that defines the module.

###### Example 10.8: Defining a RequireJS module with dependencies

```js
require.def("my/shirt",
["my/cart", "my/inventory"],
function(cart, inventory) {
    //return an object to define the "my/shirt" module.
    return {
        color: "blue",
        size: "large"
        addToCart: function() {
            inventory.decrement(this);
            cart.add(this);
        }
    }
}
);
```

In this example, a my/shirt module is created. It depends on my/cart and my/inventory. On disk, the files are structured like this:

```
my/cart.js
my/inventory.js
my/shirt.js
```

The function that defines `my/shirt` is not called until the `my/cart` and `my/inventory` modules have been loaded, and the function receives the modules as the `cart` and `inventory` arguments. The order of the function arguments must match the order in which the dependencies were required in the dependencies array. The object returned by the function call defines the `my/shirt` module. Be defining modules in this way, `my/shirt` does not exist as a global object. Modules that define globals are explicitly discouraged, so multiple versions of a module can exist in a page at a time.

Modules do not have to return objects; any valid return value from a function is allowed.

###### Example 10.9: Defining a RequireJS module that returns a function

```js
require.def("my/title",
["my/dependency1", "my/dependency2"],
function(dep1, dep2) {
    //return a function to define "my/title". It gets or sets
    //the window title.
    return function(title) {
        return title ? (window.title = title) : window.title;
    }
}
);
```

Only one module should be required per JavaScript file.

---
### Optimizing Your Code: The RequireJS Build Tool

Once you incorporate RequireJS for dependency management, your page is set up to be optimized very easily. Download the RequireJS source and place it anywhere you like, preferrably somewhere outside your web development area. For the purposes of this example, the RequireJS source is placed as a sibling to the `webapp` directory, which contains the HTML page and the scripts directory with all the scripts. Complete directory structure:

```
requirejs/ (used for the build tools)
webapp/app.html
webapp/scripts/app.js
webapp/scripts/require-jquery.js
webapp/scripts/jquery.alpha.js
webapp/scripts/jquery.beta.js
```

Then, in the scripts directory that has `require-jquery.js` and app.js, create a file called app.build.js with the following contents:

**A RequireJS build configuration file**

```js
{
appDir: "../",
baseUrl: "scripts/",
dir: "../../webapp-build",
//Comment out the optimize line if you want
//the code minified by Closure Compiler using
//the "simple" optimizations mode
optimize: "none",
 
modules: [
    {
        name: "app"
    }
]
}
```

To use the build tool, you need Java 6 installed. Closure Compiler is used for the JavaScript minification step (if `optimize: "none"` is commented out), and it requires Java 6.

To start the build, go to the webapp/scripts directory, execute the following command:

```
# non-windows systems
../../requirejs/build/build.sh app.build.js
 
# windows systems
..\..\requirejs\build\build.bat app.build.js
```

Now, in the webapp-build directory, `app.js` will have the `app.js` contents, `jquery.alpha.js` and `jquery.beta.js` inlined. If you then load the `app.html` file in the `webapp-build` directory, you should not see any network requests for `jquery.alpha.js` and `jquery.beta.js`.

## Exercises

### Create a Portlet Module

Open the file `/exercises/portlets.html` in your browser. Use the file `/exercises/js/portlets.js`. Your task is to create a portlet creation function that uses the module pattern, such that the following code will work:

```js
var myPortlet = Portlet({
title : 'Curry',
source : 'data/html/curry.html',
initialState : 'open' // or 'closed'
});
 
myPortlet.$element.appendTo('body');
```

Each portlet should be a div with a title, a content area, a button to open/close the portlet, a button to remove the portlet, and a button to refresh the portlet. The portlet returned by the Portlet function should have the following public API:

```
myPortlet.open(); // force open state
myPortlet.close(); // force close state
myPortlet.toggle(); // toggle open/close state
myPortlet.refresh(); // refresh the content
myPortlet.destroy(); // remove the portlet from the page
myPortlet.setSource('data/html/onions.html');
// change the source
```
