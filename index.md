---
layout: default
title: Home
---

{% for post in site.posts %}
{{ post.date | date: "%b %d %Y" }} [{{ post.title }}]({{ post.url | relative_url }})

{% endfor %}
