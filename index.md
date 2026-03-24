---
layout: default
title: Home
---

<h1>{{ site.title }}</h1>

{% assign posts_by_year = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}

{% for year in posts_by_year %}

  <h2 style="margin-top: 30px; font-size: 20px; font-weight: normal;">
    {{ year.name }}
  </h2>

  <ul class="post-list">
    {% for post in year.items %}
      <li>
        <span class="bullet">•</span>
        <span class="date">{{ post.date | date: "%b %d" }}</span>
        <a class="post-link" href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>

{% endfor %}
