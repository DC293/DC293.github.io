---
title: Coding Puzzles
layout: collection
permalink: /coding-puzzles/
collection: puzzles
entries_layout: grid
classes: wide
---


{% assign reversed_posts = site.puzzles | reverse %}

{% for post in reversed_posts %}
  <div class="post">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    <p>{{ post.excerpt }}</p>
  </div>
{% endfor %}
