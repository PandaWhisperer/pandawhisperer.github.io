---
layout: post
date: 2014-03-06 06:28:00 GMT
tags:
- coding
- emberjs
- tutorial
- javascript
title: "Ember.js Tutorial, Part 1"
---
So I just started a new job and one of the first things I had to learn was the&nbsp;<a href="http://emberjs.com" target="_blank">Ember.js</a>&nbsp;framework. After working through the <a href="http://emberjs.com/guides/getting-started/" target="_blank">Getting Started Guide</a>, I wanted to put my knowledge to the test and build my own app from scratch.

I decided that I wanted to do something similar in scope (no more than two controllers and one model), and something that would consume some public web API. After reading <a href="http://www.toptal.com/javascript/a-step-by-step-guide-to-building-your-first-ember-js-app" target="_blank">Balint Erdin's Tutorial</a> on Ember.js, I had an idea: wouldn't it be neat if this thing could hook up to the Spotify Web API? And that's how my idea was born.

<!-- more -->

**Ember.js for n00bs**

I've had almost 3 years of experience with Rails, and seeing that Yehuda Katz is one of the core members of the Ember.js team now, it comes to no surprise that both share a lot of similarities in concept and philosophy. However, this is JavaScript we're dealing with here, so the devil is, of course, in the details.

To get started, it helps having an application skeleton ready to go, so we don't have to do all the wiring by hand. Being spoiled with Rails' generators for years, I was pleased to learn that something similar exists for JavaScript: enter <a href="http://yeoman.io/" target="_blank">Yeoman</a>.

So, assuming you have a working installation of Node.js, go ahead and install Yeoman as follows:

    npm install -g yo

Since yeoman is a generator&nbsp;*framework*, not a generator by itself, we also need to install a <a href="https://github.com/yeoman/generator-ember" target="_blank">suitable generator</a> for our project. We do this with the following command:

    npm install -g generator-ember

Now we're ready to start some fire!

First, we need to get our app skeleton started:

    yo ember

This should generate the following directory tree for you:

    &#9500;&#9472;&#9472; app
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; bower_components
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; images
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; scripts
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; controllers
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; models
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; routes
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; views
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; styles
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; fonts
    &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; templates
    &#9492;&#9472;&#9472; test

Now before I start, I don't really like some of these names, specifically, `bower_components` and `scripts`. I prefer these to be named `libs` and `js`, respectively. Unfortunately this is not (yet?) configurable on the generator, so I manually renamed them and updated the appropriate references in `.bowerrc`, `Gruntfile.js` and `app/index.html`. The final directory layout should look like this:

    &#9500;&#9472;&#9472; app
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; images
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; js
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; controllers
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; models
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; routes
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; views
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; libs
    &#9474;&nbsp;&nbsp; &#9500;&#9472;&#9472; styles
    &#9474;&nbsp;&nbsp; &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; fonts
    &#9474;&nbsp;&nbsp; &#9492;&#9472;&#9472; templates
    &#9492;&#9472;&#9472; test

Now, to check that everything is working correctly, try running

    grunt&nbsp;serve

This should open a browser window and you should see the generator's default layout:

<figure class="tmblr-full" data-orig-height="365" data-orig-width="710" data-orig-src="https://raw.github.com/yeoman/generator-ember/master/project/img/screenshots/2013_07_17.png"><img src="images/posts/c07a90f13388a13bf609f53828d47f1a31b2fd4780b527946191245480eef391.png" alt="image" data-orig-height="365" data-orig-width="710" data-orig-src="https://raw.github.com/yeoman/generator-ember/master/project/img/screenshots/2013_07_17.png"></figure>

Check everything into git, and we're done with the first part!