---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Welcome
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter01
---

# {{ page.title }} <br> {{ page.sub-title }}

jQuery is fast becoming a must-have skill for front-end developers. The purpose of this book is to provide an overview of the jQuery JavaScript library; when you're done with the book, you should be able to complete basic tasks using jQuery, and have a solid basis from which to continue your learning. This book was designed as material to be used in a classroom setting, but you may find it useful for individual study.

This is a hands-on class. We will spend a bit of time covering a concept, and then you’ll have the chance to work on an exercise related to the concept. Some of the exercises may seem trivial; others may be downright daunting. In either case, there is no grade; the goal is simply to get you comfortable working your way through problems you’ll commonly be called upon to solve using jQuery. Example solutions to all of the exercises are included in the sample code.

## Getting the Code

The code we’ll be using in this book is hosted in a repository on Github. You can download a .zip or .tar file of the code, then uncompress it to use it on your server. If you’re git-inclined, you’re welcome to clone or fork the repository.

## Software

- You'll want to have the following tools to make the most of the class:

- The Firefox browser

- The Firebug extension for Firefox

- A plain text editor

- For the Ajax portions: A local server (such as MAMP or WAMP), or an FTP or SSH client to access a remote server.

## Adding JavaScript to Your Page

JavaScript can be included inline or by including an external file via a script tag. The order in which you include JavaScript is important: dependencies must be included before the script that depends on them.

For the sake of page performance, JavaScript should be included as close to the end of your HTML as is practical. Multiple JavaScript files should be combined for production use.

###### Example 1.1: An example of inline JavaScript

    <script>
    console.log('hello');
    </script>

###### Example 1.2: An example of including external JavaScript

    <script src='/js/jquery.js'></script>

## JavaScript Debugging

A debugging tool is essential for JavaScript development. Firefox provides a debugger via the Firebug extension; Safari and Chrome provide built-in consoles.

Each console offers:

- single- and multi-line editors for experimenting with JavaScript

- an inspector for looking at the generated source of your page

- a Network or Resources view, to examine network requests

When you are writing JavaScript code, you can use the following methods to send messages to the console:

- `console.log()` for sending general log messages
- `console.dir()` for logging a browseable object
- `console.warn()` for logging warnings
- `console.error()` for logging error messages

Other console methods are also available, though they may differ from one browser to another. The consoles also provide the ability to set break points and watch expressions in your code for debugging purposes.

## Exercises

Most chapters in the book conclude with one or more exercises. For some exercises, you’ll be able to work directly in Firebug; for others, you will need to include other scripts after the jQuery script tag as directed in the individual exercises.

In some cases, you will need to consult the jQuery documentation in order to complete an exercise, as we won’t have covered all of the relevant information in the book. This is by design; the jQuery library is large, and learning to find answers in the documentation is an important part of the process.

Here are a few suggestions for tackling these problems:

- First, make sure you thoroughly understand the problem you're being asked to solve.

- Next, figure out which elements you'll need to access in order to solve the problem, and determine how you'll get those elements. Use Firebug to verify that you're getting the elements you're after.

- Finally, figure out what you need to do with the elements to solve the problem. It can be helpful to write comments explaining what you're going to do before you try to write the code to do it.

Do not be afraid to make mistakes! Do not try to make your code perfect on the first try! Making mistakes and experimenting with solutions is part of learning the library, and you’ll be a better developer for it. Examples of solutions for these exercises are located in the /solutions directory in the sample code.

## Conventions used in this book

Methods that can be called on jQuery objects will be referred to as $.fn.methodName. Methods that exist in the jQuery namespace but that cannot be called on jQuery objects will be referred to as $.methodName. If this doesn't mean much to you, don't worry — it should become clearer as you progress through the book.

###### Example 1.3: Example of an example

    // code examples will appear like this

##### Remarks will appear like this.

> **Note**  
>
> Notes about a topic will appear like this.

## Reference Material

There are any number of articles and blog posts out there that address some aspect of jQuery. Some are phenomenal; some are downright wrong. When you read an article about jQuery, be sure it's talking about the same version as you're using, and resist the urge to just copy and paste — take the time to understand the code in the article.

Here are some excellent resources to use during your jQuery learning. The most important of all is the jQuery source itself: it contains, in code form, complete documentation of the library. It is not a black box — your understanding of the library will grow exponentially if you spend some time visiting it now and again — and I highly recommend bookmarking it in your browser and referring to it often.

- [The jQuery source](http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.js)

- [jQuery documentation](http://api.jquery.com/)

- [jQuery forum](http://forum.jquery.com/)

- [Delicious bookmarks](http://delicious.com/rdmey/jquery-class)

- [#jquery IRC channel on Freenode](http://docs.jquery.com/Discussion#Chat_.2F_IRC_Channel)
