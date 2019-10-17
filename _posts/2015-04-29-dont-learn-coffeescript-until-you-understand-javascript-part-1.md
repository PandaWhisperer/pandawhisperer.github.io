---
layout: post
date: 2015-04-29 18:38:40 GMT
tags:
- coffeescript
- javascript
- programming
- coding
- learning
title: "Don't Learn CoffeeScript Until You Understand JavaScript (Part 1)"
---
When I first found out about CoffeeScript a few years ago, I was immediately hooked. Here was a language that looked a lot like Ruby, in fact, it borrowed almost all the syntax from Ruby, and mixed in a little bit of Python just for aesthetic effect. It promised to get rid of the most annoying bugs and wrinkles in JavaScript, while making it look like a language I already knew and loved. It had a sane syntax for defining classes, array comprehensions, and a variety of other goodies I had come to love. THIS was going to be the end of my shitty JavaScript days. 

I couldn't have been more wrong.

<!-- more -->

Let me set the scene for you: at that point in my life, I was working as a Rails engineer in a very corporate setting, but ironically, most of my day-to-day work was actually spent writing JavaScript. By some strange twist of fate, even though I was hired because I knew Ruby, the majority of the tickets I was assigned where 100%  frontend. Which meant JavaScript. Which I was in no way or shape qualified to do. It's not that I didn't *know* JavaScript — technically, it was the language I had the most experience in, seeing as I had made a switch from Java to Ruby midway through my career. 

I just didn't know how to write JavaScript well.

Consequently, the front end code I was banging out quickly turned into a hot mess. It all had started so innocently: just a little [jQuery][jQ] typeahead over here, a little [jQuery UI][jQUI] calendar over there. An action button that validated the form input and sent it to the server for further processing. Nothing fancy. But fast forward about eight months, and I was dealing with over 300 lines of spaghetti code, everything tightly coupled, and if you changed something on one end, it would break on another. 

Unfortunately, due to management somehow consistently firing or losing the most competent senior engineers, there was no one around that I could ask for guidance, either. So I had to help myself. And as it frequently happens when you desperately need help, but you're not sure what KIND of help, I found the wrong help. It went by the name of CoffeeScript. 

When I first clicked the link that lead to the [CoffeeScript website][coffee], my tired eyes were lighting up. THIS I could do. THIS was going to be the solution. CoffeeScript would save my ass, by allowing me to write code in a way I already knew, without having to waste hours of my day figuring out which of JavaScript's many bugs I just just ran into.

![CoffeeScript is the solution](images/posts/1c930ff6683b743963ec9637e28655110f80dc79c4c06621834ec8ed26fad952.jpg)

I would just rewrite my 300 lines of spaghetti code in CoffeeScript, and because I no longer had to worry about all the nasty JavaScript bugs, it was going to work SO much better and be SO much more maintainable. 

Yeah right.

After spending a few weeks trying to re-code the entire front-end code in CoffeeScript, I came to a very sobering conclusion. My 300+ line JavaScript mess was now a 200 line CoffeeScript mess. It was still tightly coupled. It was still breaking as soon as I started making modifications. And it still barely made sense, even to me. Mostly, it was just JavaScript, but with `function` replaced by pointy arrows. Because that was the only way I had gotten it to work with the other libraries I was using.

It slowly dawned on me that CoffeeScript is, in the end, still "just JavaScript". It's just syntactic sugar, designed to work around the most egregious flaws in the language. All the fancy constructs, like classes and array comprehensions — at the end of the day, they just mapped to equivalent JavaScript code. And I realized that if I wanted my CoffeeScript code to cleanly interface with other JavaScript code, I'd actually have to understand how it all worked under the hood. I'd have take apart the engine and see what made it work. And that meant having to re-learn JavaScript, from scratch.

![I don't know what I'm doing](images/posts/a2fdb6dcf365465d9b7b2466c4fcbb31bf90abcd17c061f80f54e4c28d9752cf.jpg)

In the next part, I'm going to explain how I went about re-learning JavaScript, the resources I used, and the key insights I got. 

[coffee]: http://www.coffeescript.org
[jQ]: http://jquery.com
[jQUI]: http://jqueryui.com