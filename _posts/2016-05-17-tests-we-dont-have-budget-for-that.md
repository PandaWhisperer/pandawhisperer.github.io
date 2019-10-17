---
layout: post
date: 2016-05-17 15:17:50 GMT
tags:
- coding
- development
- software
- software engineering
- tdd
title: "Tests? We Don't Have Budget for That"
---
The other week, someone forwarded me [this article](https://www.toptal.com/freelance/http-request-testing-a-survival-tool) on the [TopTal engineering blog](https://www.toptal.com/blog). It was an interesting read, if only for the warm, fuzzy feeling of familiarity that arises when you realize that even someone who works for a company that prides itself in [hiring only the top 3% of developers](https://www.toptal.com/developers) has to deal with the same issues as yourself.

> For my last few projects, I was forced to work without automated testing and honestly, it was embarrassing to have the client email me after a code push to say that the application was breaking in places where I hadn’t even touched the code.

<!-- more -->

Right there with you, pal. This is something pretty much every developer with a modicum of experience can sympathize with. I think it’s safe to say that if you’ve never run into this problem before, you either have never worked on real life project before, or you’re a certified genius on the level of Donald Knuth, who writes a mathematical proof that his code is error free. In other words, you’re a unicorn. Most likely though, it’s the former. 

### What’s The Issue?

So, why is it that even the top 3% of developers have to deal with this?

> Often, the client doesn’t really understand what testing is, or why it has value for the application. Clients tend to be more concerned with rapid product delivery and therefore see programmatic testing as counterproductive.

If that’s the case, it’s either the sales team’s fault for setting the wrong expectations, or it’s the developer’s fault for not explaining the value of tests properly. Either way, it’s a sales issue, not an engineering issue. 

> Or, it may be as simple as the client not having the budget to pay for the extra time needed to implement these tests.

In that case, maybe the client should be using off-the-shelf software instead of having a custom solution built for them. 

Let’s just imagine for a second the following situation: a client walks into a car dealer’s showroom, and talks to the nearest salesperson. The following conversation ensues:

Client: “I need a new car for my family. We just got our first child and I have to replace the convertible because only has two seats.”

Salesperson: “Well, you came to the right place. All our cars have a 5 star crash safety rating.”

Client: “Well, that’s neat, but they’re so expensive. Can’t I just buy a cheaper car without a crash test rating?"

That’s literally the equivalent of what the client is asking. If you wouldn’t get on an airplane without knowing that it has undergone extensive safety testing, then why would you run your business on an app that has no tests?

At this point, of course, the comparison starts getting a little wonky. Most cars are obviously mass-produced, and therefore the money required for testing is spread out over the entire production series. But let’s say you have the money to have a custom car built for you. Would you really insist that no testing be done to it whatsoever, because “there is no budget for that”?

Obviously, not all apps are the same. Maybe the client just needs a static website. Or a landing page for collecting emails. In that case, it probably doesn’t need a lot of tests. But in that case, why pay for custom development at all? Just have someone customize Wordpress for you and you’re done. However, if you’re having an app built that you plan to run your business on, then you absolutely, positively need tests. 

It’s really surprising to what degree people still believe that the rules of traditional engineering don’t apply to software engineering. As if somehow, by virtue of using a computer, the laws of the physical universe don’t apply anymore. They do. A computer is just a physical machine. It’s susceptible to influences from the physical world. Heck, the very act of programming it to do something _is_ an influence from the physical world. The physical world is non-deterministic and occasionally produces random events. Humans make mistakes. Are you willing to bet your business against that?

No? Well, then you DO have the budget for tests.