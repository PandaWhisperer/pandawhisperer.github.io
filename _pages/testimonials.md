---
title: Testimonials
subtitle: What others are saying about me
featured_image: /images/featured/testimonials.jpg
testimonials:
  - author: Adam Stiles
    title: Software Developer, Entrepreneur
    image: /images/testimonials/adam_stiles.jpg
    link: https://www.linkedin.com/in/ajstiles/
    quote: >
      Christoph is a smart, focused engineer who produces high-quality, maintainable code. He’s a strategic, big-picture thinker with excellent communication skills. I was particularly impressed by his presentations on complicated topics to large groups of engineers. I definitely learned from him.

  - author: Sait Mesutcan Ilhaner
    title: Senior Software Engineer & Lifelong Learner
    image: /images/testimonials/sait_mesutcan_ilhaner.jpg
    link: https://www.linkedin.com/in/mesutcanilhaner/
    quote: >
      Not only is Christoph a Ruby Expert - that goes without saying - he is a great team leader. He takes the right risks, is willing to listen to other’s point of view, and encourages advancement and progress of new technologies. He really stays on top of where the Rails & JavaScript Community is going as well - and is a go-to guy if you need a suggestion on which tools are the right ones to use.It was an absolute pleasure working with him and I hope our paths cross again in the future.
---
{% for testimonial in page.testimonials %}
> &ldquo;{{ testimonial.quote | strip }}&rdquo;

<div class="author">
  <img src="{{ testimonial.image }}" alt="{{ testimonial.author }}"/>
</div>

### [{{ testimonial.author }}]({{ testimonial.link }})

{{ testimonial.title }}
{% endfor %}
