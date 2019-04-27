---
layout: page
title: categories
---

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul class="posts">
    {% for post in category[1] %}
      <li itemscope>
        <a href="{{ post.url }}">{{ post.title }}</a>
        <p class="post-date"></p>
      </li>
    {% endfor %}
  </ul>
{% endfor %}