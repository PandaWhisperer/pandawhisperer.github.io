---
layout: post
date: 2019-04-01 22:38:41 GMT
tags:
- javascript
- processing
- processing.js
- coding
- graphics
- algorithms
title: "Having Fun with Proccesing.js: Langton’s Ant"
---
The other day I solved an [interesting problem on CodeWars](https://www.codewars.com/kata/langtons-ant/), called [Langton’s Ant](https://en.wikipedia.org/wiki/Langton%27s_ant). It’s a type of [cellular automaton](https://en.wikipedia.org/wiki/Cellular_automaton?) that runs on a 2-dimensional infinite grid and moves according to two very simple rules:

1.  If the current field is colored white, make it black, turn 90 degrees to the right, and move one step forward.
2.  If the current field is colored black, make it white, turn 90 degrees to the left, and move one step forward.

While the CodeWars problem only required you to compute a few iterations of the ant’s movement and return the resulting state of the grid, I thought it would be interesting to make a visual version that runs in the browser.

<!-- more -->

After a bit of research, I finally settled on [Processing.js](http://processingjs.org/). Initially I had avoided it, because I did not want to learn yet another language, but after wasting a whole day trying to do pixel graphics with the [Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API), I figured I might as well give it a shot.

Turns out it’s a near perfect fit for this exercise, and not just because it provides a point primitive, which the Canvas API does not. Processing is a very simple, C-like language that is optimized for 2D graphics. The basic program structure is extremely simple: there is a `setup()` function and a `draw()` function. The former is called once at the start of your program, while the latter is called repeatedly in order to update the screen.

But enough of the theory, let’s see the result:

![image](/images/posts/e83b89be8db05cdb812a5e297dcab06a8a759332388acd534265579e8952e2aa.png)

[JS Bin on jsbin.com](https://jsbin.com/hifaqum/edit?js,output)

The interesting thing about Langton’s Ant is that despite its incredibly simple rules, it produces some rather interesting behavior. For a few minutes, it seems to run around the center aimlessly, until suddenly, it starts creating a sort of “highway” leading to the lower right hand corner.

If you leave it running long enough, it will eventually hit the edge of the screen and stop there, because I didn’t implement any overflow behavior, so the code will just crash at this point. One way to solve this would perhaps be by shifting the offset of the grid in the canvas, or scaling down the size of the pixels. I might try implementing that next time.

> **UPDATE:** The new version now automatically scales the resolution once the ant hits the borders of the screen. However, once the ant starts building its highway, it will inevitably run out of space eventually.   

The other interesting thing, from a computer science perspective, is the fact that the Ant works like a 2-dimensional [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine). That means that given the right input (i.e. a pre-populated grid), it can compute the solution to any algorithmic problem (if given enough time).

Another idea I’d like to pursue in the future is generalizing the ant to 3 dimensions, by giving it the ability to rotate around all 3 axes (and yes, that is indeed [the correct plural form of “axis”](https://www.quora.com/What-is-the-plural-form-of-axis-1)). This would mean I’d have to introduce more colors (two per axis). I’m curious to see if that same emergent behavior also works in 3D.
