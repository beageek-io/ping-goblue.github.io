---
layout: page
title: Miscellaneous
---

<ul>
  {% for post in site.categories.miscellaneous%}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

