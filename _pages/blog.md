---
layout: page
title: Blog
permalink: /blog
---
## Latest Posts

{% for post in site.posts %}
  {% if post.title != "Why I went to NeurIPS by train and why I will do it again" %}
    - ### [{{ post.title }}]({{ post.url }})
      {{ post.excerpt }}
  {% endif %}
{% endfor %}
