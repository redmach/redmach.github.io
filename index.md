---
layout: default
title: Home
---

# redmach

Notes on reverse engineering, exploitation, and macOS internals.

---

{% assign posts_by_year = site.posts | group_by_exp:"post", "post.date | date: '%Y'" %}
{% for year in posts_by_year %}
## {{ year.name }}

{% for post in year.items %}- [{{ post.title }}]({{ post.url | relative_url }}) — {{ post.date | date: "%b %d" }}
{% endfor %}
{% endfor %}
