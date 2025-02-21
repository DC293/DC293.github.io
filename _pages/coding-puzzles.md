---
title: Coding Puzzles
layout: collection
permalink: /coding-puzzles/
collection: puzzles
entries_layout: grid
classes: wide
---

{% for puzzle in site.puzzles %}
  <div class="grid__item">
    <h3><a href="{{ puzzle.url }}">{{ puzzle.title }}</a></h3>
    <p>{{ puzzle.excerpt }}</p>
  </div>
{% endfor %}

# My solutions for all of the puzzles in the [Advent of Code 2023](https://adventofcode.com/2023).

# [Day 1](../_posts/2023-12-01-AOC_2023_Day_1.md)

# [Day 2](../_posts/2023-12-02-AOC_2023_Day_2.md)
