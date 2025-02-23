---
title: Welcome
layout: home
---

<h3>Posts</h3>
<ul>
  {% for post in posts] %}
    <li><a href="{{ post.url }}">{{ post.date | date: "%B %Y" }} - {{ post.title }}</a></li>
  {% endfor %}
</ul>

