---
title: Machine Learning
layout: single
permalink: /machine-learning/
author_profile: true
classes: wide
---

{% assign grouped = site['machine-learning'] | group_by: "type" %}

{% for category in grouped %}
  {% if category.name %}
    <h2>{{ category.name }}</h2>
    <div class="entries-grid">
      {% for post in category.items %}
        {% include archive-single.html type="grid" %}
      {% endfor %}
    </div>
  {% endif %}
{% endfor %}
