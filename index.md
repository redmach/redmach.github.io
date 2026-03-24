---
layout: default
title: Home
---

Posts count: {{ site.posts | size }}

<ul>
{% for post in site.posts %}
  <li>{{ post.title }}</li>
{% endfor %}
</ul>
