layout: default
title: Home
---

<h1>{{ site.title }}</h1>

{% assign years = "2026,2025,2024,2023,2022" | split: "," %}

{% for year in years %}

  <h2 style="margin-top: 30px; font-size: 20px; font-weight: normal;">
    {{ year }}
  </h2>

  <ul class="post-list">
    {% assign year_posts = site.posts | where_exp:"post", "post.date | date: '%Y' == year" %}

    {% for post in year_posts %}
      <li>
        <span class="bullet">•</span>
        <span class="date">{{ post.date | date: "%b %d" }}</span>
        <a class="post-link" href="{{ post.url }}">{{ post.title }}</a>
      </li>
    {% endfor %}

    {% if year_posts == empty %}
      <li class="no-posts">No posts</li>
    {% endif %}
  </ul>

{% endfor %}
