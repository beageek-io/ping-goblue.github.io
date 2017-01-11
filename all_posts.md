---
layout: page
title: All Posts
---

<ul>
  {% for post in site.posts %}
    {% if post.url %}
        <li><a href="{{ post.url }}">[{{ post.category }}] {{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

