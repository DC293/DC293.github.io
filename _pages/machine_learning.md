---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
---

{% assign grouped = site.machine_learning | group_by: "type" %}

{% for group in grouped %}
  {% if group.name %}
    <h2>{{ group.name }}</h2>
    <div class="entries-grid">
      {% for post in group.items %}
        {% include archive-single.html type="grid" %}
      {% endfor %}
    </div>
  {% endif %}
{% endfor %}
