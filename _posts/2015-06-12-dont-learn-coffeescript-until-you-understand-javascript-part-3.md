---
layout: post
date: 2015-06-12 17:19:18 GMT
tags:
- coffeescript
- javascript
- programming
- coding
- computer science
- web programming
- node.js
title: "Don’t Learn CoffeeScript Until You Understand JavaScript (Part 3)"
---
In the [last part][pt2] of this series, we've taken a deeper look at JavaScript's prototypal inheritance model, and how it differs from the "classic" OOP model championed by languages such as Ruby, Python, Java, and C++. In this part, we'll take a look at how this knowledge can make you better at writing CoffeeScript. You might also be interested in [part 1][pt1], which discusses my motivation for doing this.

[pt1]: http://bit.ly/coffee-js-pt1
[pt2]: http://bit.ly/coffee-js-pt2

<!-- more -->

CoffeeScript, while bearing many (intentional) similarities to JavaScript, is a complete programming language in its own right, and includes a full [self hosting][selfh] compiler, as is par for the course. However, that compiler, does not of emit machine code (such as C/C++) or bytecode (such as Java). Instead it produces plain, 100% human readable (though not optimized) JavaScript. Therefore, *anything* you can do in CoffeeScript, you can do in JavaScript. As the [CoffeeScript homepage][coffee] states:

The golden rule of CoffeeScript is: "It's just JavaScript"

[coffee]: http://coffeescript.org/
[selfh]: http://en.wikipedia.org/wiki/Self-hosting

You *could* compare CoffeeScript with a preprocessor, but that would not be entirely correct. A preprocessor can only *add* things to the language (like macros), but CoffeeScript *removes* a significant amount as well — such as semicolons, curly braces, the [`===` operator][csop], just to name a few. Basically,  the goal of CoffeeScript is to fix JavaScript's most egregious bugs and design flaws, and expose the simple and beautiful core underneath it.

[csop]: http://coffeescript.org/#operators

## A Better JavaScript

The most distinctive syntactic differences to JavaScript are immediately obvious: semicolons are not required (and not [inserted automatically][js-semi] in surprising ways either). 
Curly braces are (mostly) unnecessary, too. CoffeeScript instead requires you to indent your code properly in order to create blocks, [just as in Python][py-indent]. 
Finally, parentheses for function calls are optional as well, at least as long as there is at least one argument. Argument-less function calls still require parentheses, however (so they can be distinguished from accessing the function itself). Finally, function literals are written using a single (`->`) or double arrow (`=>`) to separate the parameter list from the function body. All in all, CoffeeScript looks like a mixture of Ruby and Python, except for the somewhat unusual (but elegant) function syntax.

To anyone who has used CoffeeScript before, it comes as no surprise that the next iteration of JavaScript, ES6, [contains][es6arrows] [a lot][es6template] of [syntax improvements][es6destruct] that are [almost 100% identical][es6classes] to what CoffeeScript had for the last few years.

[js-semi]: http://bonsaiden.github.io/JavaScript-Garden/#core.semicolon
[py-indent]: http://en.wikipedia.org/wiki/Python_syntax_and_semantics#Indentation
[es6arrows]: https://github.com/lukehoban/es6features#arrows
[es6template]: https://github.com/lukehoban/es6features#template-strings
[es6destruct]: https://github.com/lukehoban/es6features#destructuring
[es6default]: https://github.com/lukehoban/es6features#default--rest--spread

## Okay, so how does knowing JavaScript make you better at CoffeeScript?

Since JavaScript is now [one of the world's most popular programming languages][langpop], there's a lot of code out there that's already written in it. A LOT. Therefore, chances are that if you're going to write *anything* in CoffeeScript, more often than not, you'll be either integrating with other JavaScript code, leveraging [existing libraries][jquery] [written in JavaScript][mocha], or (most likely) both. It makes sense, therefore, to at least familiarize yourself with some of the patterns that are common in JavaScript, and how CoffeeScript implements some of its "magic" features in plain old JS. Let's start, shall we?

[langpop]: http://langpop.com
[jquery]: http://jquery.com
[mocha]: http://mochajs.org

### Everything is an Expression

CoffeeScript implements Ruby's philosophy of "everything is an expression". So you can do stuff like this:

    over_18 = if user.age > 18 then false else true

This compiles to the following JavaScript:

    var over_18;
    over_18 = (function() {
      if (user.age < 18) {
        return false;
      } else {
        return true;
      }
    })();

Granted, an experienced JavaScript developer could have made this a one-liner, too:

    var over_18 = (user.age >= 18);

But it's not quite as readable, and might take most junior devs a few minutes and some head scratching before they get that the result is always a boolean. The CoffeeScript version is marginally longer, but MUCH easier to read.

By the way, you'll find this pattern extremely often in JavaScript code that was generated by the CoffeeScript compiler. CoffeeScript makes use of this *any time* it needs to turn something into an expression that's ordinarily a statement, for instance, the [`switch` statement][cs-switch].

[cs-switch]: http://coffeescript.org/#switch

### Keeping it Classy

Inheritance in JavaScript has [always been a bit messy][classpattern], so CoffeeScript decided to introduce some nice syntactic sugar for it (which then [promptly made it into the next release of JavaScript, ES6][es6classes]).

[classpattern]: http://arjanvandergaag.nl/blog/javascript-class-pattern.html
[es6classes]: https://github.com/lukehoban/es6features#classes

    class Animal
      constructor: (@name) ->
    
      move: (meters) ->
        alert @name + " moved #{meters}m."

This is a lot more straightforward and easier on the eye than the traditional JavaScript way of doing the same thing:

    function Animal(name) {
      this.name = name;
    }
    
    Animal.prototype.move = function(meters) {
      return alert(this.name + " moved " + meters + "m.");
    };

Of course, in order to really make use of this pattern, you need to also be able to recognize it in existing JavaScript code. In other words, when you see something like the second example, your brain should go "oh, that's a class in CoffeeScript, and and `move` is an instance method." Bonus points for [extending a JavaScript "class" using CoffeeScript][cs-extend-js].

[cs-extend-js]: http://spin.atomicobject.com/2011/05/06/using-backbone-js-with-coffeescript/

### Fat Arrows, Two Ways

The `this` keyword is probably one of the most misunderstood parts of JavaScript, yet one of the most frequently used ones at the same time. It really pays off to spend some time to get to know it better, including the tools you need to bend it to your will (call/apply and bind). I promise you'll have much fewer mystery bugs and your day will be a lot more productive.

Since manipulating the scope of `this` is such a frequent requirement, CoffeeScript brings some additional syntax to make things more pleasant. By using a "fat arrow" `=&gt;` instead of a regular one when defining a function, a function will [automatically inherit its parent's scope's `this`][cs-fat-arrow]. In other words, no more

    var self = this;
    $('#whatever').on('click', function(event) {
      self.setColor(event.target.value); // or something involving `this`
    });

[cs-fat-arrow]: http://coffeescript.org/#fat-arrow

However, things get a little confusing when using the fat arrow for defining *methods* on objects, as opposed to anonymous functions.

    foo =
      foo: 'bar'
      test: => console.log @foo

    class Foo
      foo: 'bar'
      test: => console.log @foo
    
    foo.test()
    test = (new Foo).test
    test()

This is compiled to the following JavaScript:

    var Foo, foo, test,
     bind = function(fn, me){ return function(){ return fn.apply(me, arguments); }; };

    foo = {
     foo: 'bar',
     test: (function(_this) {
       return function() {
         return console.log(_this.foo);
       };
     })(this)
    };

    Foo = (function() {
     function Foo() {
       this.test = bind(this.test, this);
     }

     Foo.prototype.foo = 'bar';

     Foo.prototype.test = function() {
       return console.log(this.foo);
     };

     return Foo;
    })();

    foo.test();
    test = (new Foo).test;
    test();

This code will produce the following output:

    undefined
    bar

Even though we used the fat arrow both times, *and* we called the first function *with* the proper object reference, and the second one without, only the latter did what we expected. Why is that?

If you look closely, you'll see what happened: in the first declaration, CoffeeScript still uses the `this` from the *surrounding* scope (which is, in this case, empty). In other words, the function is bound at the time it's declared. In the second part, however, the function is not bound until an *instance* of the class is created. Only when the constructor is executed, the function is bound to the current instance. Unfortunately, the CoffeeScript docs don't do a very good job at explaining this (in fact, it's not mentioned at all).

## Summary

CoffeeScript vastly reduces the amount of clutter in your code, and enforces good habits by requiring proper code indentation. However, since we can't live exclusively on CoffeeScript island, we have venture out into JavaScript land from time to time, and collaborate with its denizens. Therefore, knowing the local customs and idioms is extremely helpful, even if you don't plan on making your life there.