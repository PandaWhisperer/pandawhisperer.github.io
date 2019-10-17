---
layout: post
date: 2015-03-17 23:18:09 GMT
tags:
- software
- development
- coding
- javascript
- coffeescript
- http
title: "Photo post"
---
{% img images/posts/6f5cefa9b90f674d9fcd17fb6208c070f21e425b772a51cfb73079ca48b7c03e.jpg %}

# Panda Swagger

A main focus we have at [Panda Strike][ps] is not just doing the right things, but also doing things right. Especially when it comes to the basics. One of those things that are considered really basic these days is [HTTP][http]. The basic spec is almost 16 years old, but to this day, it remains at the foundation of the Internet as we know it. Since this isn't like to change very soon, we think that it makes sense to [learn how to use it correctly][http4], and based on that knowledge, build the tools that make the best practice and easy practice.

One of those tools is [shred][shred]. It's a simple library to build resource-oriented HTTP clients in JavaScript (or CoffeeScript), that conform to all RFCs and make it a breeze to access almost any API out there. So easy, in fact, that [Swagger][swag], a company who is in the business of *building* standards-compliant web APIs simple, [chose shred][swagshred] as a cornerstone of their own client library, [swagger-js][swagjs].

Welcome to the club!

[ps]: https://www.pandastrike.com
[http]: http://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
[shred]: https://github.com/pandastrike/shred
[swag]: http://swagger.io/
[swagjs]: https://github.com/swagger-api/swagger-js/
[swagshred]: https://github.com/swagger-api/swagger-js/blob/master/package.json#L21
[http1]: https://www.pandastrike.com/posts/20131211-http-made-simple
[http2]: https://www.pandastrike.com/posts/20140111-http-made-simple-part-2
[http3]: https://www.pandastrike.com/posts/20140128-http-made-simple-part-3
[http4]: https://www.pandastrike.com/posts/20140310-http-made-simple-part-4