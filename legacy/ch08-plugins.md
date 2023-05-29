---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Plugins
chapter: 8
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter08
---

# {{ page.title }} <br> Chapter {{ page.chapter }}: {{ page.sub-title }}

## What exactly is a plugin?

A jQuery plugin is simply a new method that we use to extend jQuery's prototype object. By extending the prototype object you enable all jQuery objects to inherit any methods that you add. As established, whenever you call jQuery() you're creating a new jQuery object, with all of jQuery's methods inherited.

The idea of a plugin is to do something with a collection of elements. You could consider each method that comes with the jQuery core a plugin, like fadeOut or addClass.

You can make your own plugins and use them privately in your code or you can release them into the wild. There are thousands of jQuery plugins available online. The barrier to creating a plugin of your own is so low that you'll want to do it straight away!

How to create a basic plugin
The notation for creating a typical plugin is as follows:

1
(function($){
2
    $.fn.myNewPlugin = function() {
3
        return this.each(function(){
4
            // do something
5
        });
6
    };
7
}(jQuery));
Don't let that confuse you though. The point of a jQuery plugin is to extend jQuery's prototype object, and that's what's happening on this line:

1
$.fn.myNewPlugin = function() { //...
We wrap this assignment in an immediately-invoked function:

1
(function($){
2
    //...
3
}(jQuery));
This has the effect of creating a "private" scope that allows us to extend jQuery using the dollar symbol without having to risk the possibility that the dollar has been overwritten by another library.

So our actual plugin, thus far, is this:

1
$.fn.myNewPlugin = function() {
2
    return this.each(function(){
3
        // do something
4
    });
5
};
The this keyword within the new plugin refers to the jQuery object on which the plugin is being called.

1
var somejQueryObject = $('#something');
2
 
3
$.fn.myNewPlugin = function() {
4
    alert(this === somejQueryObject);
5
};
6
 
7
somejQueryObject.myNewPlugin(); // alerts 'true'
Your typical jQuery object will contain references to any number of DOM elements, and that's why jQuery objects are often referred to as collections.

So, to do something with a collection we need to loop through it, which is most easily achieved using jQuery's each() method:

1
$.fn.myNewPlugin = function() {
2
    return this.each(function(){
3
 
4
    });
5
};
jQuery's each() method, like most other jQuery methods, returns a jQuery object, thus enabling what we've all come to know and love as 'chaining' ($(...).css().attr()...). We wouldn't want to break this convention so we return the this object. Within this loop you can do whatever you want with each element. Here's an example of a small plugin using some of the techniques we've discussed:

01
(function($){
02
    $.fn.showLinkLocation = function() {
03
        return this.filter('a').each(function(){
04
            $(this).append(
05
                ' (' + $(this).attr('href') + ')'
06
            );
07
        });
08
    };
09
}(jQuery));
10
 
11
// Usage example:
12
$('a').showLinkLocation();
This handy plugin goes through all anchors in the collection and appends the href attribute in brackets.

1
<!-- Before plugin is called: -->
2
<a href="page.html">Foo</a>
3
 
4
<!-- After plugin is called: -->
5
<a href="page.html">Foo (page.html)</a>
Our plugin can be optimised though:

1
(function($){
2
    $.fn.showLinkLocation = function() {
3
        return this.filter('a').append(function(){
4
              return ' (' + this.href + ')';
5
        });
6
    };
7
}(jQuery));
We're using the append method's capability to accept a callback, and the return value of that callback will determine what is appended to each element in the collection. Notice also that we're not using the attr method to retrieve the href attribute, because the native DOM API gives us easy access with the aptly named href property.

Here's another example of a plugin. This one doesn't require us to loop through every elememt with the each() method. Instead, we're simply going to delegate to other jQuery methods directly:

01
(function($){
02
    $.fn.fadeInAndAddClass = function(duration, className) {
03
        return this.fadeIn(duration, function(){
04
            $(this).addClass(className);
05
        });
06
    };
07
}(jQuery));
08
 
09
// Usage example:
10
$('a').fadeInAndAddClass(400, 'finishedFading');
Finding & Evaluating Plugins
Plugins extend the basic jQuery functionality, and one of the most celebrated aspects of the library is its extensive plugin ecosystem. From table sorting to form validation to autocompletion ... if there’s a need for it, chances are good that someone has written a plugin for it.

The quality of jQuery plugins varies widely. Many plugins are extensively tested and well-maintained, but others are hastily created and then ignored. More than a few fail to follow best practices.

Google is your best initial resource for locating plugins, though the jQuery team is working on an improved plugin repository. Once you’ve identified some options via a Google search, you may want to consult the jQuery mailing list or the #jquery IRC channel to get input from others.

When looking for a plugin to fill a need, do your homework. Ensure that the plugin is well-documented, and look for the author to provide lots of examples of its use. Be wary of plugins that do far more than you need; they can end up adding substantial overhead to your page. For more tips on spotting a subpar plugin, read Signs of a poorly written jQuery plugin by Remy Sharp.

Once you choose a plugin, you’ll need to add it to your page. Download the plugin, unzip it if necessary, place it your application’s directory structure, then include the plugin in your page using a script tag (after you include jQuery).

Writing Plugins
Sometimes you want to make a piece of functionality available throughout your code; for example, perhaps you want a single method you can call on a jQuery selection that performs a series of operations on the selection. In this case, you may want to write a plugin.

Most plugins are simply methods created in the $.fn namespace. jQuery guarantees that a method called on a jQuery object will be able to access that jQuery object as this inside the method. In return, your plugin needs to guarantee that it returns the same object it received, unless explicitly documented otherwise.

Here is an example of a simple plugin:

Example 8.1: Creating a plugin to add and remove a class on hover

01
// defining the plugin
02
(function($){
03
    $.fn.hoverClass = function(c) {
04
        return this.hover(
05
            function() { $(this).toggleClass(c); }
06
        );
07
    };
08
})(jQuery);
09
 
10
// using the plugin
11
$('li').hoverClass('hover');
For more on plugin development, read Mike Alsup's essential post, A Plugin Development Pattern. In it, he creates a plugin called $.fn.hilight, which provides support for the metadata plugin if it's present, and provides a centralized method for setting global and instance options for the plugin.

Example 8.2: The Mike Alsup jQuery Plugin Development Pattern

01
//
02
// create closure
03
//
04
(function($) {
05
  //
06
  // plugin definition
07
  //
08
  $.fn.hilight = function(options) {
09
    debug(this);
10
    // build main options before element iteration
11
    var opts = $.extend({}, $.fn.hilight.defaults, options);
12
    // iterate and reformat each matched element
13
    return this.each(function() {
14
      $this = $(this);
15
      // build element specific options
16
      var o = $.meta ? $.extend({}, opts, $this.data()) : opts;
17
      // update element styles
18
      $this.css({
19
        backgroundColor: o.background,
20
        color: o.foreground
21
      });
22
      var markup = $this.html();
23
      // call our format function
24
      markup = $.fn.hilight.format(markup);
25
      $this.html(markup);
26
    });
27
  };
28
  //
29
  // private function for debugging
30
  //
31
  function debug($obj) {
32
    if (window.console && window.console.log)
33
      window.console.log('hilight selection count: ' + $obj.size());
34
  };
35
  //
36
  // define and expose our format function
37
  //
38
  $.fn.hilight.format = function(txt) {
39
    return '<strong>' + txt + '</strong>';
40
  };
41
  //
42
  // plugin defaults
43
  //
44
  $.fn.hilight.defaults = {
45
    foreground: 'red',
46
    background: 'yellow'
47
  };
48
//
49
// end of closure
50
//
51
})(jQuery);
Writing Stateful Plugins with the jQuery UI Widget Factory
Note
This section is based, with permission, on the blog post Building Stateful jQuery Plugins by Scott Gonzalez.

While most existing jQuery plugins are stateless — that is, we call them on an element and that is the extent of our interaction with the plugin — there’s a large set of functionality that doesn’t fit into the basic plugin pattern.

In order to fill this gap, jQuery UI has implemented a more advanced plugin system. The new system manages state, allows multiple functions to be exposed via a single plugin, and provides various extension points. This system is called the widget factory and is exposed as jQuery.widget as part of jQuery UI 1.8; however, it can be used independently of jQuery UI.

To demonstrate the capabilities of the widget factory, we'll build a simple progress bar plugin.

To start, we’ll create a progress bar that just lets us set the progress once. As we can see below, this is done by calling jQuery.widget with two parameters: the name of the plugin to create and an object literal containing functions to support our plugin. When our plugin gets called, it will create a new plugin instance and all functions will be executed within the context of that instance. This is different from a standard jQuery plugin in two important ways. First, the context is an object, not a DOM element. Second, the context is always a single object, never a collection.

Example 8.3: A simple, stateful plugin using the jQuery UI widget factory

1
$.widget("nmk.progressbar", {
2
    _create: function() {
3
        var progress = this.options.value + "%";
4
        this.element
5
            .addClass("progressbar")
6
            .text(progress);
7
    }
8
});
The name of the plugin must contain a namespace; in this case we’ve used the nmk namespace. There is a limitation that namespaces be exactly one level deep — that is, we can't use a namespace like nmk.foo. We can also see that the widget factory has provided two properties for us. this.element is a jQuery object containing exactly one element. If our plugin is called on a jQuery object containing multiple elements, a separate plugin instance will be created for each element, and each instance will have its own this.element. The second property, this.options, is a hash containing key/value pairs for all of our plugin’s options. These options can be passed to our plugin as shown here.

Note
In our example we use the nmk namespace. The ui namespace is reserved for official jQuery UI plugins. When building your own plugins, you should create your own namespace. This makes it clear where the plugin came from and whether it is part of a larger collection.

Example 8.4: Passing options to a widget

1
$("<div></div>")
2
    .appendTo( "body" )
3
    .progressbar({ value: 20 });
When we call jQuery.widget it extends jQuery by adding a method to jQuery.fn (the same way we'd create a standard plugin). The name of the function it adds is based on the name you pass to jQuery.widget, without the namespace; in our case it will create jQuery.fn.progressbar. The options passed to our plugin get set in this.options inside of our plugin instance. As shown below, we can specify default values for any of our options. When designing your API, you should figure out the most common use case for your plugin so that you can set appropriate default values and make all options truly optional.

Example 8.5: Setting default options for a widget

01
$.widget("nmk.progressbar", {
02
    // default options
03
    options: {
04
        value: 0
05
    },
06
 
07
    _create: function() {
08
        var progress = this.options.value + "%";
09
        this.element
10
            .addClass( "progressbar" )
11
            .text( progress );
12
    }
13
});
Adding Methods to a Widget
Now that we can initialize our progress bar, we’ll add the ability to perform actions by calling methods on our plugin instance. To define a plugin method, we just include the function in the object literal that we pass to jQuery.widget. We can also define “private” methods by prepending an underscore to the function name.

Example 8.6: Creating widget methods

01
$.widget("nmk.progressbar", {
02
    options: {
03
        value: 0
04
    },
05
 
06
    _create: function() {
07
        var progress = this.options.value + "%";
08
        this.element
09
            .addClass("progressbar")
10
            .text(progress);
11
    },
12
 
13
    // create a public method
14
    value: function(value) {
15
        // no value passed, act as a getter
16
        if (value === undefined) {
17
            return this.options.value;
18
        // value passed, act as a setter
19
        } else {
20
            this.options.value = this._constrain(value);
21
            var progress = this.options.value + "%";
22
            this.element.text(progress);
23
        }
24
    },
25
 
26
    // create a private method
27
    _constrain: function(value) {
28
        if (value > 100) {
29
            value = 100;
30
        }
31
        if (value < 0) {
32
            value = 0;
33
        }
34
        return value;
35
    }
36
});
To call a method on a plugin instance, you pass the name of the method to the jQuery plugin. If you are calling a method that accepts parameters, you simply pass those parameters after the method name.

Example 8.7: Calling methods on a plugin instance

01
var bar = $("<div></div>")
02
    .appendTo("body")
03
    .progressbar({ value: 20 });
04
 
05
// get the current value
06
alert(bar.progressbar("value"));
07
 
08
// update the value
09
bar.progressbar("value", 50);
10
 
11
// get the current value again
12
alert(bar.progressbar("value"));
Note
Executing methods by passing the method name to the same jQuery function that was used to initialize the plugin may seem odd. This is done to prevent pollution of the jQuery namespace while maintaining the ability to chain method calls.

Working with Widget Options
One of the methods that is automatically available to our plugin is the option method. The option method allows you to get and set options after initialization. This method works exactly like jQuery’s css and attr methods: you can pass just a name to use it as a setter, a name and value to use it as a single setter, or a hash of name/value pairs to set multiple values. When used as a getter, the plugin will return the current value of the option that corresponds to the name that was passed in. When used as a setter, the plugin’s _setOption method will be called for each option that is being set. We can specify a _setOption method in our plugin to react to option changes.

Example 8.8: Responding when an option is set

01
$.widget("nmk.progressbar", {
02
    options: {
03
        value: 0
04
    },
05
 
06
    _create: function() {
07
        this.element.addClass("progressbar");
08
        this._update();
09
    },
10
 
11
    _setOption: function(key, value) {
12
        this.options[key] = value;
13
        this._update();
14
    },
15
 
16
    _update: function() {
17
        var progress = this.options.value + "%";
18
        this.element.text(progress);
19
    }
20
});
Adding Callbacks
One of the easiest ways to make your plugin extensible is to add callbacks so users can react when the state of your plugin changes. We can see below how to add a callback to our progress bar to signify when the progress has reached 100%. The _trigger method takes three parameters: the name of the callback, a native event object that initiated the callback, and a hash of data relevant to the event. The callback name is the only required parameter, but the others can be very useful for users who want to implement custom functionality on top of your plugin. For example, if we were building a draggable plugin, we could pass the native mousemove event when triggering a drag callback; this would allow users to react to the drag based on the x/y coordinates provided by the event object.

Example 8.9: Providing callbacks for user extension

01
$.widget("nmk.progressbar", {
02
    options: {
03
        value: 0
04
    },
05
 
06
    _create: function() {
07
        this.element.addClass("progressbar");
08
        this._update();
09
    },
10
 
11
    _setOption: function(key, value) {
12
        this.options[key] = value;
13
        this._update();
14
    },
15
 
16
    _update: function() {
17
        var progress = this.options.value + "%";
18
        this.element.text(progress);
19
        if (this.options.value == 100) {
20
            this._trigger("complete", null, { value: 100 });
21
        }
22
    }
23
});
Callback functions are essentially just additional options, so you can get and set them just like any other option. Whenever a callback is executed, a corresponding event is triggered as well. The event type is determined by concatenating the plugin name and the callback name. The callback and event both receive the same two parameters: an event object and a hash of data relevant to the event, as we’ll see below.

If your plugin has functionality that you want to allow the user to prevent, the best way to support this is by creating cancelable callbacks. Users can cancel a callback, or its associated event, the same way they cancel any native event: by calling event.preventDefault() or using return false. If the user cancels the callback, the _trigger method will return false so you can implement the appropriate functionality within your plugin.

Example 8.10: Binding to widget events

01
var bar = $("<div></div>")
02
    .appendTo("body")
03
    .progressbar({
04
        complete: function(event, data) {
05
            alert( "Callbacks are great!" );
06
        }
07
    })
08
    .bind("progressbarcomplete", function(event, data) {
09
        alert("Events bubble and support many handlers for extreme flexibility.");
10
        alert("The progress bar value is " + data.value);
11
    });
12
 
13
bar.progressbar("option", "value", 100);
The Widget Factory: Under the Hood

When you call jQuery.widget, it creates a constructor function for your plugin and sets the object literal that you pass in as the prototype for your plugin instances. All of the functionality that automatically gets added to your plugin comes from a base widget prototype, which is defined as jQuery.Widget.prototype. When a plugin instance is created, it is stored on the original DOM element using jQuery.data, with the plugin name as the key.

Because the plugin instance is directly linked to the DOM element, you can access the plugin instance directly instead of going through the exposed plugin method if you want. This will allow you to call methods directly on the plugin instance instead of passing method names as strings and will also give you direct access to the plugin’s properties.

01
var bar = $("<div></div>")
02
    .appendTo("body")
03
    .progressbar()
04
    .data("progressbar" );
05
 
06
// call a method directly on the plugin instance
07
bar.option("value", 50);
08
 
09
// access properties on the plugin instance
10
alert(bar.options.value);
One of the biggest benefits of having a constructor and prototype for a plugin is the ease of extending the plugin. By adding or modifying methods on the plugin’s prototype, we can modify the behavior of all instances of our plugin. For example, if we wanted to add a method to our progress bar to reset the progress to 0% we could add this method to the prototype and it would instantly be available to be called on any plugin instance.

1
$.nmk.progressbar.prototype.reset = function() {
2
    this._setOption("value", 0);
3
};
Cleaning Up
In some cases, it will make sense to allow users to apply and then later unapply your plugin. You can accomplish this via the destroy method. Within the destroy method, you should undo anything your plugin may have done during initialization or later use. The destroy method is automatically called if the element that your plugin instance is tied to is removed from the DOM, so this can be used for garbage collection as well. The default destroy method removes the link between the DOM element and the plugin instance, so it’s important to call the base function from your plugin’s destroy method.

Example 8.11: Adding a destroy method to a widget

01
$.widget( "nmk.progressbar", {
02
    options: {
03
        value: 0
04
    },
05
 
06
    _create: function() {
07
        this.element.addClass("progressbar");
08
        this._update();
09
    },
10
 
11
    _setOption: function(key, value) {
12
        this.options[key] = value;
13
        this._update();
14
    },
15
 
16
    _update: function() {
17
        var progress = this.options.value + "%";
18
        this.element.text(progress);
19
        if (this.options.value == 100 ) {
20
            this._trigger("complete", null, { value: 100 });
21
        }
22
    },
23
 
24
    destroy: function() {
25
        this.element
26
            .removeClass("progressbar")
27
            .text("");
28
 
29
        // call the base destroy function
30
        $.Widget.prototype.destroy.call(this);
31
    }
32
});
Conclusion
The widget factory is only one way of creating stateful plugins. There are a few different models that can be used and each have their own advantages and disadvantages. The widget factory solves lots of common problems for you and can greatly improve productivity, it also greatly improves code reuse, making it a great fit for jQuery UI as well as many other stateful plugins.

Exercises
Make a Table Sortable
For this exercise, your task is to identify, download, and implement a table sorting plugin on the index.html page. When you’re done, all columns in the table on the page should be sortable.

Write a Table-Striping Plugin
Open the file /exercises/index.html in your browser. Use the file /exercises/js/stripe.js. Your task is to write a plugin called "stripe" that you can call on any table element. When the plugin is called on a table element, it should change the color of odd rows in the table body to a user-specified color.

1
$('#myTable').stripe('#cccccc');
Don't forget to return the table so other methods can be chained after the plugin!
