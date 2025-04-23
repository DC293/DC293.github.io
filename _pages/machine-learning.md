---
title: Machine Learning
layout: collection
permalink: /machine-learning/
collection: machine-learning
entries_layout: grid
classes: wide
sort_by: weight
order: reverse
author_profile: True
---

Welcome to my machine learning collection. Below are posts grouped by subtopic.

---

{% assign ml_posts = site.machine-learning %}
{% assign all_categories = "" | split: "" %}

{% for post in ml_posts %}
  {% for cat in post.categories %}
    {% unless all_categories contains cat %}
      {% assign all_categories = all_categories | push: cat %}
    {% endunless %}
  {% endfor %}
{% endfor %}

{% assign all_categories = all_categories | sort %}

{% for cat in all_categories %}
  ### {{ cat | capitalize }}

  <div class="entries-grid">
  {% for post in ml_posts %}
    {% if post.categories contains cat %}
      {% include archive-single.html type="grid" %}
    {% endif %}
  {% endfor %}
  </div>
{% endfor %}


