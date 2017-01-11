---
layout: page
title: All Posts
---

<ul>
  {% for post in site.posts %}
    {% if post.url %}
        <li><a href="{{ post.url }}">
        <span class="post-date">{{ page.date }}</span>
        [{{ post.category }}] {{ post.title }}
        </a></li>
    {% endif %}
  {% endfor %}
</ul>

