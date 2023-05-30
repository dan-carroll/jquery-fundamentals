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

Example 10.1: An object literal

01
var myFeature = {
02
myProperty : 'hello',
03
 
04
myMethod : function() {
05
    console.log(myFeature.myProperty);
06
},
07
 
08
init : function(settings) {
09
    myFeature.settings = settings;
10
},
11
 
12
readSettings : function() {
13
    console.log(myFeature.settings);
14
}
15
};
16
 
17
myFeature.myProperty; // 'hello'
18
myFeature.myMethod(); // logs 'hello'
19
myFeature.init({ foo : 'bar' });
20
myFeature.readSettings(); // logs { foo : 'bar' }
The object literal above is simply an object assigned to a variable. The object has one property and several methods. All of the properties and methods are public, so any part of your application can see the properties and call methods on the object. While there is an init method, there's nothing requiring that it be called before the object is functional.

How would we apply this pattern to jQuery code? Let's say that we had this code written in the traditional jQuery style:

01
// clicking on a list item loads some content
02
// using the list item's ID and hides content
03
// in sibling list items
04
$(document).ready(function() {
05
$('#myFeature li')
06
.append('<div/>')
07
.click(function() {
08
  var $this = $(this);
09
  var $div = $this.find('div');
10
  $div.load('foo.php?item=' +
11
    $this.attr('id'),
12
    function() {
13
      $div.show();
14
      $this.siblings()
15
        .find('div').hide();
16
    }
17
  );
18
});
19
});
If this were the extent of our application, leaving it as-is would be fine. On the other hand, if this was a piece of a larger application, we'd do well to keep this functionality separate from unrelated functionality. We might also want to move the URL out of the code and into a configuration area. Finally, we might want to break up the chain to make it easier to modify pieces of the functionality later.

Example 10.2: Using an object literal for a jQuery feature

01
var myFeature = {
02
init : function(settings) {
03
    myFeature.config = {
04
        $items : $('#myFeature li'),
05
        $container : $('<div class="container"></div>'),
06
        urlBase : '/foo.php?item='
07
    };
08
 
09
    // allow overriding the default config
10
    $.extend(myFeature.config, settings);
11
 
12
    myFeature.setup();
13
},
14
 
15
setup : function() {
16
    myFeature.config.$items
17
        .each(myFeature.createContainer)
18
        .click(myFeature.showItem);
19
},
20
 
21
createContainer : function() {
22
    var $i = $(this),
23
        $c = myFeature.config.$container.clone()
24
                 .appendTo($i);
25
 
26
    $i.data('container', $c);
27
},
28
 
29
buildUrl : function() {
30
    return myFeature.config.urlBase +
31
           myFeature.$currentItem.attr('id');
32
},
33
 
34
showItem : function() {
35
    var myFeature.$currentItem = $(this);
36
    myFeature.getContent(myFeature.showContent);
37
},
38
 
39
getContent : function(callback) {
40
    var url = myFeature.buildUrl();
41
    myFeature.$currentItem
42
        .data('container').load(url, callback);
43
},
44
 
45
showContent : function() {
46
    myFeature.$currentItem
47
        .data('container').show();
48
    myFeature.hideContent();
49
},
50
 
51
hideContent : function() {
52
    myFeature.$currentItem.siblings()
53
        .each(function() {
54
            $(this).data('container').hide();
55
        });
56
}
57
};
58
 
59
$(document).ready(myFeature.init);
The first thing you'll notice is that this approach is obviously far longer than the original — again, if this were the extent of our application, using an object literal would likely be overkill. Assuming it's not the extent of our application, though, we've gained several things:

We've broken our feature up into tiny methods. In the future, if we want to change how content is shown, it's clear where to change it. In the original code, this step is much harder to locate.

We've eliminated the use of anonymous functions.

We've moved configuration options out of the body of the code and put them in a central location.

We've eliminated the constraints of the chain, making the code easier to refactor, remix, and rearrange.

For non-trivial features, object literals are a clear improvement over a long stretch of code stuffed in a $(document).ready() block, as they get us thinking about the pieces of our functionality. However, they aren't a whole lot more advanced than simply having a bunch of function declarations inside of that $(document).ready() block.

The Module Pattern
The module pattern overcomes some of the limitations of the object literal, offering privacy for variables and functions while exposing a public API if desired.

Example 10.3: The module pattern

01
var feature =(function() {
02
 
03
// private variables and functions
04
var privateThing = 'secret',
05
    publicThing = 'not secret',
06
 
07
    changePrivateThing = function() {
08
        privateThing = 'super secret';
09
    },
10
 
11
    sayPrivateThing = function() {
12
        console.log(privateThing);
13
        changePrivateThing();
14
    };
15
 
16
// public API
17
return {
18
    publicThing : publicThing,
19
    sayPrivateThing : sayPrivateThing
20
}
21
 
22
})();
23
 
24
feature.publicThing; // 'not secret'
25
 
26
feature.sayPrivateThing();
27
// logs 'secret' and changes the value
28
// of privateThing
In the example above, we self-execute an anonymous function that returns an object. Inside of the function, we define some variables. Because the variables are defined inside of the function, we don't have access to them outside of the function unless we put them in the return object. This means that no code outside of the function has access to the privateThing variable or to the changePrivateThing function. However, sayPrivateThing does have access to privateThing and changePrivateThing, because both were defined in the same scope as sayPrivateThing.

This pattern is powerful because, as you can gather from the variable names, it can give you private variables and functions while exposing a limited API consisting of the returned object's properties and methods.

Below is a revised version of the previous example, showing how we could create the same feature using the module pattern while only exposing one public method of the module, showItemByIndex().

Example 10.4: Using the module pattern for a jQuery feature

01
$(document).ready(function() {
02
var feature = (function() {
03
 
04
    var $items = $('#myFeature li'),
05
        $container = $('<div class="container"></div>'),
06
        $currentItem,
07
 
08
        urlBase = '/foo.php?item=',
09
 
10
        createContainer = function() {
11
            var $i = $(this),
12
                $c = $container.clone().appendTo($i);
13
 
14
            $i.data('container', $c);
15
        },
16
 
17
        buildUrl = function() {
18
            return urlBase + $currentItem.attr('id');
19
        },
20
 
21
        showItem = function() {
22
            var $currentItem = $(this);
23
            getContent(showContent);
24
        },
25
 
26
        showItemByIndex = function(idx) {
27
            $.proxy(showItem, $items.get(idx));
28
        },
29
 
30
        getContent = function(callback) {
31
            $currentItem.data('container').load(buildUrl(), callback);
32
        },
33
 
34
        showContent = function() {
35
            $currentItem.data('container').show();
36
            hideContent();
37
        },
38
 
39
        hideContent = function() {
40
            $currentItem.siblings()
41
                .each(function() {
42
                    $(this).data('container').hide();
43
            });
44
        };
45
 
46
    $items
47
        .each(createContainer)
48
        .click(showItem);
49
 
50
    return { showItemByIndex : showItemByIndex };
51
})();
52
 
53
feature.showItemByIndex(0);
54
});
Managing Dependencies
Note
This section is based heavily on the excellent RequireJS documentation at http://requirejs.org/docs/jquery.html, and is used with the permission of RequireJS author James Burke.

When a project reaches a certain size, managing the script modules for a project starts to get tricky. You need to be sure to sequence the scripts in the right order, and you need to start seriously thinking about combining scripts together into a bundle for deployment, so that only one or a very small number of requests are made to load the scripts. You may also want to load code on the fly, after page load.

RequireJS, a dependency management tool by James Burke, can help you manage the script modules, load them in the right order, and make it easy to combine the scripts later via the RequireJS optimization tool without needing to change your markup. It also gives you an easy way to load scripts after the page has loaded, allowing you to spread out the download size over time.

RequireJS has a module system that lets you define well-scoped modules, but you do not have to follow that system to get the benefits of dependency management and build-time optimizations. Over time, if you start to create more modular code that needs to be reused in a few places, the module format for RequireJS makes it easy to write encapsulated code that can be loaded on the fly. It can grow with you, particularly if you want to incorporate internationalization (i18n) string bundles, to localize your project for different languages, or load some HTML strings and make sure those strings are available before executing code, or even use JSONP services as dependencies.

Getting RequireJS
The easiest way to use RequireJS with jQuery is to download a build of jQuery that has RequireJS built in. This build excludes portions of RequireJS that duplicate jQuery functionality. You may also find it useful to download a sample jQuery project that uses RequireJS.

Using RequireJS with jQuery
Using RequireJS in your page is simple: just include the jQuery that has RequireJS built in, then require your application files. The following example assumes that the jQuery build, and your other scripts, are all in a scripts/ directory.

Example 10.5: Using RequireJS: A simple example

01
<!DOCTYPE html>
02
<html>
03
<head>
04
    <title>jQuery+RequireJS Sample Page</title>
05
    <script src="scripts/require-jquery.js"></script>
06
    <script>require(["app"]);</script>
07
</head>
08
<body>
09
    <h1>jQuery+RequireJS Sample Page</h1>
10
</body>
11
</html>
The call to require(["app"]) tells RequireJS to load the scripts/app.js file. RequireJS will load any dependency that is passed to require() without a .js extension from the same directory as require-jquery.js, though this can be configured to behave differently. If you feel more comfortable specifying the whole path, you can also do the following:

1
<script>require(["scripts/app.js"]);</script>
What is in app.js? Another call to require.js to load all the scripts you need and any init work you want to do for the page. This example app.js script loads two plugins, jquery.alpha.js and jquery.beta.js (not the names of real plugins, just an example). The plugins should be in the same directory as require-jquery.js:

Example 10.6: A simple JavaScript file with dependencies

1
require(["jquery.alpha", "jquery.beta"], function() {
2
//the jquery.alpha.js and jquery.beta.js plugins have been loaded.
3
$(function() {
4
    $('body').alpha().beta();
5
});
6
});
Creating Reusable Modules with RequireJS
RequireJS makes it easy to define reusable modules via require.def(). A RequireJS module can have dependencies that can be used to define a module, and a RequireJS module can return a value — an object, a function, whatever — that can then be consumed by yet other modules.

If your module does not have any dependencies, then just specify the name of the module as the first argument to require.def(). The second argument is just an object literal that defines the module's properties. For example:

Example 10.7: Defining a RequireJS module that has no dependencies

1
require.def("my/simpleshirt",
2
{
3
    color: "black",
4
    size: "unisize"
5
}
6
);
This example would be stored in a my/simpleshirt.js file.

If your module has dependencies, you can specify the dependencies as the second argument to require.def() (as an array) and then pass a function as the third argument. The function will be called to define the module once all dependencies have loaded. The function receives the values returned by the dependencies as its arguments (in the same order they were required in the array), and the function should return an object that defines the module.

Example 10.8: Defining a RequireJS module with dependencies

01
require.def("my/shirt",
02
["my/cart", "my/inventory"],
03
function(cart, inventory) {
04
    //return an object to define the "my/shirt" module.
05
    return {
06
        color: "blue",
07
        size: "large"
08
        addToCart: function() {
09
            inventory.decrement(this);
10
            cart.add(this);
11
        }
12
    }
13
}
14
);
In this example, a my/shirt module is created. It depends on my/cart and my/inventory. On disk, the files are structured like this:

1
my/cart.js
2
my/inventory.js
3
my/shirt.js
The function that defines my/shirt is not called until the my/cart and my/inventory modules have been loaded, and the function receives the modules as the cart and inventory arguments. The order of the function arguments must match the order in which the dependencies were required in the dependencies array. The object returned by the function call defines the my/shirt module. Be defining modules in this way, my/shirt does not exist as a global object. Modules that define globals are explicitly discouraged, so multiple versions of a module can exist in a page at a time.

Modules do not have to return objects; any valid return value from a function is allowed.

Example 10.9: Defining a RequireJS module that returns a function

01
require.def("my/title",
02
["my/dependency1", "my/dependency2"],
03
function(dep1, dep2) {
04
    //return a function to define "my/title". It gets or sets
05
    //the window title.
06
    return function(title) {
07
        return title ? (window.title = title) : window.title;
08
    }
09
}
10
);
Only one module should be required per JavaScript file.

Optimizing Your Code: The RequireJS Build Tool
Once you incorporate RequireJS for dependency management, your page is set up to be optimized very easily. Download the RequireJS source and place it anywhere you like, preferrably somewhere outside your web development area. For the purposes of this example, the RequireJS source is placed as a sibling to the webapp directory, which contains the HTML page and the scripts directory with all the scripts. Complete directory structure:

1
requirejs/ (used for the build tools)
2
webapp/app.html
3
webapp/scripts/app.js
4
webapp/scripts/require-jquery.js
5
webapp/scripts/jquery.alpha.js
6
webapp/scripts/jquery.beta.js
Then, in the scripts directory that has require-jquery.js and app.js, create a file called app.build.js with the following contents:

A RequireJS build configuration file

01
{
02
appDir: "../",
03
baseUrl: "scripts/",
04
dir: "../../webapp-build",
05
//Comment out the optimize line if you want
06
//the code minified by Closure Compiler using
07
//the "simple" optimizations mode
08
optimize: "none",
09
 
10
modules: [
11
    {
12
        name: "app"
13
    }
14
]
15
}
To use the build tool, you need Java 6 installed. Closure Compiler is used for the JavaScript minification step (if optimize: "none" is commented out), and it requires Java 6.

To start the build, go to the webapp/scripts directory, execute the following command:

1
# non-windows systems
2
../../requirejs/build/build.sh app.build.js
3
 
4
# windows systems
5
..\..\requirejs\build\build.bat app.build.js
Now, in the webapp-build directory, app.js will have the app.js contents, jquery.alpha.js and jquery.beta.js inlined. If you then load the app.html file in the webapp-build directory, you should not see any network requests for jquery.alpha.js and jquery.beta.js.

Exercises
Create a Portlet Module
Open the file /exercises/portlets.html in your browser. Use the file /exercises/js/portlets.js. Your task is to create a portlet creation function that uses the module pattern, such that the following code will work:

1
var myPortlet = Portlet({
2
title : 'Curry',
3
source : 'data/html/curry.html',
4
initialState : 'open' // or 'closed'
5
});
6
 
7
myPortlet.$element.appendTo('body');
Each portlet should be a div with a title, a content area, a button to open/close the portlet, a button to remove the portlet, and a button to refresh the portlet. The portlet returned by the Portlet function should have the following public API:

1
myPortlet.open(); // force open state
2
myPortlet.close(); // force close state
3
myPortlet.toggle(); // toggle open/close state
4
myPortlet.refresh(); // refresh the content
5
myPortlet.destroy(); // remove the portlet from the page
6
myPortlet.setSource('data/html/onions.html');
7
// change the source
