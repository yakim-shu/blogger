---
layout: page
title: categories
---

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul class="posts">
    {% for post in category[1] %}
      <p><a href="{{ post.url }}">{{ post.title }}</a></p>
    {% endfor %}
  </ul>
{% endfor %}