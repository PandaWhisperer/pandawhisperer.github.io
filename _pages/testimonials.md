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

  - author: Lynn Chang
    title: Senior UI Designer
    image: /images/testimonials/lynn_chang.jpg
    link: https://www.linkedin.com/in/yellowsubmarine/
    quote: >
      It was a pleasure to work with Chris at Oversee. I know Chris as hard-working and very dedicated person focused on goal and quality. He is also someone that is smart and knowledgeable, he never hesitate to share what he knows and learned. Besides, he is also someone that is easy and fun to work with.

  - author: Kristin Heininger
    title: Ruby on Rails Developer
    image: /images/testimonials/kristin_heininger.jpg
    link: https://www.linkedin.com/in/kristin-heininger-a46962139/
    quote: >
      Chris was my mentor while I was enrolled at the Firehose Project, an online code school for Ruby on Rails. He was always supportive, and still is even though I’m no longer enrolled in the program. Chris tailored things to my needs. I finished the curriculum early, and he would give me more challenges so that I could continue learning. He helped me better understand the nuances of Ruby and JavaScript, and would show me how I could improve my code even after I had a working solution by improving the readability or efficiency of my algorithms. Chris encouraged me to continue problem-solving, ask questions and not be afraid to make mistakes, arguably the most important qualities in a software engineer. He would make a great addition to any team because he not only has a wide range of experiences as a developer, but he's willing to share it to help others in their professional development.
---
{% for testimonial in page.testimonials %}
> &ldquo;{{ testimonial.quote | strip }}&rdquo;

<div class="author">
  <img src="{{ testimonial.image }}" alt="{{ testimonial.author }}"/>
</div>

### [{{ testimonial.author }}]({{ testimonial.link }})

{{ testimonial.title }}
{% endfor %}
