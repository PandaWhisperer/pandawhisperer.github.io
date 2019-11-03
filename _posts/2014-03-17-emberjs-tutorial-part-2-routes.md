---
layout: post
date: 2014-03-17 23:45:00 GMT
tags:
- emberjs
- coding
- tutorial
- javascript
title: "Ember.js Tutorial, Part 2: Routes"
---
Alright, after [last week's][part1] post, we've gotten our application scaffold up and running, now it's time to write some code! 

First, we'll carve our name into the proverbial tree, by opening the generated `app/js/app.js` file and modifying it as follows: 

<!-- more -->

    var SpotifyExplorer = window.SpotifyExplorer = Ember.Application.create({
        appName: 'SpotifyExplorer',
    
        // Basic logging, e.g. "Transitioned into 'post'"
        LOG_TRANSITIONS: false, 
    
        // Extremely detailed logging, highlighting every internal
        // step made while transitioning into a route, including
        // `beforeModel`, `model`, and `afterModel` hooks, and
        // information about redirects and aborted transitions
        LOG_TRANSITIONS_INTERNAL: false
    });

    /* -- require()'s start here -- */

All we did was put our application name into the `appName` variable, and added some debug switches, which might come in handy later. Notice they're set to `false` right now – if you have issues with your routes later, you might want to set them to `true`, in which case Ember.js will write information about route transitions into your web brower's JavaScript console. More about that when we start talking about routes. 

I also noticed that Yeoman named my application namespace `Spotifyexplorer` instead of `SpotifyExplorer` (note the capital 'E'), which is of course purely cosmetic, but it sets off my OCD tendencies, so I went ahead and changed all references accordingly. 

Next, we'll start with configuring our routes. Ember.js has a [sophisticated routing mechanism](http://emberjs.com/guides/routing/) (quite obviously inspired by [Ruby on Rails](http://www.rubyonrails.org)), which maps URL paths to different pieces of our application. Since an Ember.js app is basically a single page (the location is never reloaded, instead, the DOM is modified dynamically), the *actual* URL never changes. Instead, what changes is the part of the URL *after* the `#` symbol, i.e. the *anchor*. In traditional HTML files, anchors are used to mark different sections in the document that can be [directly accessed via hyperlinks](http://htmldog.com/reference/htmltags/a/). Ember.js uses the same mechanism to jump to different parts, or *views* in our application, and the router is the mechanism that makes that happen.

Open the `app/js/router.js` file and paste in the following code:

    SpotifyExplorer.Router.map(function () {
        this.resource("search", { path: '/'}, function() {
            this.route("results", { path: '/results/:id' });
        });
    });

What happens here? We've defined two routes, which according to the Ember.js [Naming Conventions](http://emberjs.com/guides/concepts/naming-conventions/) will be named `SearchRoute` and `SearchResultsRoute`. The `SearchRoute` is mapped to the *root path*, `/`, while the `SearchResultsRoute` is mapped to the path `/results/:id`. Note that since the second route is *embedded* within the first, it inherits the first part of its name from the parent route.

The colon denotes a *dynamic segment*, that is, it serves simultaneously as a wildcard and a parameter holder. Any path starting with `#/results/` will invoke our `ResultsRoute`, and the part that follows will be passed to the route as a parameter (as [demonstrated in the Ember.js docs](http://emberjs.com/guides/concepts/naming-conventions/#toc_dynamic-segments)). 

Next, we'll create our routes. In Ember.js, a route is the first thing that gets invoked when a URL is loaded. It is responsible for loading the model and setting up the controller. Here's a good diagram showing how all the pieces fit together, courtesy of [Smashing Magazine](http://coding.smashingmagazine.com/2013/11/07/an-in-depth-introduction-to-ember-js/).

![Ember.js MVC Diagram](/images/posts/b416fae524cf2df796d519530a42bbb81641ff189d07f5daf0aa6e85663af56b.png)

We'll create our two routes as shown below. The `SearchRoute` goes into `app/js/routes/search_route.js`:

    SpotifyExplorer.SearchRoute = Ember.Route.extend({
        model: function() {
            return this.store.find('search');
        }
    });

All this route does is load a model named `Search` from a [Ember.Data](http://emberjs.com/api/data/) store. We'll discuss models in the next part of the tutorial, for now, all we need to know is that they're JavaScript objects that store data that is to be rendered by the view, and modified by the controller. 

Our second route, `SearchResultsRoute`, will be a bit more complicated, because it will interact with the Spotify API to load the results for the given search. Paste the code below into the `app/js/routes/search_results_route.js` file:

    SpotifyExplorer.SearchResultsRoute = Ember.Route.extend({
        model: function(params) {
            return this.store.find('search', params.id);
        },

        afterModel: function(search, transition) {
            var url = "https://ws.spotify.com/search/1/track.json?q=" + search.get('query');

            // query Spotify API and transform results
            Ember.$.getJSON(url).then(function(result) {
                // turn results into Ember objects
                var results = result.tracks.map(function(track) {
                    return SpotifyExplorer.Track.create(track);
                });
                // set as property on our model
                return search.set('results', results);
            });
        }
    });

Here, the `model()` function, instead of returning all models from the store, only loads one model, with an `id` that is passed through the `params`. In the `afterModel()` function (that is, as its name suggests, invoked automatically *after* the model is set), we perform an API call and in the callback, transform the results into Ember.js objects (instances of the `Track` model we will create in the next part). 

[part1]: http://www.pandawhisperer.net/post/112532248390/ember-js-tutorial-part-1