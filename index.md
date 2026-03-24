---
layout: default
title: Home
---

<div style="float: left; margin-right: 20px; margin-top: 10px;">
  <img src="{{ '/imgs/hexley_fork_450_darwin.png' | relative_url }}" alt="Darwin macOS Dragon" style="max-width: 80px; height: auto; border-radius: 50%; margin-top: -10px;">
</div>

{% assign posts_by_year = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}

{% for year in posts_by_year %}
  <h2 style="font-size: 20px; font-weight: normal;">
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
