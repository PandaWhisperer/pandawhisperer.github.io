---
layout: post
date: 2014-05-28 22:56:00 GMT
tags:
- ui
- ux
- design
title: "UI design is HARD"
---
I'm certainly [not the first](http://blog.codinghorror.com/ui-is-hard/) to notice this, but just today I ran into an excellent example to illustrate this fact. 

In my day job at [GoodData](http://www.gooddata.com), one of the projects I've been working on is a [labs app](https://developer.gooddata.com/blog/introducing-gooddata-labs) called List Metrics. It looks something like this:

<!-- more -->

![Screenshot](/images/posts/07a19e4177fba2f373a548598bf45bba4d4585a6357d72c0143d337615225a45.png)

It shows all the [metrics](https://developer.gooddata.com/article/metrics-and-maql) in a selected project along with some metadata, and, most importantly, whether or not a metric is being used at all. This is important for our consultants because oftentimes, when working on a client project, they'll create a number of metrics for testing or validation purposes that never make it into the final project. Before we hand over a project to a client, we want to clean those up. And that's where it becomes very useful to know which of these metrics are unused and therefore safe to delete. 

So in the last column, you'll see a green dot when the metric is in use, and a red one when it isn't. When we first designed this, it just seemed obvious to do it that way. Green = yes, red = no.

Now last week, I gave a internal presentation of this tool (which was very well received), and we added an official [support forum](https://support.gooddata.com/entries/63616888-App-Metrics-Usage). And what do you think the first entry was?

It was about the choice of colors. Shouldn't it be "green = safe to delete" vs. "red = do not delete"? 

Well, he has a good point. Simply using colors is obviously not as intuitive as we originally thought. And this is before even considering the problem that some users might be colorblind. 

In a way, this reminds me of the classic optical illusion that I'm sure you have seen before. It's black and white picture in which some people tend to see the outline of a vase, and other people see the silhouettes of two people facing each other. If you look at it long enough, you can make your brain "switch" between seeing one or the other. 

Here's the picture I'm talking about: 

![](/images/posts/266fde3f9e5945d13376d0f200a45f73a0c4f39886dfc5e3f84560eb3d35890f.jpg)

So, then, how do we solve this problem? I came up with a number of different solutions:

  1. use words: instead of symbols, we could simply have the column say "yes" or "no". Perhaps still colored green and red, for easier scanning. The problem with that: yes and no don't make any sense by themselves, without knowing the question being answered.
  2. use distinct shapes: instead of circles for both, we can use use different symbols for each. A square and a triangle. Or a circle and a triangle. The problem with that? Squares and triangles don't have any inherent meaning, so that would be difficult to grasp intuitively.
  3. use symbols / icons: taking the last idea a step further, of course we could use different icons. The [Font Awesome project](http://fortawesome.github.io/) provides a [large number of icons](http://fortawesome.github.io/Font-Awesome/icons/) that can easily be used. We're already using it in the project (in fact, the circles are taken from there), so that would be easy to accomplish.

Long story short, we opted for option number 3. Now the statuses are discernible even without using the color information. However, it doesn't solve the "vase or two faces" problem. If you have a good solution for this issue, I'd love to hear it.