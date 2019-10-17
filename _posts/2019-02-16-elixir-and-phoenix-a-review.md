---
layout: post
date: 2019-02-16 03:58:08 GMT
tags:
- elixir
- phoenix
- programming
- review
title: "Elixir and Phoenix – A Review"
---
So I've been spending some time recently learning about Elixir and Phoenix. I thought I'd share my experiences here, maybe someone will find it useful. If you've never heard of it, Elixir is a new(-ish) programming language that borrows a lot of syntax from Ruby. However, even though it looks quite similar on the surface, it's really a very different language.

<!-- more -->

Ruby is a object oriented, multi-paradigm language (meaning it supports imperative and functional programming styles). It's also interpreted, meaning that there is no compilation step. This is what makes many of the advanced features in Ruby work, but it's also the reason why it is rather slow compared to languages like Java or C, which involve a compilation step.

Elixir is similar to Java, in the sense that there is a compiler, but instead of machine code, it produces byte code that runs on a virtual machine. In the case of Elixir, this virtual machine is [Erlang][erlang], a language created over 30 years ago by the Swedish telecom company Ericsson for use in their telecommunications equipment.

It turns out that Erlang has many features that are very desirable for building web applications, such as high performance, fault tolerance, and the ability to support many millions of simultaneous connections. It also has very good support for multi-core CPUs, something that is more and more important these days, now that even the smartphone in your pocket likely has multiple cores. However, Erlang's syntax is quite dated, so Elixir aims to bring Erlang into the 21st century. 

Phoenix on the other hand is a web framework for Elixir. It's heavily inspired by Ruby on Rails and indeed shares many of the same features. However, it is generally at least an order of magnitude faster. Instead of taking several milliseconds, it can answer most requests in a matter of microseconds. 

There is a cost, however, and that's learning a new programming paradigm. Elixir is a [**functional** programming language][fp], meaning that there are no objects, only functions. Furthermore, functions have to be "pure" in the mathematical sense, that is, they can have no side effects.

All data in Elixir is **immutable** by default, meaning it cannot be changed. If you want to make changes to a variable, you need to return a new copy that includes the changes. If this sounds expensive to you, then keep in mind that the developers thought of this and heavily optimized the compiler for this use case, so in practice, this is much faster that you'd think. Behind the scenes, the language uses many tricks to make this incredibly cheap. For example, inserting an element at the beginning of a [linked list][linked-list] does not need to copy the entire list – since the tail of the list is guaranteed to never change, the new list can be constructed by simply making a new head and linking it to the old list.

The advantage of having immutable data is that it allows for easy parallelization. The biggest problem that languages like Ruby have with parallelization is when data needs to be shared between two threads. If two separate threads want to access the same data (say, one wants to read and another wants to write), that creates a conflict. They have to synchronize somehow, so that one thread has to wait until the other is done before accessing the shared data. This can lead to many difficult-to-resolve bugs.

In Elixir, everything is immutable, so there is no problem sharing data between threads. If one thread wants to make changes, it automatically gets its own copy of the data. 

While it takes a bit of practice, but after a while it's not that difficult thinking only in functions. A web server is really just a collection of functions (AWS lambda makes that pretty clear). A request, [consisting of HTTP method, URL, headers and body (for POST/PUT/PATCH)][http-msg] goes in, HTML comes out. Since HTTP is by definition a [stateless protocol][stateless], the functional paradigm is a perfect match for this use case.

Of course, in a web app, it usually takes many steps to produce the desired response. Phoenix gives you the tools to compose your functions out of many smaller functions, and it also provides a convention to help organize them in a sensible way. 

Similar to Rails, Phoenix uses the [MVC (Model, View, Controller) pattern][mvc], but it introduces a few more layers to keep things neat. In Phoenix, every controller has a single view module, which in turn can have several templates. Any helper functions that are used in the view (which go into a helper module in Rails) go into the view module, assuring that they are only available to the templates that need them, not application-wide like in Rails.

Phoenix also adds a new layer of abstraction between controllers and models (called a “context”), which absorbs all the database queries. This fixes a big problem with Rails, where models often grow tremendously large because they have to deal with both business logic **and** database logic. Phoenix draws a clear line, separating the business logic from the database code.

Another interesting aspect of Phoenix is the idea of pipelines, which fixes another big problem of Rails: middleware. If you've ever worked on a large Rails app, it can be quite difficult figuring out exactly what filters are run for a request and in what order. Even more so if you have an application that has both an HTML frontend and an API — In this case, you often want a different set of filters to apply for API requests than you have for HTML requests. Phoenix solves this by letting you combine all the filters (which it calls "plugs") into a pipeline and then giving you a way to decide which pipeline a request should be routed through. If it's a browser request, you send it through the `:browser` pipeline. If it’s an API request, you send it through the `:api` pipeline. Neat.

All in all, it makes for quite an interesting framework. While I wouldn't exactly recommend it to someone brand new to web programming (Rails is much more beginner friendly than Phoenix), it's easy enough to pick up for someone with a few years of Rails on their back. It does take some getting used to, however – especially Ecto, the persistence layer, feels rather clunky compared to elegance of Rails's ActiveRecord (although it is no less powerful). However, the enormous performance gain might well be worth making the switch for. A single app server running Elixir and Phoenix can easily replace 10 Rails servers (and probably have room left to grow). I'm looking forward to learn more in the future.

[erlang]: https://en.wikipedia.org/wiki/Erlang_(programming_language)
[elixir]: https://elixir-lang.org/
[mvc]: https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller
[http-msg]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Message_format
[stateless]: https://en.wikipedia.org/wiki/Stateless_protocol
[fp]: https://en.wikipedia.org/wiki/Functional_programming
[linked-list]: https://en.wikipedia.org/wiki/Linked_list