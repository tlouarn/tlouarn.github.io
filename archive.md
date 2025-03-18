---
title: Archive
layout: page
---

<ul>
  {% for post in site.posts %}
    <li>{{ post.date | date: "%B %Y" }} - <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>