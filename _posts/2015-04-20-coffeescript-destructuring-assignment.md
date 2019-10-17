---
layout: post
date: 2015-04-20 16:55:04 GMT
tags:
- coffeescript
- javascript
- es6
- tips
- programming
- coding
title: "CoffeeScript: destructuring assignment"
---
In CoffeeScript (and [JavaScript as of ES6][es6ds]), you can do a bunch of neat things with the assignment operator that will lead to cleaner, more concise code.

For instance: 

    array = ["one", "two", "three"] 
    [one, two, three] = array
    
    console.log two # prints "two"
    
<!-- more -->

In other words, the compiler picks apart the array on the right and matches it with the array expression on the left, assigning each variable with the corresponding match from the array.

Don't need all the elements from the array? You can combine this with the [splat operator][splat], too:

    array = ["all", "the", "funny", "things"]
    [first, second, rest...] = array
    
    console.log first  # prints "all"
    console.log second # prints "the"
    console.log rest   # prints "[ 'funny', 'things' ]"
    
Want more? This also works for objects:

    person =
      name: "Albert Einstein"
      occupation: "Physicist"
      birthdate:
        year: 1879
        month: 3
        day: 14
        
    {name, occupation} = person
    
    console.log "#{name}, #{occupation}" 
    # prints "Albert Einstein, Physicist"
    
To access field nested deeper inside the object, all you have to do is mimic the object structure:

    {birthdate: {year, month, day}} = person
    
    console.log "Born: #{month}/#{day}/#{year}"
    # prints "Born: 3/14/1879"
    
You can also combine array- and object destructuring.

Now, what is this good for in practice? For instance, you can use it simulate [named parameters][np]:

    drawText = (text, options) ->
      {font, style, size} = options
      # ... implementation ...
      
    drawText "Hello", font: "Comic Sans", style: "bold", size: 24

Or, even shorter, putting the destructuring right into the parameter list:

    drawText = (text, {font, style, size}) -> ...
    
This even works with instance variables:

    class Person
      constructor: (options) ->
        {@name, @age, @city} = options
        
    tim = new Person name: "Tim", age: 24, city: "Los Angeles, CA"
    
Or even:

    class Person
      constructor: ({@name, @age, @city}) ->
        # nothing to do here
        
    tim = new Person name: "Tim", age: 24, city: "Los Angeles, CA"
    
Another favorite use case of mine is for Node.js imports:

    {get} = require "http"
    {writeFile, createWriteStream} = require "fs"
    
    output = createWriteStream "output.txt"
    
    request = get "http://www.google.com", (res) ->
      writeFile "headers.json", JSON.stringify res.headers
      res.pipe output
      res.on 'finish', -> output.close()

This little script fetches a URL (in this case, the Google homepage) and writes the headers to a JSON file, and the body to a text file.

That's all, folks. Happy CoffeeScripting!

**UPDATE**: Pavel makes a great point in the comments: using destructuring assignment in parameter lists can cause problems if the parameter is optional. Specifically:

    drawText = (text, {font, style, size}) -> #...

    drawText "foo"

Results in 

    TypeError: cannot read property "font" of undefined
    
Fortunately, this can be fixed very easily using default parameter values:

    drawText = (text, {font, style, size}={}) -> #...
    
Prevents this problem (and leaves all optional parameters `undefined`). Alternatively, you can spell out specific defaults there as well:

    drawText = (text, {font, style, size} = 
               {font: "Helvetica", style: "normal", size: 24}) ->
               
Thanks for noticing.

[es6ds]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
[splat]: http://coffeescript.org/#splats
[np]: http://en.wikipedia.org/wiki/Named_parameter