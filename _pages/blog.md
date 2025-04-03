---
layout: page
title: Blog
permalink: /blog
---
## Latest Posts

{% for post in site.posts offset:1 %}
  - ### [{{ post.title }}]({{ post.url }})
    {{ post.excerpt }}
{% endfor %}
