---
layout: post
date: 2016-05-31 17:05:10 GMT
tags:
- meteor
- javascript
- blaze
- spacebars
- templates
- tips
title: "Quick Meteor Tip: Using Inline Objects (and Arrays) in Blaze Templates"
---
The other day, I was working on my upcoming [Meteor Tutorial][meteor-yelp-clone], and I found myself wanting to pass a literal object to a template for testing purposes. This happens more often than you'd think — for instance, the excellent [`dburles:google-maps` package][meteor-google-maps] takes a single `options` parameter in order to support the [bewildering amount of configuration settings available through the Google Maps API][google-maps-options]:

    {{> googleMap name="map" options=mapOptions}}

The correct solution, of course, is to create a `mapOptions` helper on the template containing the map, which then return an object. Sometimes, however, you just want to test something real quick and would like to be able to pass a literal object right in the template.

<!-- more -->

If you follow your instinct, however, and write something like

    {{> template param={ foo: "bar", things: [1, 2, 3] } }}

You'll find out very quickly that [Spacebars doesn't support that kind of construct][spacebars-inline-objects]. However, after fooling around a bit with a [suggestion made in the comments][comment], I was able to figure out a workaround for this. I wrote two [global template helpers][meteor-template-helper] as follows:

    // This allows us to write inline objects in Blaze templates
    // like so: {{> template param=(object key="value") }}
    // => The template's data context will look like this:
    // { param: { key: "value" } }
    Template.registerHelper('object', function({ hash }) {
      return hash;
    });
    
    // This allows us to write inline arrays in Blaze templates
    // like so: {{> template param=(array 1 2 3) }}
    // => The template's data context will look like this:
    // { param: [1, 2, 3] }
    Template.registerHelper('array', function() {
      return Array.from(arguments).slice(0, arguments.length-1);
    });

With these, you can now pass a literal object to any Meteor template as follows:

    {{> template param=(object foo="bar") }}
    
Will result in the template's `param` looking like this:

    { foo: "bar" }

You can also pass literal arrays:

    {{> template things=(array 1 2 3) }}
    
This will result in the template's data looking like this:

    { things: [1, 2, 3] }

Both helpers can be combined:

    {{> template param=(object foo="bar" things=(array 1 2 3)) }}

Will result in the template's [data context][meteor-template-data] looking like this: 

    { param: { foo: "bar", things: [1, 2, 3] } }
    
If this was useful, please let me know in the comments below.

[meteor-google-maps]: https://github.com/dburles/meteor-google-maps
[meteor-yelp-clone]: https://github.com/PandaWhisperer/meteor-yelp-clone
[google-maps-options]: https://developers.google.com/maps/documentation/javascript/3.exp/reference#MapOptions
[spacebars-inline-objects]: https://github.com/meteor/meteor/issues/5095
[comment]: https://github.com/meteor/meteor/issues/5095#issuecomment-149053809
[meteor-template-helper]: http://docs.meteor.com/api/templates.html#Template-registerHelper
[meteor-template-data]: http://guide.meteor.com/blaze.html#data-contexts