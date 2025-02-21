---
layout: single
title: "Coding Puzzles"
permalink: /coding-puzzles/
author_profile: true
entries_layout: grid
---

Below are some coding puzzles to test your skills!

{% for post in site.categories.coding-puzzles %}
  <div class="thumbnail-container">
    <a href="{{ post.url }}">
      <img src="{{ post.header.image | relative_url }}" alt="{{ post.title }}">
      <h3>{{ post.title }}</h3>
    </a>
  </div>
{% endfor %}
