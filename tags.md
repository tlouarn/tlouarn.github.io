---
title: Tags
layout: home
---

<h3>Tags</h3>
<ul>
  {% for post in posts limit:5 %}
    <li><a href="{{ post.url }}">{{ post.date | date: "%B %Y" }} - {{ post.title }}</a></li>
  {% endfor %}
</ul>
