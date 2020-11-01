---
name: compsci
layout: default
---
# Compsci posts

{% assign filtered_posts = site.posts | where: 'category', page.name %}
{% for post in filtered_posts %}
  <h3><a href="{{ post.url }}">{{ post.title }}</a></h3>
  <p>{{ page.summary }}</p>
{% endfor %}
