---
layout: page
title: Archive
---


{% for post in site.posts %}
  <div class="archived-post">
    <div class="date-container">
      <span><small>{{ post.date | date: "%B %e, %Y" }}: </small></span>
    </div>
    <h3 class="title">
      <a href="{{ post.url }}">{{ post.title }}</a>
    </h3>
  </div>
{% endfor %}
