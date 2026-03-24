---
layout: default
title: Home
---

<h1>{{ site.title }}</h1>

<ul class="post-list">
  {% for post in site.posts %}
    <li>
      <span class="date">{{ post.date | date: "%b %d" }}</span>
      <a class="post-link" href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
