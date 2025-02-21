---
layout: archive
title: "Coding Puzzles"
permalink: /coding-puzzles/
author_profile: true
entries_layout: list
---
Below are some coding puzzles to test your skills!

{% for post in site.categories.coding-puzzles %}
- [{{ post.title }}]({{ post.url }}) ({{ post.date | date: "%B %d, %Y" }})
{% endfor %}
