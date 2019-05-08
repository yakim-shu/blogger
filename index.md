---
layout: default
title: Home
---

{% for post in paginator.posts %}
<div class="posts">
  <h1>
    <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
  </h1>

  {% if post.image %}
    <a href="{{ post.url | prepend: site.baseurl }}"><img src="{{ post.image }}"></a>
  {% endif %}
  <p>
    {{ post.content | strip_html | truncate: 50 }} <a href="{{ post.url | prepend: site.baseurl }}">Read more</a>
    <span class="post-date" style="margin-top:3px"><i class="fa fa-calendar" aria-hidden="true"></i> {{ post.date | date_to_string }} - <i class="fa fa-clock-o" aria-hidden="true"></i> {% include read-time.html %}</span>
  </p>
</div>
{% endfor %}

<!-- Pagination links -->
<div class="pagination">
  {% if paginator.next_page %}
    <a class="pagination-button pagination-active" href="{{ site.baseurl }}{{ paginator.next_page_path }}" class="next">Older</a>
  {% else %}
    <span class="pagination-button">Older</span>
  {% endif %}

  {% if paginator.previous_page %}
    <a class="pagination-button pagination-active" href="{{ site.baseurl }}{{ paginator.previous_page_path }}">Newer</a>
    {% else %}
      <span class="pagination-button">Newer</span>
  {% endif %}
</div>