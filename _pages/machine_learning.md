---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
---

## Supervised

<div class="entries-grid">
  {% assign supervised = site.machine_learning | where: "type", "Supervised" %}
  {% for post in supervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

## Unsupervised

<div class="entries-grid">
  {% assign unsupervised = site.machine_learning | where: "type", "Unsupervised" %}
  {% for post in unsupervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
