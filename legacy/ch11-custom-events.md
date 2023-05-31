---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Custom Events
chapter: 11
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter11
---

# {{ page.title }} <br> Chapter {{ page.chapter }}: {{ page.sub-title }}

## Introducing Custom Events

We’re all familiar with the basic events — click, mouseover, focus, blur, submit, etc. — that we can latch on to as a user interacts with the browser. Custom events open up a whole new world of event-driven programming. In this chapter, we’ll use jQuery’s custom events system to make a simple Twitter search application.

It can be difficult at first to understand why you'd want to use custom events, when the built-in events seem to suit your needs just fine. It turns out that custom events offer a whole new way of thinking about event-driven JavaScript. Instead of focusing on the element that triggers an action, custom events put the spotlight on the element being acted upon. This brings a bevy of benefits, including:

Behaviors of the target element can easily be triggered by different elements using the same code.

Behaviors can be triggered across multiple, similar, target elements at once.

Behaviors are more clearly associated with the target element in code, making code easier to read and maintain.

Why should you care? An example is probably the best way to explain. Suppose you have a lightbulb in a room in a house. The lightbulb is currently turned on, and it’s controlled by two three-way switches and a clapper:

1
<div class="room" id="kitchen">
2
    <div class="lightbulb on"></div>
3
    <div class="switch"></div>
4
    <div class="switch"></div>
5
    <div class="clapper"></div>
6
</div>
Triggering the clapper or either of the switches will change the state of the lightbulb. The switches and the clapper don’t care what state the lightbulb is in; they just want to change the state.

Without custom events, you might write some code like this:

1
$('.switch, .clapper').click(function() {
2
    var $light = $(this).parent().find('.lightbulb');
3
    if ($light.hasClass('on')) {
4
        $light.removeClass('on').addClass('off');
5
    } else {
6
        $light.removeClass('off').addClass('on');
7
    }
8
});
With custom events, your code might look more like this:

01
$('.lightbulb').bind('changeState', function(e) {
02
    var $light = $(this);
03
    if ($light.hasClass('on')) {
04
        $light.removeClass('on').addClass('off');
05
    } else {
06
        $light.removeClass('off').addClass('on');
07
    }
08
});
09
 
10
$('.switch, .clapper').click(function() {
11
    $(this).parent().find('.lightbulb').trigger('changeState');
12
});
This last bit of code is not that exciting, but something important has happened: we’ve moved the behavior of the lightbulb to the lightbulb, and away from the switches and the clapper.

Let’s make our example a little more interesting. We’ll add another room to our house, along with a master switch, as shown here:

01
<div class="room" id="kitchen">
02
    <div class="lightbulb on"></div>
03
    <div class="switch"></div>
04
    <div class="switch"></div>
05
    <div class="clapper"></div>
06
</div>
07
<div class="room" id="bedroom">
08
    <div class="lightbulb on"></div>
09
    <div class="switch"></div>
10
    <div class="switch"></div>
11
    <div class="clapper"></div>
12
</div>
13
<div id="master_switch"></div>
If there are any lights on in the house, we want the master switch to turn all the lights off; otherwise, we want it to turn all lights on. To accomplish this, we’ll add two more custom events to the lightbulbs: turnOn and turnOff. We’ll make use of them in the changeState custom event, and use some logic to decide which one the master switch should trigger:

01
$('.lightbulb')
02
    .bind('changeState', function(e) {
03
        var $light = $(this);
04
        if ($light.hasClass('on')) {
05
            $light.trigger('turnOff');
06
        } else {
07
            $light.trigger('turnOn');
08
        }
09
    })
10
    .bind('turnOn', function(e) {
11
        $(this).removeClass('off').addClass('on');
12
    })
13
    .bind('turnOff', function(e) {
14
        $(this).removeClass('off').addClass('on');
15
    });
16
 
17
$('.switch, .clapper').click(function() {
18
    $(this).parent().find('.lightbulb').trigger('changeState');
19
});
20
 
21
$('#master_switch').click(function() {
22
    if ($('.lightbulb.on').length) {
23
        $('.lightbulb').trigger('turnOff');
24
    } else {
25
        $('.lightbulb').trigger('turnOn');
26
    }
27
});
Note how the behavior of the master switch is attached to the master switch; the behavior of a lightbulb belongs to the lightbulbs.

Note
If you’re accustomed to object-oriented programming, you may find it useful to think of custom events as methods of objects. Loosely speaking, the object to which the method belongs is created via the jQuery selector. Binding the changeState custom event to all $(‘.light’) elements is akin to having a class called Light with a method of changeState, and then instantiating new Light objects for each element with a classname of light.

Recap: $.fn.bind and $.fn.trigger

In the world of custom events, there are two important jQuery methods: $.fn.bind and $.fn.trigger. In the Events chapter, we saw how to use these methods for working with user events; for this chapter, it's important to remember two things:

The $.fn.bind method takes an event type and an event handling function as arguments. Optionally, it can also receive event-related data as its second argument, pushing the event handling function to the third argument. Any data that is passed will be available to the event handling function in the data property of the event object. The event handling function always receives the event object as its first argument.
The $.fn.trigger method takes an event type as its argument. Optionally, it can also take an array of values. These values will be passed to the event handling function as arguments after the event object.
Here is an example of the usage of $.fn.bind and $.fn.trigger that uses custom data in both cases:

1
$(document).bind('myCustomEvent', { foo : 'bar' }, function(e, arg1, arg2) {
2
    console.log(e.data.foo); // 'bar'
3
    console.log(arg1); // 'bim'
4
    console.log(arg2); // 'baz'
5
});
6
 
7
$(document).trigger('myCustomEvent', [ 'bim', 'baz' ]);
A Sample Application
To demonstrate the power of custom events, we’re going to create a simple tool for searching Twitter. The tool will offer several ways for a user to add search terms to the display: by entering a search term in a text box, by entering multiple search terms in the URL, and by querying Twitter for trending terms.

The results for each term will be shown in a results container; these containers will be able to be expanded, collapsed, refreshed, and removed, either individually or all at once.

When we’re done, it will look like this:

Figure 11.1. Our finished application

Our finished application

The Setup
We’ll start with some basic HTML:

01
<h1>Twitter Search</h1>
02
<input type="button" id="get_trends"
03
    value="Load Trending Terms" />
04
 
05
<form>
06
    <input type="text" class="input_text"
07
        id="search_term" />
08
    <input type="submit" class="input_submit"
09
        value="Add Search Term" />
10
</form>
11
 
12
<div id="twitter">
13
    <div class="template results">
14
        <h2>Search Results for
15
        <span class="search_term"></span></h2>
16
    </div>
17
</div>
This gives us a container (#twitter) for our widget, a template for our results containers (hidden via CSS), and a simple form where users can input a search term. (For the sake of simplicity, we’re going to assume that our application is JavaScript-only and that our users will always have CSS.)

There are two types of objects we’ll want to act on: the results containers, and the Twitter container.

The results containers are the heart of the application. We’ll create a plugin that will prepare each results container once it’s added to the Twitter container. Among other things, it will bind the custom events for each container and add the action buttons at the top right of each container. Each results container will have the following custom events:

refresh
Mark the container as being in the “refreshing” state, and fire the request to fetch the data for the search term.

populate
Receive the returned JSON data and use it to populate the container.

remove
Remove the container from the page after the user verifies the request to do so. Verification can be bypassed by passing true as the second argument to the event handler. The remove event also removes the term associated with the results container from the global object containing the search terms.

collapse
Add a class of collapsed to the container, which will hide the results via CSS. It will also turn the container’s “Collapse” button into an “Expand” button.

expand
Remove the collapsed class from the container. It will also turn the container’s “Expand” button into a “Collapse” button.

The plugin is also responsible for adding the action buttons to the container. It binds a click event to each action’s list item, and uses the list item’s class to determine which custom event will be triggered on the corresponding results container.

001
$.fn.twitterResult = function(settings) {
002
    return this.each(function() {
003
        var $results = $(this),
004
            $actions = $.fn.twitterResult.actions =
005
                $.fn.twitterResult.actions ||
006
                $.fn.twitterResult.createActions(),
007
            $a = $actions.clone().prependTo($results),
008
            term = settings.term;
009
 
010
        $results.find('span.search_term').text(term);
011
 
012
        $.each(
013
            ['refresh', 'populate', 'remove', 'collapse', 'expand'],
014
            function(i, ev) {
015
                $results.bind(
016
                    ev,
017
                    { term : term },
018
                    $.fn.twitterResult.events[ev]
019
                );
020
            }
021
        );
022
 
023
        // use the class of each action to figure out
024
        // which event it will trigger on the results panel
025
        $a.find('li').click(function() {
026
            // pass the li that was clicked to the function
027
            // so it can be manipulated if needed
028
            $results.trigger($(this).attr('class'), [ $(this) ]);
029
        });
030
    });
031
};
032
 
033
$.fn.twitterResult.createActions = function() {
034
    return $('<ul class="actions" />').append(
035
        '<li class="refresh">Refresh</li>' +
036
        '<li class="remove">Remove</li>' +
037
        '<li class="collapse">Collapse</li>'
038
    );
039
};
040
 
041
$.fn.twitterResult.events = {
042
    refresh : function(e) {
043
           // indicate that the results are refreshing
044
        var $this = $(this).addClass('refreshing');
045
 
046
        $this.find('p.tweet').remove();
047
        $results.append('<p class="loading">Loading ...</p>');
048
 
049
        // get the twitter data using jsonp
050
        $.getJSON(
051
            'http://search.twitter.com/search.json?q=' +
052
                escape(e.data.term) + '&rpp=5&callback=?',
053
            function(json) {
054
                $this.trigger('populate', [ json ]);
055
            }
056
        );
057
    },
058
 
059
    populate : function(e, json) {
060
        var results = json.results;
061
        var $this = $(this);
062
 
063
        $this.find('p.loading').remove();
064
 
065
        $.each(results, function(i,result) {
066
            var tweet = '<p class="tweet">' +
067
                '<a href="http://twitter.com/' +
068
                result.from_user +
069
                '">' +
070
                result.from_user +
071
                '</a>: ' +
072
                result.text +
073
                ' <span class="date">' +
074
                result.created_at +
075
                '</span>' +
076
            '</p>';
077
            $this.append(tweet);
078
        });
079
 
080
        // indicate that the results
081
        // are done refreshing
082
        $this.removeClass('refreshing');
083
    },
084
 
085
    remove : function(e, force) {
086
        if (
087
            !force &&
088
            !confirm('Remove panel for term ' + e.data.term + '?')
089
        ) {
090
            return;
091
        }
092
        $(this).remove();
093
 
094
        // indicate that we no longer
095
        // have a panel for the term
096
        search_terms[e.data.term] = 0;
097
    },
098
 
099
    collapse : function(e) {
100
        $(this).find('li.collapse').removeClass('collapse')
101
            .addClass('expand').text('Expand');
102
 
103
        $(this).addClass('collapsed');
104
    },
105
 
106
    expand : function(e) {
107
        $(this).find('li.expand').removeClass('expand')
108
            .addClass('collapse').text('Collapse');
109
 
110
        $(this).removeClass('collapsed');
111
    }
112
};
The Twitter container itself will have just two custom events:

getResults
Receives a search term and checks to determine whether there’s already a results container for the term; if not, adds a results container using the results template, set up the results container using the $.fn.twitterResult plugin discussed above, and then triggers the refresh event on the results container in order to actually load the results. Finally, it will store the search term so the application knows not to re-fetch the term.

getTrends
Queries Twitter for the top 10 trending terms, then iterates over them and triggers the getResults event for each of them, thereby adding a results container for each term.

Here's how the Twitter container bindings look:

01
$('#twitter')
02
    .bind('getResults', function(e, term) {
03
        // make sure we don't have a box for this term already
04
        if (!search_terms[term]) {
05
            var $this = $(this);
06
            var $template = $this.find('div.template');
07
 
08
            // make a copy of the template div
09
            // and insert it as the first results box
10
            $results = $template.clone().
11
                removeClass('template').
12
                insertBefore($this.find('div:first')).
13
                twitterResult({
14
                    'term' : term
15
                });
16
 
17
            // load the content using the "refresh"
18
            // custom event that we bound to the results container
19
            $results.trigger('refresh');
20
            search_terms[term] = 1;
21
        }
22
    })
23
    .bind('getTrends', function(e) {
24
        var $this = $(this);
25
        $.getJSON('http://search.twitter.com/trends.json?callback=?', function(json) {
26
                var trends = json.trends;
27
                $.each(trends, function(i, trend) {
28
                    $this.trigger('getResults', [ trend.name ]);
29
                });
30
            });
31
    });
So far, we’ve written a lot of code that does approximately nothing, but that’s OK. By specifying all the behaviors that we want our core objects to have, we’ve created a solid framework for rapidly building out the interface.

Let’s start by hooking up our text input and the “Load Trending Terms” button. For the text input, we’ll capture the term that was entered in the input and pass it as we trigger the Twitter container’s getResults event. Clicking the “Load Trending Terms” will trigger the Twitter container’s getTrends event:

1
$('form').submit(function(e) {
2
    e.preventDefault();
3
    var term = $('#search_term').val();
4
    $('#twitter').trigger('getResults', [ term ]);
5
});
6
 
7
$('#get_trends').click(function() {
8
    $('#twitter').trigger('getTrends');
9
});
By adding a few buttons with the appropriate IDs, we can make it possible to remove, collapse, expand, and refresh all results containers at once, as shown below. For the remove button, note how we’re passing a value of true to the event handler as its second argument, telling the event handler that we don’t want to verify the removal of individual containers.

1
$.each(['refresh', 'expand', 'collapse'], function(i, ev) {
2
    $('#' + ev).click(function(e) { $('#twitter div.results').trigger(ev); });
3
});
4
 
5
$('#remove').click(function(e) {
6
    if (confirm('Remove all results?')) {
7
        $('#twitter div.results').trigger('remove', [ true ]);
8
    }
9
});
Conclusion
Custom events offer a new way of thinking about your code: they put the emphasis on the target of a behavior, not on the element that triggers it. If you take the time at the outset to spell out the pieces of your application, as well as the behaviors those pieces need to exhibit, custom events can provide a powerful way for you to “talk” to those pieces, either one at a time or en masse. Once the behaviors of a piece have been described, it becomes trivial to trigger those behaviors from anywhere, allowing for rapid creation of and experimentation with interface options. Finally, custom events can enhance code readability and maintainability, by making clear the relationship between an element and its behaviors.

You can see the full application at demos/custom-events.html and demos/js/custom-events.js in the sample code.
