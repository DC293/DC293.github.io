---
title: Machine Learning
layout: single
permalink: /machine-learning/
author_profile: true
classes: wide
---

{% assign grouped = site.machine-learning | group_by: "categories" %}

{% for category in grouped %}
  {% assign category_name = category.name %}
  {% if category_name %}
  <h2>{{ category.name | first }}</h2>
  <div class="entries-grid">
    {% for post in category.items %}
      {% include archive-single.html type="grid" %}
    {% endfor %}
  </div>
  {% endif %}
{% endfor %}
