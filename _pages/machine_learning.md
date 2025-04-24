---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
---

## Supervised models
### Regression

<div class="entries-grid">
  {% assign supervised = site.machine_learning | where: "type", "regression" %}
  {% for post in supervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

### Classification

<div class="entries-grid">
  {% assign supervised = site.machine_learning | where: "type", "classification" %}
  {% for post in supervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

## Unsupervised models

### Clustering
<div class="entries-grid">
  {% assign unsupervised = site.machine_learning | where: "type", "clustering" %}
  {% for post in unsupervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

## Neural networks
<div class="entries-grid">
  {% assign unsupervised = site.machine_learning | where: "type", "neural-network" %}
  {% for post in unsupervised %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
