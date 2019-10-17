---
layout: post
date: 2015-05-05 22:28:16 GMT
tags:
- javascript
- coffescript
- inheritance
- classes
- prototype
- coding
- programming
title: "Don’t Learn CoffeeScript Until You Understand JavaScript (Part 2)"
---
In the [last part][part1], I talked about my motivation for re-learning JavaScript. In this part, I'll tell you how I went about it. The third part will be about how being better at JavaScript will make you better at CoffeeScript as well.

After the sobering conclusion that I didn't really know how to write JavaScript very well, I decided to fix this once and for all. It just seemed to make sense. Web application had (and still have) been consistently getting more front-end heavy, and after serving as a sideshow for almost 15 years, JavaScript was getting ready for prime time. *Everyone* was investing in it, and Google was leading the charge. But where to start?

<!-- more -->

"Let's ask Google," I thought. They should know. 

Well, I don't remember exactly what I typed into Google, but I do remember the most important resources I found. The first was [JavaScript Garden][jsg]. After having struggled for *months* tripping over each and every of JavaScript's weird idiosyncracies, each of them often costing me a few hours, or sometimes up to a day of coding, I finally had a complete compendium of ALL the bugs and wrinkles in JavaScript, and how to work around them. This resource should definitely be in every JavaScript coder's bookmarks. It also helped me to finally disabuse myself of the notion that I actually *understood* JavaScript in some way, just because I had been a Java programmer for two and a half years, and JavaScript's syntax is similar to Java's. This was my extraction from the Matrix of false belief. 

Now, the next step was finding a resource to help me re-learn JavaScript, from the ground up. My [Nebuchadnezzar][matrix], if you will. This came in the form of the excellent (and free!) [Eloquent JavaScript][ejs] by Marijn Haverbeke. This wonderful book teaches JavaScript as a first programming language, and it does so with impeccable style and attention to detail. Especially [chapter 6][ejsch6] was extremely helpful in overcoming my preconceived notions about how JavaScript's object orientation *should* work, and instead understand how it actually *does* work.

<figure>
![](images/posts/7a1bd9311027f111f4eb0f44664f4c0ba4c4c5d31d3fa4bf277eca7b515126e4.jpg)
<figcaption>The Nebuchadnezzar</figcaption>
</figure>

I'm not going to lie to you though — this chapter is still rather dense, and you might need to read it and re-read it a couple of times. In order to better process what I learned and deepen my own understanding, I then signed up to give a [talk about the JavaScript object model][jsyuno] at one of the weekly tech talks held at our company. 

Here's the [TL;DR][tldr]: JavaScript features *prototypal* inheritance, which is in stark contrast to the classic *class-based* inheritance used by almost all other object oriented languages. In prototype-based inheritance, there are no classes. Instead, there are only objects (what is called *instances* in other languages). Anytime you write `{}` in JavaScript, you've created an object. Since functions are values, you can outfit your object with *methods* simply by attaching function-valued properties. If a function is called *via an object* , the `this` keyword *inside* the function body will be bound to the object the function was called on:

    var person = {name: "Simon"};
    person.speak = function (line) {
      console.log(this.name + " says '" + line + "'");
    }
    person.speak("Jump in the air!");
    // → Simon says 'Jump in the air!'

## Objects, Prototypes, Oh My...

Now, how does inheritance work without classes? Simple: it works just like in a real world. Each object has an ancestor, the so-called *prototype*. By default, if an object's prototype isn't otherwise specified, it will be [`Object.prototype`][obj.proto]. This default prototype contains methods that are common to *all* JavaScript objects, such as the well-known `.toString()`, or [`.hasOwnProperty()`][obj.hop]. Anytime you create an object using the *literal notation* `{...}`, its prototype is going to be `Object.prototype`. 

One way to imagine this is to literally visualize objects being people, and the prototype being their parent (since objects don't reproduce sexually, they'll only have one parent instead of two). An object inherits all of its parent's properties, but has the option to modify (i.e. override) them. 

<figure>
![](images/posts/dd53ed43f57e59a3713323cb86f2c9011642a0f6368d4172cc3337da10ffe0a6.jpg)
<figcaption>JavaScript: a class-less system</figcaption>
</figure>

Now, we know from experience that certain objects in JavaScript have additional methods, which are only available on objects of that type. For instance, arrays have a [`.forEach()`][arr.forEach] method, functions have [`.call()`][fun.call] and [`.apply()`][fun.apply], and strings have [`.toUpperCase()`][str.tou] and [`.toLowerCase()`][str.tol], among others. It would make sense then, that all arrays would share a common `Array` prototype, functions have a `Function` prototype, and strings have `String` prototype. And indeed that is the case (you can find the prototype of an object using [`Object.getPrototypeOf()`][obj.gpo]):

    Object.getPrototypeOf([])) == Array.prototype; // → true
    Object.getPrototypeOf(function() {})) == Function.prototype; // → true
    Object.getPrototypeOf('') == String.prototype;
    // → TypeError: Object.getPrototypeOf called on non-object
    // Oh JavaScript...

And all of those prototypes actually contain the methods we mentioned above:

    Array.prototype.hasOwnProperty('forEach'); // → true
    Function.prototype.hasOwnProperty('call'); // → true
    Function.prototype.hasOwnProperty('apply'); // → true
    String.prototype.hasOwnProperty('toUpperCase'); // → true
    String.prototype.hasOwnProperty('toLowerCase'); // → true

## Inheriting Prototypes

Now, inheritance traditionally works in multiple levels: Class A can inherit from class B which can inherit from class C, which makes all methods (and properties) of class C available in class A, as long as they aren't overridden somewhere along the line (either in class B or in class A itself). 

The same goes for prototypal inheritance. For example, an array's prototype is `Array.prototype`, which is an object. All objects, if not otherwise specified, have `Object.prototype` as their prototype, and `Array.prototype` is an object — therefore, its prototype must be `Object.prototype`:

    Object.getPrototypeOf(Array.prototype) == Object.prototype; // → true
    
Okay. I know what you're thinking. "Does it *have* to be `Object.prototype`? Couldn't it be something else?"

Yes, it could. There could be another prototype nestled in between. And another, and another... before we finally get to `Object.prototype`. This is what's known as a *prototype chain*. And understanding the prototype chain is essential to solve the mystery of how JavaScript's classes work. This chain defines how JavaScript resolves properties on objects. 

For instance, an empty array `[]` has the following prototype chain:

    [] → Array.prototype → Object.prototype
    
Any property or method that we might call on an array has to be defined somewhere in that chain. Let's say we call `[].toString()`. JavaScript will first look in the array object itself, but it will come up empty. Next, it will start traversing the prototype chain, going one level up to the ancestor, `Array.prototype`. As it happens, `Array.prototype` contains a `.toString()` method (which thus overrides the default `.toString()` implementation from `Object.prototype`). The runtime found a match, so it will execute that function.

<figure>
![](images/posts/f7717e3296e2fd793b241076a861ac4a221b678e575757c1c552b7226d2b9fca.jpg)
<figcaption>Not that kind of prototype chain</figcaption>
</figure>

Let's say we try another method, one that is implemented on `Object.prototype`, but not `Array.prototype`. [`.hasOwnProperty()`][obj.hop] is such a method. In this case, the runtime would come up empty in `Array.prototype`, and traverse the chain another level up, looking in `Object.prototype`, where this method is defined. It finds a match there, and executes the function.

If the interpreter has finished traversing the entire prototype chain, but still comes up empty, it will consider that property to be `undefined`. Of course, if you tried to call it as a method (by providing an argument list), this will cause the well-known `TypeError: undefined is not a function`.

Before we move on, let's finish this off with an example showing how to set up a prototype chain yourself. We'll be creating some ancestors for Simon. Let's start with his grandpa:

    var simonsGrandpa = {name: "Simon's Grandpa"};
    simonsGrandpa.speak = function (line) {
        console.log(this.name + " says '" + line + "'");
    }
    simonsGrandpa.speak("How do you do?");
    // → Simon's Grandpa says 'How do you do?'
   
Next, we'll create his dad. You'll see that he inherits some of his behavior from Simon's grandpa, but he also learns a new trick:

    var simonsDad = Object.create(simonsGrandpa);
    simonsDad.name = "Simon's Dad";
    simonsDad.proclaim = function() {
        this.speak("But I would walk 500 miles");
    }
    
    simonsDad.speak("Fine and dandy.");
    // → Simon's Dad says 'Fine and dandy.'
    simonsDad.proclaim()
    // → Simon's Dad says 'But I would walk 500 miles'
    
Finally, here's Simon:

    var simon = Object.create(simonsDad);
    simon.name = "Simon";
    
Simon inherits everything from his dad and his granddad:

    simon.speak("Jump in the air!");
    // → Simon says 'Jump in the air!'
    simon.proclaim();
    // → Simon says 'But I would walk 500 miles'

Now, let's teach Simon something new, by modifying a behavior he learned from his dad. In order to do this, we need to access the version of `proclaim` from Simon's prototype:

    simon.proclaim = function() {
        Object.getPrototypeOf(this).proclaim();
        this.speak("And I would walk 500 more");
    }
    
    simon.proclaim()
    // → Simon's Dad says 'But I would walk 500 miles'
    // → Simon says 'And I would walk 500 more'
    
Whoops! What happened here? What's Simon's dad doing here? Well, remember that `this` in JavaScript is always set to the object a method was *invoked* on. In this case, that's Simon's `prototype`, i.e. his dad. In order to have that method work on Simon instead, we need to use [`.call()`][fun.call] or [`.apply()`][fun.apply], and specify the target object explicitly:

    simon.proclaim = function() {
        Object.getPrototypeOf(this).proclaim.apply(this);
        this.speak("And I would walk 500 more");
    }
    
    simon.proclaim()
    // → Simon says 'But I would walk 500 miles'
    // → Simon says 'And I would walk 500 more'
   
## Prototype, meet Constructor, Constructor, meet Prototype

The next and final piece to this puzzle is the constructor. If you have spent any time at all around JavaScript, you will have noticed that you can create objects of a specific type using the same notation that's used in other languages. For example, you can create an array using `new Array()`. That looks a lot like we're instantiating a class, doesn't it?

Yes. And no. Let me explain. What the `new` operator is doing is simply calling a function as a *constructor*. What happens behind the scenes is this: the runtime allocates a new, empty object, *setting its prototype to the constructor function's `.prototype` property*. Then, it invokes the constructor function, with `this` set to the newly created object. The constructor function can then make any modifications to the object that it wishes. The following examples illustrates that:

    function Person(name) {
      this.name = name;
    } 
    console.log(Person.prototype); // → '{}'
    
    Person.prototype.speak = function(line) {
      console.log(this.name + " says '" + line + "'");
    };
    
    var simon = new Person('Simon');
    simon.speak("Stick out your tongue!");
    // → Simon says 'Stick out your tongue!'

So there you have it: this is the essence of prototypal inheritance. In the next part, I'll talk about how being good a JavaScript makes you better at CoffeeScript. If you have any questions, comments, or suggestions, please share them below.

[part1]: http://bit.ly/coffee-js-pt1
[jsg]: http://bonsaiden.github.io/JavaScript-Garden/
[ejs]: http://eloquentjavascript.net
[ejsch6]: http://eloquentjavascript.net/06_object.html
[matrix]: http://en.wikipedia.org/wiki/The_Matrix#Plot
[jsyuno]: http://pandawhisperer.github.io/reveal.js/
[tldr]: http://en.wikipedia.org/wiki/TL;DR
[obj.proto]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype
[obj.gpo]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf
[obj.hop]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty
[fun.apply]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply
[fun.call]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call
[arr.forEach]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
[str.tol]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase
[str.tou]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase