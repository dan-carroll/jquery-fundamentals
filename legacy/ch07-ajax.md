---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Ajax
chapter: 7
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter07
---

# {{ page.title }} <br> Chapter {{ page.chapter }}: {{ page.sub-title }}

## Overview

The XMLHttpRequest method (XHR) allows browsers to communicate with the server without requiring a page reload. This method, also known as Ajax (Asynchronous JavaScript and XML), allows for web pages that provide rich, interactive experiences.

Ajax requests are triggered by JavaScript code; your code sends a request to a URL, and when it receives a response, a callback function can be triggered to handle the response. Because the request is asynchronous, the rest of your code continues to execute while the request is being processed, so it’s imperative that a callback be used to handle the response.

jQuery provides Ajax support that abstracts away painful browser differences. It offers both a full-featured `$.ajax()` method, and simple convenience methods such as `$.get()`, `$.getScript()`, `$.getJSON()`, `$.post()`, and `$().load()`.

Most jQuery applications don’t in fact use XML, despite the name “Ajax”; instead, they transport data as plain HTML or JSON (JavaScript Object Notation).

In general, Ajax does not work across domains. Exceptions are services that provide JSONP (JSON with Padding) support, which allow limited cross-domain functionality.

## Key Concepts

Proper use of Ajax-related jQuery methods requires understanding some key concepts first.

### GET vs. Post

The two most common “methods” for sending a request to a server are GET and POST. It’s important to understand the proper application of each.

The GET method should be used for non-destructive operations — that is, operations where you are only “getting” data from the server, not changing data on the server. For example, a query to a search service might be a GET request. GET requests may be cached by the browser, which can lead to unpredictable behavior if you are not expecting it. GET requests generally send all of their data in a query string.

The POST method should be used for destructive operations — that is, operations where you are changing data on the server. For example, a user saving a blog post should be a POST request. POST requests are generally not cached by the browser; a query string can be part of the URL, but the data tends to be sent separately as post data.

### Data Types

jQuery generally requires some instruction as to the type of data you expect to get back from an Ajax request; in some cases the data type is specified by the method name, and in other cases it is provided as part of a configuration object. There are several options:

text
: For transporting simple strings

html
: For transporting blocks of HTML to be placed on the page

script
: For adding a new script to the page

json
: For transporting JSON-formatted data, which can include strings, arrays, and objects

: > #### Note  
>   
> As of jQuery 1.4, if the JSON data sent by your server isn't properly formatted, the request may fail silently. See [http://json.org](http://json.org/) for details on properly formatting JSON, but as a general rule, use built-in language methods for generating JSON on the server to avoid syntax issues.

jsonp
: For transporting JSON data from another domain

xml
: For transporting data in a custom XML schema

*I am a strong proponent of using the JSON format in most cases, as it provides the most flexibility. It is especially useful for sending both HTML and data at the same time.*

### A is for Asynchronous

The asynchronicity of Ajax catches many new jQuery users off guard. Because Ajax calls are asynchronous by default, the response is not immediately available. Responses can only be handled using a callback. So, for example, the following code will not work:

1
var response;
2
$.get('foo.php', function(r) { response = r; });
3
console.log(response); // undefined!
Instead, we need to pass a callback function to our request; this callback will run when the request succeeds, at which point we can access the data that it returned, if any.

1
$.get('foo.php', function(response) { console.log(response); });
Same-Origin Policy and JSONP
In general, Ajax requests are limited to the same protocol (http or https), the same port, and the same domain as the page making the request. This limitation does not apply to scripts that are loaded via jQuery's Ajax methods.

The other exception is requests targeted at a JSONP service on another domain. In the case of JSONP, the provider of the service has agreed to respond to your request with a script that can be loaded into the page using a <script> tag, thus avoiding the same-origin limitation; that script will include the data you requested, wrapped in a callback function you provide.

Ajax and Firebug
Firebug (or the Webkit Inspector in Chrome or Safari) is an invaluable tool for working with Ajax requests. You can see Ajax requests as they happen in the Console tab of Firebug (and in the Resources > XHR panel of Webkit Inspector), and you can click on a request to expand it and see details such as the request headers, response headers, response content, and more. If something isn't going as expected with an Ajax request, this is the first place to look to track down what's wrong.

jQuery's Ajax-Related Methods
While jQuery does offer many Ajax-related convenience methods, the core $.ajax method is at the heart of all of them, and understanding it is imperative. We'll review it first, and then touch briefly on the convenience methods.

I generally use the $.ajax method and do not use convenience methods. As you'll see, it offers features that the convenience methods do not, and its syntax is more easily understandable, in my opinion.

$.ajax
jQuery’s core $.ajax method is a powerful and straightforward way of creating Ajax requests. It takes a configuration object that contains all the instructions jQuery requires to complete the request. The $.ajax method is particularly valuable because it offers the ability to specify both success and failure callbacks. Also, its ability to take a configuration object that can be defined separately makes it easier to write reusable code. For complete documentation of the configuration options, visit http://api.jquery.com/jQuery.ajax/.

Example 7.1: Using the core $.ajax method

01
$.ajax({
02
    // the URL for the request
03
    url : 'post.php',
04
 
05
    // the data to send
06
    // (will be converted to a query string)
07
    data : { id : 123 },
08
 
09
    // whether this is a POST or GET request
10
    type : 'GET',
11
 
12
    // the type of data we expect back
13
    dataType : 'json',
14
 
15
    // code to run if the request succeeds;
16
    // the response is passed to the function
17
    success : function(json) {
18
        $('<h1/>').text(json.title).appendTo('body');
19
        $('<div class="content"/>')
20
            .html(json.html).appendTo('body');
21
    },
22
 
23
    // code to run if the request fails;
24
    // the raw request and status codes are
25
    // passed to the function
26
    error : function(xhr, status) {
27
        alert('Sorry, there was a problem!');
28
    },
29
 
30
    // code to run regardless of success or failure
31
    complete : function(xhr, status) {
32
        alert('The request is complete!');
33
    }
34
});
Note
A note about the dataType setting: if the server sends back data that is in a different format than you specify, your code may fail, and the reason will not always be clear, because the HTTP response code will not show an error. When working with Ajax requests, make sure your server is sending back the data type you're asking for, and verify that the Content-type header is accurate for the data type. For example, for JSON data, the Content-type header should be application/json.

$.ajax Options
There are many, many options for the $.ajax method, which is part of its power. For a complete list of options, visit http://api.jquery.com/jQuery.ajax/; here are several that you will use frequently:

async
Set to false if the request should be sent synchronously. Defaults to true. Note that if you set this option to false, your request will block execution of other code until the response is received.

cache
Whether to use a cached response if available. Defaults to true for all dataTypes except "script" and "jsonp". When set to false, the URL will simply have a cachebusting parameter appended to it.

complete
A callback function to run when the request is complete, regardless of success or failure. The function receives the raw request object and the text status of the request.

context
The scope in which the callback function(s) should run (i.e. what this will mean inside the callback function(s)). By default, this inside the callback function(s) refers to the object originally passed to $.ajax.

data
The data to be sent to the server. This can either be an object or a query string, such as foo=bar&baz=bim.

dataType
The type of data you expect back from the server. By default, jQuery will look at the MIME type of the response if no dataType is specified.

error
A callback function to run if the request results in an error. The function receives the raw request object and the text status of the request.

jsonp
The callback name to send in a query string when making a JSONP request. Defaults to "callback".

success
A callback function to run if the request succeeds. The function receives the response data (converted to a JavaScript object if the dataType was JSON), as well as the text status of the request and the raw request object.

timeout
The time in milliseconds to wait before considering the request a failure.

traditional
Set to true to use the param serialization style in use prior to jQuery 1.4. For details, see http://api.jquery.com/jQuery.param/.

type
The type of the request, "POST" or "GET". Defaults to "GET". Other request types, such as "PUT" and "DELETE" can be used, but they may not be supported by all browsers.

url
The URL for the request.

The url option is the only required property of the $.ajax configuration object; all other properties are optional.

Convenience Methods
If you don't need the extensive configurability of $.ajax, and you don't care about handling errors, the Ajax convenience functions provided by jQuery can be useful, terse ways to accomplish Ajax requests. These methods are just "wrappers" around the core $.ajax method, and simply pre-set some of the options on the $.ajax method.

The convenience methods provided by jQuery are:

$.get
Perform a GET request to the provided URL.

$.post
Perform a POST request to the provided URL.

$.getScript
Add a script to the page.

$.getJSON
Perform a GET request, and expect JSON to be returned.

In each case, the methods take the following arguments, in order:

url
The URL for the request. Required.

data
The data to be sent to the server. Optional. This can either be an object or a query string, such as foo=bar&baz=bim.

Note
This option is not valid for $.getScript.

success callback
A callback function to run if the request succeeds. Optional. The function receives the response data (converted to a JavaScript object if the data type was JSON), as well as the text status of the request and the raw request object.

data type
The type of data you expect back from the server. Optional.

Note
This option is only applicable for methods that don't already specify the data type in their name.

Example 7.2: Using jQuery's Ajax convenience methods

01
// get plain text or html
02
$.get('/users.php', { userId : 1234 }, function(resp) {
03
    console.log(resp);
04
});
05
 
06
// add a script to the page, then run a function defined in it
07
$.getScript('/static/js/myScript.js', function() {
08
    functionFromMyScript();
09
});
10
 
11
// get JSON-formatted data from the server
12
$.getJSON('/details.php', function(resp) {
13
    $.each(resp, function(k, v) {
14
        console.log(k + ' : ' + v);
15
    });
16
});
$.fn.load
The $.fn.load method is unique among jQuery’s Ajax methods in that it is called on a selection. The $.fn.load method fetches HTML from a URL, and uses the returned HTML to populate the selected element(s). In addition to providing a URL to the method, you can optionally provide a selector; jQuery will fetch only the matching content from the returned HTML.

Example 7.3: Using $.fn.load to populate an element

1
$('#newContent').load('/foo.html');
Example 7.4: Using $.fn.load to populate an element based on a selector

1
$('#newContent').load('/foo.html #myDiv h1:first', function(html) {
2
  alert('Content updated!');
3
});
Ajax and Forms
jQuery’s ajax capabilities can be especially useful when dealing with forms. The jQuery Form Plugin is a well-tested tool for adding Ajax capabilities to forms, and you should generally use it for handling forms with Ajax rather than trying to roll your own solution for anything remotely complex. That said, there are a two jQuery methods you should know that relate to form processing in jQuery: $.fn.serialize and $.fn.serializeArray.

Example 7.5: Turning form data into a query string

1
$('#myForm').serialize();
Example 7.6: Creating an array of objects containing form data

1
$('#myForm').serializeArray();
2
 
3
// creates a structure like this:
4
[
5
    { name : 'field1', value : 123 },
6
    { name : 'field2', value : 'hello world' }
7
]
Working with JSONP
The advent of JSONP — essentially a consensual cross-site scripting hack — has opened the door to powerful mashups of content. Many prominent sites provide JSONP services, allowing you access to their content via a predefined API. A particularly great source of JSONP-formatted data is the Yahoo! Query Language, which we'll use in the following example to fetch news about cats.

Example 7.7: Using YQL and JSONP

01
$.ajax({
02
    url : 'http://query.yahooapis.com/v1/public/yql',
03
 
04
    // the name of the callback parameter,
05
    // as specified by the YQL service
06
    jsonp : 'callback',
07
 
08
    // tell jQuery we're expecting JSONP
09
    dataType : 'jsonp',
10
 
11
    // tell YQL what we want and that we want JSON
12
    data : {
13
        q : 'select title,abstract,url from search.news where query="cat"',
14
        format : 'json'
15
    },
16
 
17
    // work with the response
18
    success : function(response) {
19
        console.log(response);
20
    }
21
});
jQuery handles all the complex aspects of JSONP behind-the-scenes — all we have to do is tell jQuery the name of the JSONP callback parameter specified by YQL ("callback" in this case), and otherwise the whole process looks and feels like a normal Ajax request.

Ajax Events
Often, you’ll want to perform an operation whenever an Ajax requests starts or stops, such as showing or hiding a loading indicator. Rather than defining this behavior inside every Ajax request, you can bind Ajax events to elements just like you'd bind other events. For a complete list of Ajax events, visit http://docs.jquery.com/Ajax_Events.

Example 7.8: Setting up a loading indicator using Ajax Events

1
$('#loading_indicator')
2
    .ajaxStart(function() { $(this).show(); })
3
    .ajaxStop(function() { $(this).hide(); });
Exercises
Load External Content
Open the file /exercises/index.html in your browser. Use the file /exercises/js/load.js. Your task is to load the content of a blog item when a user clicks on the title of the item.

Create a target div after the headline for each blog post and store a reference to it on the headline element using $.fn.data.
Bind a click event to the headline that will use the $.fn.load method to load the appropriate content from /exercises/data/blog.html into the target div. Don't forget to prevent the default action of the click event.
Note that each blog headline in index.html includes a link to the post. You'll need to leverage the href of that link to get the proper content from blog.html. Once you have the href, here's one way to process it into an ID that you can use as a selector in $.fn.load:

1
var href = 'blog.html#post1';
2
var tempArray = href.split('#');
3
var id = '#' + tempArray[1];
Remember to make liberal use of console.log to make sure you're on the right path!

Load Content Using JSON
Open the file /exercises/index.html in your browser. Use the file /exercises/js/specials.js. Your task is to show the user details about the special for a given day when the user selects a day from the select dropdown.

Append a target div after the form that's inside the #specials element; this will be where you put information about the special once you receive it.

Bind to the change event of the select element; when the user changes the selection, send an Ajax request to /exercises/data/specials.json.
When the request returns a response, use the value the user selected in the select (hint: $.fn.val) to look up information about the special in the JSON response.
Add some HTML about the special to the target div you created.

Finally, because the form is now Ajax-enabled, remove the submit button from the form.

Note that we're loading the JSON every time the user changes their selection. How could we change the code so we only make the request once, and then use a cached response when the user changes their choice in the select?
