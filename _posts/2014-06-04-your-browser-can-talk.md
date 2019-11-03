---
layout: post
date: 2014-06-04 00:52:00 GMT
tags:
- coding
- javascript
- html5
- api
- fun
title: "Your browser can talk!?!"
---
So I was debugging some code in Chrome's Web Inspector, and while looking at the [window object](https://developer.mozilla.org/en-US/docs/Web/API/Window) I came across an entry called `speechSynthesis`: 

![](/images/posts/e7de9e950f1e6546591bb4facfa44ed9deb6a1bada06ec76ac5d9e06218583d8.png)

What's that? I thought, and promptly googled it. Well, [turns out](http://updates.html5rocks.com/2014/01/Web-apps-that-talk---Introduction-to-the-Speech-Synthesis-API) that a real [text-to-speech (TTS) ](http://en.wikipedia.org/wiki/Speech_synthesis) API has somehow made it into HTML5, and it's already support it new(ish) versions of Chrome and Safari. 

<!-- more -->

So what can you do with it? First thing I noticed was that the API is a wee bit cumbersome, requiring you to instantiate a `SpeechSynthesisUtterance` object before you can do anything:

    var text = new SpeechSynthesisUtterance("Hello, good Sir");
    speechSynthesis.speak(text);

Second thing I noticed, there are different voices available! You can use this code to print a list of them:

    speechSynthesis.getVoices().forEach(function(voice) {
        console.log(voice.name);
    });

On my Macbook Pro with Chrome 35, I get the following list:

    Google US English
    Google UK English Male
    Google UK English Female
    Google Español
    Google Français
    Google Italiano
    Google Deutsch
    Google 日本人
    Google 한국의
    Google 中国的
    Alex
    Agnes
    Albert
    Bad News
    Bahh
    Bells
    Boing
    Bruce
    Bubbles
    Cellos
    Deranged
    Fred
    Good News
    Hysterical
    Junior
    Kathy
    Pipe Organ
    Princess
    Ralph
    Trinoids
    Vicki
    Victoria
    Whisper
    Zarvox

Looks like the first ten are supplied by Google, while the rest are from MacOS X's [built-in text-to-speech API](http://www.wikihow.com/Activate-Text-to-Speech-in-Mac-OSx). 

So now that we know that, why don't we have each voice introduce itself? Here's the code:

    speechSynthesis.getVoices().forEach(function(voice) { 
        var name = voice.name, 
            speech = new SpeechSynthesisUtterance("Hello, my name is " + name);
        speech.voice = voice;
        speechSynthesis.speak(speech);
    });

I don't know about you, but I nearly died from laughter while listening to some of them. The accent on the Spanish and Japanese voices is just so perfect. 

Finally, let's have some fun! We'll use this to make our browser ticklish. Try clicking 
<a href="javascript:(function()%7Bvar%20f%20%3D%20function()%20%7Bvar%20v%20%3D%20speechSynthesis.getVoices().filter(function(v)%20%7B%20return%20v.name%20%3D%3D%20'Hysterical'%3B%20%7D)%5B0%5D%2Cs%20%3D%20%5B%22ahahahaha%22%2C%20%22stop%20it%22%2C%20%22don't%20tickle%20me%22%5D%2Ct%20%3D%20new%20SpeechSynthesisUtterance(s%5B~~(Math.random()*s.length)%5D)%3Bt.voice%20%3D%20v%3B%20speechSynthesis.speak(t)%3B%7D%3BArray.prototype.slice.call(document.querySelectorAll('a')).forEach(function(a)%20%7Ba.addEventListener('mouseover'%2C%20f)%3B%7D)%7D)()">this link</a>
and then hover your mouse pointer over some links. Works best on a Mac, because the voice I'm using is probably not available on Windows.

[Here is the source](https://gist.github.com/cw4gn3r/c161fd368cfe7b206aa2) if you're interested. 

What would you build with this?

EDIT: User L0wkey on reddit [mentioned](http://www.reddit.com/r/javascript/comments/279s5s/your_browser_can_talk/chz9zwu?context=3) a great [CodePen](http://codepen.io/matt-west/pen/wGzuJ) that gives you a minimalistic UI to explore the capabilities of your browser's speech synthesis. Thanks!
