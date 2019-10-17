---
layout: post
date: 2015-05-25 04:19:18 GMT
tags:
- javascript
- coffeescript
- programming
- web programming
- meteor
title: "This Meteor has crashed"
---
So about a year ago I started working on a side project, because I was bored at work and I kept hearing about this new framework called [Meteor][1], which supposedly made building Single Page Apps stupidly simple. 

I started my app somewhere around version 0.5. Turns out it really was deceptively simple to get a working app, and I made progress very quickly. Over the summer and the fall, new versions of the framework kept getting released, but updating was usually a breeze. That is, until the big 1.0 rolled around. 

<!-- more -->

This one was a *bitch* to install. The entire packaging system had changed, from being an unofficial hack to being part of the platform. It took months until I finally found the time to complete the migration. 

Last weekend I took the project out again to do some work. But upon starting the server, it failed with the following error message:

    TypeError: Object # has no method 'host'
    
The error was not coming from any of my own code, rather, from somewhere [deep inside the framework][2]. Googling for the error message yielded only one result, a StackOverflow question with zero answers. 

At a loss, I went on IRC, where someone in `#meteor` suggested I try `meteor reset`. I did, but alas, it changed nothing. It also made me wonder what on earth this command does? Overall, Meteor is pretty opaque as it is, and this just added another notch to the tally. It seems the maintainers do not want to bother "ordinary" developers with the complexity of the internal workings of the magic, and instead prefer them to learn a variety of arcane incantations. 

### Update

Finally, in a last hope effort, I noticed that I had a `node_modules` directory in my Meteor project, which was left from some experiments I did with the [`json-server`][4] module. As soon as I deleted it, the server started up cleanly again. 

Of course, this makes some amount of sense, since Meteor's strategy is to bundle [any and all][5] JavaScript code it finds in your project, unless it's in the `server` directory. It's really a shame that the developers not only eschew the standard NPM packaging system, but haven't even thought about providing a safeguard to prevent accidents like this one. This certainly puts a dent into the comet's shiny exterior. 

[1]: https://www.meteor.com/
[2]: https://gist.github.com/PandaWhisperer/d6707300a496d996b4ec
[3]: http://stackoverflow.com/questions/29479882/meteor-app-fails-to-run-object-compiler-has-no-method-host/30427543#30427543
[4]: https://github.com/typicode/json-server
[5]: http://docs.meteor.com/#/basic/filestructure