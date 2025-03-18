---
title: Archive
layout: page
---

All posts ordered by reverse chronological order.

<ul>
  {% for post in posts %}
    <li><a href="{{ post.url }}">{{ post.date | date: "%B %Y" }} - {{ post.title }}</a></li>
  {% endfor %}
</ul>