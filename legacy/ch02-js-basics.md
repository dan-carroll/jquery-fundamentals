---
layout: default
title: jQuery Fundamentals, legacy
sub-title: Javascript Basics
project: legacy-book
page-type: book-chapter
page: legacy-book-chapter02
---

# {{ page.title }} <br> {{ page.sub-title }}

## Overview

jQuery is built on top of JavaScript, a rich and expressive language in its own right. This section covers the basic concepts of JavaScript, as well as some frequent pitfalls for people who have not used JavaScript before. While it will be of particular value to people with no programming experience, even people who have used other programming languages may benefit from learning about some of the peculiarities of JavaScript.

If you’re interested in learning more about the JavaScript language, I highly recommend JavaScript: The Good Parts by Douglas Crockford.

## Syntax Basics

Understanding statements, variable naming, whitespace, and other basic JavaScript syntax.

###### Example 2.1: A simple variable declaration

 ```javascript
 var foo = 'hello world';
```

###### Example 2.2: Whitespace has no meaning outside of quotation marks

```javascript
var foo =         'hello world';
```

###### Example 2.3: Parentheses indicate precedence

```javascript
2 * 3 + 5;    // returns 11; multiplication happens first  
2 * (3 + 5);  // returns 16; addition happens first
```

###### Example 2.4: Tabs enhance readability, but have no special meaning

```javascript
var foo = function() {  
    console.log('hello');  
};
```

## Operators

### Basic Operators

Basic operators allow you to manipulate values.

###### Example 2.5: Concatenation

```javascript
var foo = 'hello';  
var bar = 'world';  
   
console.log(foo + ' ' + bar); // 'hello world'
```

###### Example 2.6: Multiplication and division

```javascript
2 * 3;  
2 / 3;
```

###### Example 2.7: Incrementing and decrementing

```javascript
var i = 1;  
   
var j = ++i;  // pre-increment:  j equals 2; i equals 2  
var k = i++;  // post-increment: k equals 2; i equals 3
```

### Operations on Numbers & Strings

In JavaScript, numbers and strings will occasionally behave in ways you might not expect.

###### Example 2.8: Addition vs. concatenation

```javascript
var foo = 1;  
var bar = '2';  
   
console.log(foo + bar);  // 12. uh oh
```

###### Example 2.9: Forcing a string to act as a number

```javascript
var foo = 1;  
var bar = '2';  
   
// coerce the string to a number  
console.log(foo + Number(bar));
```

The Number constructor, when called as a function (like above) will have the effect of casting its argument into a number. You could also use the unary plus operator, which does the same thing:

###### Example 2.10: Forcing a string to act as a number (using the unary-plus operator)

```javascript
console.log(foo + +bar);
```

### Logical Operators

Logical operators allow you to evaluate a series of operands using AND and OR operations.

###### Example 2.11: Logical AND and OR operators

```javascript
var foo = 1;  
var bar = 0;  
var baz = 2;  
   
foo || bar;   // returns 1, which is true  
bar || foo;   // returns 1, which is true  
   
foo && bar;   // returns 0, which is false  
foo && baz;   // returns 2, which is true  
baz && foo;   // returns 1, which is true
```

Though it may not be clear from the example, the operator returns the value of the first truthy operand, or in cases where neither operand is truthy, it'll return the last of both operands.

The &amp;&amp; operator returns the value of the first false operand, or the value of the last operand if both operands are truthy.

Be sure to consult [the section called “Truthy and Falsy Things”](http://jqfundamentals.com/legacy/#Truthy%20and%20Falsy%20Things) for more details on which values evaluate to true and which evaluate to false.

> Note  
>   
> You'll sometimes see developers use these logical operators for flow control instead of using if statements. For example:  
>  
> // do something with foo if foo is truthy  
> foo && doSomething(foo);  
>    
> // set bar to baz if baz is truthy;  
> // otherwise, set it to the return  
> // value of createBar()  
> var bar = baz || createBar();  
>   
> This style is quite elegant and pleasantly terse; that said, it can be really hard to read, especially for beginners. I bring it up here so you'll recognize it in code you read, but I don't recommend using it until you're extremely comfortable with what it means and how you can expect it to behave.

### Comparison Operators

Comparison operators allow you to test whether values are equivalent or whether values are identical.

###### Example 2.12: Comparison operators

```javascript
var foo = 1;
var bar = 0;
var baz = '1';
var bim = 2;
 
foo == bar;   // returns false
foo != bar;   // returns true
foo == baz;   // returns true; careful!
 
foo === baz;             // returns false
foo !== baz;             // returns true
foo === parseInt(baz);   // returns true
 
foo > bim;    // returns false
bim > baz;    // returns true
foo <= baz;   // returns true
```

### Conditional Code

Sometimes you only want to run a block of code under certain conditions. Flow control — via if and else blocks — lets you run code only under certain conditions.

###### Example 2.13: Flow control

```javascript
var foo = true;
var bar = false;
 
if (bar) {
    // this code will never run
    console.log('hello!');
}
 
if (bar) {
    // this code won't run
} else {
    if (foo) {
        // this code will run
    } else {
        // this code would run if foo and bar were both false
    }
}
```

Note

While curly braces aren't strictly required around single-line if statements, using them consistently, even when they aren't strictly required, makes for vastly more readable code.

Be mindful not to define functions with the same name multiple times within separate if/else blocks, as doing so may not have the expected result.

Truthy and Falsy Things
In order to use flow control successfully, it's important to understand which kinds of values are "truthy" and which kinds of values are "falsy." Sometimes, values that seem like they should evaluate one way actually evaluate another.

Example 2.14: Values that evaluate to true

1
'0';
2
'any string';
3
[];  // an empty array
4
{};  // an empty object
5
1;   // any non-zero number
Example 2.15: Values that evaluate to false

1
0;
2
'';  // an empty string
3
NaN; // JavaScript's "not-a-number" variable
4
null;
5
undefined;  // be careful -- undefined can be redefined!
Conditional Variable Assignment with The Ternary Operator
Sometimes you want to set a variable to a value depending on some condition. You could use an if/else statement, but in many cases the ternary operator is more convenient. [Definition: The ternary operator tests a condition; if the condition is true, it returns a certain value, otherwise it returns a different value.]

Example 2.16: The ternary operator

1
// set foo to 1 if bar is true;
2
// otherwise, set foo to 0
3
var foo = bar ? 1 : 0;
While the ternary operator can be used without assigning the return value to a variable, this is generally discouraged.

Switch Statements
Rather than using a series of if/else if/else blocks, sometimes it can be useful to use a switch statement instead. [Definition: Switch statements look at the value of a variable or expression, and run different blocks of code depending on the value.]

Example 2.17: A switch statement

01
switch (foo) {
02
 
03
    case 'bar':
04
        alert('the value was bar -- yay!');
05
    break;
06
 
07
    case 'baz':
08
        alert('boo baz :(');
09
    break;
10
 
11
    default:
12
        alert('everything else is just ok');
13
    break;
14
 
15
}
Switch statements have somewhat fallen out of favor in JavaScript, because often the same behavior can be accomplished by creating an object that has more potential for reuse, testing, etc. For example:

01
var stuffToDo = {
02
    'bar' : function() {
03
        alert('the value was bar -- yay!');
04
    },
05
 
06
    'baz' : function() {
07
        alert('boo baz :(');
08
    },
09
 
10
    'default' : function() {
11
        alert('everything else is just ok');
12
    }
13
};
14
 
15
if (stuffToDo[foo]) {
16
    stuffToDo[foo]();
17
} else {
18
    stuffToDo['default']();
19
}
We'll look at objects in greater depth later in this chapter.

Loops
Loops let you run a block of code a certain number of times.

Example 2.18: Loops

1
// logs 'try 0', 'try 1', ..., 'try 4'
2
for (var i=0; i<5; i++) {
3
    console.log('try ' + i);
4
}
Note that in Loops even though we use the keyword var before the variable name i, this does not "scope" the variable i to the loop block. We'll discuss scope in depth later in this chapter.

The for loop
A for loop is made up of four statements and has the following structure:

1
for ([initialisation]; [conditional]; [iteration])
2
 [loopBody]
The initialisation statement is executed only once, before the loop starts. It gives you an opportunity to prepare or declare any variables.

The conditional statement is executed before each iteration, and its return value decides whether or not the loop is to continue. If the conditional statement evaluates to a falsey value then the loop stops.

The iteration statement is executed at the end of each iteration and gives you an opportunity to change the state of important variables. Typically, this will involve incrementing or decrementing a counter and thus bringing the loop ever closer to its end.

The loopBody statement is what runs on every iteration. It can contain anything you want. You'll typically have multiple statements that need to be executed and so will wrap them in a block ( {...}).

Here's a typical for loop:

Example 2.19: A typical for loop

1
for (var i = 0, limit = 100; i < limit; i++) {
2
    // This block will be executed 100 times
3
    console.log('Currently at ' + i);
4
    // Note: the last log will be "Currently at 99"
5
}
The while loop
A while loop is similar to an if statement, except that its body will keep executing until the condition evaluates to false.

1
while ([conditional]) [loopBody]
Here's a typical while loop:

Example 2.20: A typical while loop

1
var i = 0;
2
while (i < 100) {
3
 
4
    // This block will be executed 100 times
5
    console.log('Currently at ' + i);
6
 
7
    i++; // increment i
8
 
9
}
You'll notice that we're having to increment the counter within the loop's body. It is possible to combine the conditional and incrementer, like so:

Example 2.21: A while loop with a combined conditional and incrementer

1
var i = -1;
2
while (++i < 100) {
3
    // This block will be executed 100 times
4
    console.log('Currently at ' + i);
5
}
Notice that we're starting at -1 and using the prefix incrementer (++i).

The do-while loop
This is almost exactly the same as the while loop, except for the fact that the loop's body is executed at least once before the condition is tested.

1
do [loopBody] while ([conditional])
Here's a do-while loop:

Example 2.22: A do-while loop

1
do {
2
 
3
    // Even though the condition evaluates to false
4
    // this loop's body will still execute once.
5
 
6
    alert('Hi there!');
7
 
8
} while (false);
These types of loops are quite rare since only few situations require a loop that blindly executes at least once. Regardless, it's good to be aware of it.

Breaking and continuing
Usually, a loop's termination will result from the conditional statement not evaluating to true, but it is possible to stop a loop in its tracks from within the loop's body with the break statement.

Example 2.23: Stopping a loop

1
for (var i = 0; i < 10; i++) {
2
    if (something) {
3
        break;
4
    }
5
}
You may also want to continue the loop without executing more of the loop's body. This is done using the continue statement.

Example 2.24: Skipping to the next iteration of a loop

01
for (var i = 0; i < 10; i++) {
02
 
03
    if (something) {
04
        continue;
05
    }
06
 
07
    // The following statement will only be executed
08
    // if the conditional 'something' has not been met
09
    console.log('I have been reached');
10
 
11
}
Reserved Words
JavaScript has a number of “reserved words,” or words that have special meaning in the language. You should avoid using these words in your code except when using them with their intended meaning.

abstract
boolean
break
byte
case
catch
char
class
const
continue
debugger
default
delete
do
double
else
enum
export
extends
final
finally
float
for
function
goto
if
implements
import
in
instanceof
int
interface
long
native
new
package
private
protected
public
return
short
static
super
switch
synchronized
this
throw
throws
transient
try
typeof
var
void
volatile
while
with
Arrays
Arrays are zero-indexed lists of values. They are a handy way to store a set of related items of the same type (such as strings), though in reality, an array can include multiple types of items, including other arrays.

Example 2.25: A simple array

1
var myArray = [ 'hello', 'world' ];
Example 2.26: Accessing array items by index

1
var myArray = [ 'hello', 'world', 'foo', 'bar' ];
2
console.log(myArray[3]);   // logs 'bar'
Example 2.27: Testing the size of an array

1
var myArray = [ 'hello', 'world' ];
2
console.log(myArray.length);   // logs 2
Example 2.28: Changing the value of an array item

1
var myArray = [ 'hello', 'world' ];
2
myArray[1] = 'changed';
While it's possible to change the value of an array item as shown in “Changing the value of an array item”, it's generally not advised.

Example 2.29: Adding elements to an array

1
var myArray = [ 'hello', 'world' ];
2
myArray.push('new');
Example 2.30: Working with arrays

1
var myArray = [ 'h', 'e', 'l', 'l', 'o' ];
2
var myString = myArray.join('');   // 'hello'
3
var mySplit = myString.split('');  // [ 'h', 'e', 'l', 'l', 'o' ]
Objects
Objects contain one or more key-value pairs. The key portion can be any string. The value portion can be any type of value: a number, a string, an array, a function, or even another object.

[Definition: When one of these values is a function, it’s called a method of the object.] Otherwise, they are called properties.

As it turns out, nearly everything in JavaScript is an object — arrays, functions, numbers, even strings — and they all have properties and methods.

Example 2.31: Creating an "object literal"

01
var myObject = {
02
    sayHello : function() {
03
        console.log('hello');
04
    },
05
 
06
    myName : 'Rebecca'
07
};
08
 
09
myObject.sayHello();            // logs 'hello'
10
console.log(myObject.myName);   // logs 'Rebecca'
Note
When creating object literals, you should note that the key portion of each key-value pair can be written as any valid JavaScript identifier, a string (wrapped in quotes) or a number:

1
var myObject = {
2
    validIdentifier: 123,
3
    'some string': 456,
4
    99999: 789
5
};
Object literals can be extremely useful for code organization; for more information, read Using Objects to Organize Your Code by Rebecca Murphey.

Functions
Functions contain blocks of code that need to be executed repeatedly. Functions can take zero or more arguments, and can optionally return a value.

Functions can be created in a variety of ways:

Example 2.32: Function Declaration

1
function foo() { /* do something */ }
Example 2.33: Named Function Expression

1
var foo = function() { /* do something */ }
I prefer the named function expression method of setting a function's name, for some rather in-depth and technical reasons. You are likely to see both methods used in others' JavaScript code.

Using Functions
Example 2.34: A simple function

1
var greet = function(person, greeting) {
2
    var text = greeting + ', ' + person;
3
    console.log(text);
4
};
5
 
6
 
7
greet('Rebecca', 'Hello');
Example 2.35: A function that returns a value

1
var greet = function(person, greeting) {
2
    var text = greeting + ', ' + person;
3
    return text;
4
};
5
 
6
console.log(greet('Rebecca','hello'));
Example 2.36: A function that returns another function

1
var greet = function(person, greeting) {
2
    var text = greeting + ', ' + person;
3
    return function() { console.log(text); };
4
};
5
 
6
 
7
var greeting = greet('Rebecca', 'Hello');
8
greeting();
Self-Executing Anonymous Functions
A common pattern in JavaScript is the self-executing anonymous function. This pattern creates a function expression and then immediately executes the function. This pattern is extremely useful for cases where you want to avoid polluting the global namespace with your code — no variables declared inside of the function are visible outside of it.

Example 2.37: A self-executing anonymous function

1
(function(){
2
    var foo = 'Hello world';
3
})();
4
 
5
 
6
console.log(foo);   // undefined!
Functions as Arguments
In JavaScript, functions are "first-class citizens" &mdash they can be assigned to variables or passed to other functions as arguments. Passing functions as arguments is an extremely common idiom in jQuery.

Example 2.38: Passing an anonymous function as an argument

1
var myFn = function(fn) {
2
    var result = fn();
3
    console.log(result);
4
};
5
 
6
myFn(function() { return 'hello world'; });   // logs 'hello world'
Example 2.39: Passing a named function as an argument

01
var myFn = function(fn) {
02
    var result = fn();
03
    console.log(result);
04
};
05
 
06
var myOtherFn = function() {
07
    return 'hello world';
08
};
09
 
10
myFn(myOtherFn);   // logs 'hello world'
Testing Type
JavaScript offers a way to test the "type" of a variable. However, the result can be confusing — for example, the type of an Array is "object".

It's common practice to use the typeof operator when trying to determining the type of a specific value.

Example 2.40: Testing the type of various variables

01
var myFunction = function() {
02
    console.log('hello');
03
};
04
 
05
var myObject = {
06
    foo : 'bar'
07
};
08
 
09
var myArray = [ 'a', 'b', 'c' ];
10
 
11
var myString = 'hello';
12
 
13
var myNumber = 3;
14
 
15
typeof myFunction;   // returns 'function'
16
typeof myObject;     // returns 'object'
17
typeof myArray;      // returns 'object' -- careful!
18
typeof myString;     // returns 'string';
19
typeof myNumber;     // returns 'number'
20
 
21
typeof null;         // returns 'object' -- careful!
22
 
23
 
24
if (myArray.push && myArray.slice && myArray.join) {
25
    // probably an array
26
    // (this is called "duck typing")
27
}
28
 
29
if (Object.prototype.toString.call(myArray) === '[object Array]') {
30
    // Definitely an array!
31
    // This is widely considered as the most robust way
32
    // to determine if a specific value is an Array.
33
}
jQuery offers utility methods to help you determine the type of an arbitrary value. These will be covered later.

The this keyword
In JavaScript, as in most object-oriented programming languages, this is a special keyword that is used within methods to refer to the object on which a method is being invoked. The value of this is determined using a simple series of steps:

If the function is invoked using Function.call or Function.apply, this will be set to the first argument passed to call/apply. If the first argument passed to call/apply is null or undefined, this will refer to the global object (which is the window object in Web browsers).
If the function being invoked was created using Function.bind, this will be the first argument that was passed to bind at the time the function was created.
If the function is being invoked as a method of an object, this will refer to that object.
Otherwise, the function is being invoked as a standalone function not attached to any object, and this will refer to the global object.
Example 2.41: A function invoked using Function.call

01
var myObject = {
02
    sayHello : function() {
03
        console.log('Hi! My name is ' + this.myName);
04
    },
05
 
06
    myName : 'Rebecca'
07
};
08
 
09
var secondObject = {
10
    myName : 'Colin'
11
};
12
 
13
myObject.sayHello();                  // logs 'Hi! My name is Rebecca'
14
myObject.sayHello.call(secondObject); // logs 'Hi! My name is Colin'
Example 2.42: A function created using Function.bind

01
var myName = 'the global object',
02
 
03
    sayHello = function () {
04
        console.log('Hi! My name is ' + this.myName);
05
    },
06
 
07
    myObject = {
08
        myName : 'Rebecca'
09
    };
10
 
11
var myObjectHello = sayHello.bind(myObject);
12
 
13
sayHello();       // logs 'Hi! My name is the global object'
14
myObjectHello();  // logs 'Hi! My name is Rebecca'
Example 2.43: A function being attached to an object at runtime

01
var myName = 'the global object',
02
 
03
    sayHello = function() {
04
        console.log('Hi! My name is ' + this.myName);
05
    },
06
 
07
    myObject = {
08
        myName : 'Rebecca'
09
    },
10
 
11
    secondObject = {
12
        myName : 'Colin'
13
    };
14
 
15
myObject.sayHello = sayHello;
16
secondObject.sayHello = sayHello;
17
 
18
sayHello();               // logs 'Hi! My name is the global object'
19
myObject.sayHello();      // logs 'Hi! My name is Rebecca'
20
secondObject.sayHello();  // logs 'Hi! My name is Colin'
Note
When invoking a function deep within a long namespace, it is often tempting to reduce the amount of code you need to type by storing a reference to the actual function as a single, shorter variable. It is important not to do this with instance methods as this will cause the value of this within the function to change, leading to incorrect code operation. For instance:

01
var myNamespace = {
02
    myObject : {
03
        sayHello : function() {
04
            console.log('Hi! My name is ' + this.myName);
05
        },
06
 
07
        myName : 'Rebecca'
08
    }
09
};
10
 
11
var hello = myNamespace.myObject.sayHello;
12
 
13
hello();  // logs 'Hi! My name is undefined'
You can, however, safely reduce everything up to the object on which the method is invoked:

01
var myNamespace = {
02
    myObject : {
03
        sayHello : function() {
04
            console.log('Hi! My name is ' + this.myName);
05
        },
06
 
07
        myName : 'Rebecca'
08
    }
09
};
10
 
11
var obj = myNamespace.myObject;
12
 
13
obj.sayHello();  // logs 'Hi! My name is Rebecca'
Scope
"Scope" refers to the variables that are available to a piece of code at a given time. A lack of understanding of scope can lead to frustrating debugging experiences.

When a variable is declared inside of a function using the var keyword, it is only available to code inside of that function — code outside of that function cannot access the variable. On the other hand, functions defined inside that function will have access to to the declared variable.

Furthermore, variables that are declared inside a function without the var keyword are not local to the function — JavaScript will traverse the scope chain all the way up to the window scope to find where the variable was previously defined. If the variable wasn't previously defined, it will be defined in the global scope, which can have extremely unexpected consequences;

Example 2.44: Functions have access to variables defined in the same scope

1
var foo = 'hello';
2
 
3
var sayHello = function() {
4
    console.log(foo);
5
};
6
 
7
sayHello();         // logs 'hello'
8
console.log(foo);   // also logs 'hello'
Example 2.45: Code outside the scope in which a variable was defined does not have access to the variable

1
var sayHello = function() {
2
    var foo = 'hello';
3
    console.log(foo);
4
};
5
 
6
sayHello();         // logs 'hello'
7
console.log(foo);   // doesn't log anything
Example 2.46: Variables with the same name can exist in different scopes with different values

1
var foo = 'world';
2
 
3
var sayHello = function() {
4
    var foo = 'hello';
5
    console.log(foo);
6
};
7
 
8
sayHello();         // logs 'hello'
9
console.log(foo);   // logs 'world'
Example 2.47: Functions can "see" changes in variable values after the function is defined

01
var myFunction = function() {
02
    var foo = 'hello';
03
 
04
    var myFn = function() {
05
        console.log(foo);
06
    };
07
 
08
    foo = 'world';
09
 
10
    return myFn;
11
};
12
 
13
var f = myFunction();
14
f();  // logs 'world' -- uh oh
Example 2.48: Scope insanity

01
// a self-executing anonymous function
02
(function() {
03
    var baz = 1;
04
    var bim = function() { alert(baz); };
05
    bar = function() { alert(baz); };
06
})();
07
 
08
console.log(baz);  // baz is not defined outside of the function
09
 
10
bar();  // bar is defined outside of the anonymous function
11
        // because it wasn't declared with var; furthermore,
12
        // because it was defined in the same scope as baz,
13
        // it has access to baz even though other code
14
        // outside of the function does not
15
 
16
bim();  // bim is not defined outside of the anonymous function,
17
        // so this will result in an error
Closures
Closures are an extension of the concept of scope — functions have access to variables that were available in the scope where the function was created. If that’s confusing, don’t worry: closures are generally best understood by example.

In “Functions can "see" changes in variable values after the function is defined”, we saw how functions have access to changing variable values. The same sort of behavior exists with functions defined within loops — the function "sees" the change in the variable's value even after the function is defined, resulting in all clicks alerting 5.

Example 2.49: How to lock in the value of i?

1
/* this won't behave as we want it to; */
2
/* every click will alert 5 */
3
for (var i=0; i<5; i++) {
4
    $('<p>click me</p>').appendTo('body').click(function() {
5
        alert(i);
6
    });
7
}
Example 2.50: Locking in the value of i with a closure

1
/* fix: “close” the value of i inside createFunction, so it won't change */
2
var createFunction = function(i) {
3
    return function() { alert(i); };
4
};
5
 
6
for (var i=0; i<5; i++) {
7
    $('<p>click me</p>').appendTo('body').click(createFunction(i));
8
}
Closures can also be used to resolve issues with the this keyword, which is unique to each scope:

Example 2.51: Using a closure to access inner and outer object instances simultaneously

01
var outerObj = {
02
    myName : 'outer',
03
    outerFunction : function () {
04
 
05
        // provide a reference to outerObj through innerFunction's closure
06
        var self = this;
07
 
08
        var innerObj = {
09
            myName : 'inner',
10
            innerFunction : function () {
11
                console.log(self.myName, this.myName); // logs 'outer inner'
12
            }
13
        };
14
 
15
        innerObj.innerFunction();
16
 
17
        console.log(this.myName); // logs 'outer'
18
    }
19
};
20
 
21
outerObj.outerFunction();
This mechanism can be particularly useful when dealing with callbacks, though in those cases, it is often better to use Function.bind, which will avoid any overhead associated with scope traversal.