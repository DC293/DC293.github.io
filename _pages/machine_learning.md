---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
sort_by: weight
---

## Supervised models

### Regression

<div class="ml-posts-grid">
  {% assign regression = site.machine_learning | where: "type", "regression" %}
  {% for post in regression %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

### Classification

<div class="ml-posts-grid">
  {% assign classification = site.machine_learning | where: "type", "classification" %}
  {% for post in classification %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

## Unsupervised models

### Clustering

<div class="ml-posts-grid">
  {% assign clustering = site.machine_learning | where: "type", "clustering" %}
  {% for post in clustering %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

## Neural networks

<div class="ml-posts-grid">
  {% assign neural_network = site.machine_learning | where: "type", "neural-network" %}
  {% for post in neural_network %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
