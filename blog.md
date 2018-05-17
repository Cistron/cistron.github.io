---
layout: page
title: Ruminations
---

Maybe, with the correct weather conditions and if Jupiter is ascendentul to Mars, at some point, in the not so distant yet not too close future, I might actually publish some musings here.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span class="post-date">{{ post.date | date_to_string }}</span>
    </li>
  {% endfor %}
</ul>