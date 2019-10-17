---
layout: post
date: 2016-04-14 22:51:44 GMT
title: "Last week I tried \"real\" TDD, and it was beautiful"
---
If you've been coding for more than a few weeks, I'm sure you've heard of Test-Driven Development, or TDD for short. The idea, of course, is deceptively simple: instead of writing code iteratively and testing it by hand, you write the test first, and *then* you write the code that makes it pass.

After discussing this principle with one of my mentees at [theFirehoseProject][tfhp], he sent me an article he found, penned by the venerable [Bob Martin][bmtw], in which he elucidates on the [The 3 Rules of TDD][3rtdd].  They are:

<!-- more -->

1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

I've been struggling with learning "real" TDD for a few years now. I chalk  up most of that struggle to bad habits, which have a tendency to expand themselves. Unless you make a real commitment to change, you somehow keep finding yourself in situations where everyone else has the same avoidance habits as you. 

> TDD? Yes, we do that. But we write the tests *afterwards*. At least usually. Well, sometimes. When we have time.  — Someone, at every company, every day

Now, we all know that "when we have time" in practice almost always means "never". And so, even after 5 years on the job, it's no surprise that when you find yourself thrown on the next project, even though the code is deployed to production, the tests either aren't working, hopelessly outdated, or completely non-existent. At best, there's some poor schmuck who gets paid to test the entire site by hand every time there's a deploy. True story.

Testing really is the ugly stepchild of the industry. Everyone just pays it lip service, but no one ever invites it to the party. And I'll be the first to admit that my previous encounters with TDD have often been all but fulfilling. In fact, they were mostly frustrating (I'm looking at you, [RSpec][rspec]). But after reading Bob Martin's 3 rules, and the advantages of following them, I couldn't help but feel a surge of motivation roll over me, followed by the desire to give it another try. Good thing I had just received a programming challenge in the mail for a job I was interviewing for. There wasn't going to be any better reason to try it than that, so I decided to give it go.

## Giving it a spin

So, with the 3 rules firmly in mind, I set out to finish this challenge. Without revealing too much, it was basically a simple JSON API server with 4 endpoints, 2 `POST` and 2 `GET`. There was one collection, a `POST` call to create a new element in it,  and a `GET` call to retrieve the entire collection (`POST /collection` and `GET /collection`, respectively). The other two calls were to retrieve summary statistics on the entire collection (`GET /collection/summary`) and to delete the entire collection (`POST /collection/clear`). The last one kind of broke the REST principle, but let's not be sticklers about that right now.

As Uncle Bob demanded, of course, I started with the tests. Well, I admit that I may have set up a very basic Express app skeleton first, but I definitely wrote the first test before writing any of the endpoints. Then, using [supertest][spt], I wrote a test for the first endpoint, `POST /collection`.  Reading right off the specification I got, I outlined the following test stubs:

    app = require '../src/app'
    request = require 'supertest'
    
    describe 'Collection API', ->
    
      describe 'POST /collection', ->
      describe 'GET /collection', ->
      describe 'GET /collection/summary', ->
      describe 'POST /collection/clear', ->

Of course, none of that will fail, so I added the following test case:

      describe 'POST /collection', ->
        it 'creates a new item', ->
          request(app)
            .post('/collection')
            .send()
            .expect(200)
            .expect('Content-Type', 'application/json')

Keeping in mind rule number 2, I stopped here, even though you may have noticed I'm not even POSTing any data yet. This was already sufficient to fail, because after all, my server didn't even know about the `/collection` endpoint yet. In fact, I realized, just writing `expect(200)` would have been enough, checking the content type should have come later. In fact, I realized at this point, all I should have written was this (let's ignore for a moment the fact that the status code should be 201 — I was just working off of the requirements here):

        it 'responds with status code 200', ->
          request(app)
            .post('/collection')
            .expect(200)

 And so I went on to rule number 3, and wrote exactly enough production code to make this test pass:

    app.post '/events', (request, response) ->
      response.json path: 'POST /events'

First round accomplished! That really wasn't so hard. 

I'll spare you the intimate details about the rest of the journey. I think these examples are enough to get the idea. After the realization that my first step was already too big, I tried really hard to pace myself on the next iterations, doing my best to literally only write enough of a test to make the production code fail. It's a strange feeling if you've been used to doing it differently. It somehow feels like your progress is much slower, because you're taking such small steps. However, a few hours later, it really started paying off: by this time, I would have normally gotten bored and take a long-ish break to read HackerNews or catch up on Facebook or some other nonsense, but instead, I was motivated to keep going, since my code was literally all working, and I could prove it, thanks to my growing number of tests.

By the end of the afternoon, I had the entire app working according to specs, and 33 unit tests to prove my work. I was feeling great! There was only one small thing: I hadn't even added a database yet, because I had no idea how to write a test for that. In fact, my server was storing everything sent to in an array in memory, because of rule #3 ("You are not allowed to write any more production code than is sufficient to pass the one failing unit test."). However, having a database backend was definitely one of the requirements.

## Now what?

I took some time that evening to think about whether it was possible to test for this, but couldn't come up with a good answer. Obviously, retaining state between tests is a no-no (that was how the requirement was worded: "we need a database to persist data between runs). 

I briefly considered writing a test that actually starts the server from the command line (in another thread), runs the first part of the suite, and then terminates the server without erasing data, only to start another instance, verify that the data is still there, and then delete it. But that seemed just a bit too involved. So at this point, I decided to cheat, and I simply added the database backend without having a failing test that I needed to make work. However, after that was done, I still had my suite of 33 tests that I could run against the server to make sure everything is still working.

Overall, this was a really fun experiment, and I think I gained a new appreciation for TDD. I'll certainly try this approach again for any new project that I'll be working on. Now if only the 3 rules could help with that one project that's already in production, but doesn't have any tests whatsoever...

[tfhp]: http://www.thefirehoseproject.com/
[bmtw]: https://twitter.com/unclebobmartin
[3rtdd]: http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd
[rspec]: http://www.rubyinside.com/dhh-offended-by-rspec-debate-4610.html
[spt]: https://github.com/visionmedia/supertest