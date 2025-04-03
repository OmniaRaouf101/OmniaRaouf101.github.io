---
layout: page
title: Blog
permalink: /blog
---
## Latest Posts

{% assign posts = site.posts | slice: 1, site.posts.size %} 

{% for post in posts %}
  - ### [{{ post.title }}]({{ post.url }})
    {{ post.excerpt }}
{% endfor %}
