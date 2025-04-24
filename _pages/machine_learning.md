---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
sort_by: weight
---

<div class="section-title">
  <h2>Supervised models</h2>
</div>

### Regression

<div class="entries-grid">
  {% assign regression = site.machine_learning | where: "type", "regression" %}
  {% for post in regression %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

### Classification

<div class="entries-grid">
  {% assign classification = site.machine_learning | where: "type", "classification" %}
  {% for post in classification %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<div class="section-title">
  <h2>Unsupervised models</h2>
</div>

### Clustering

<div class="entries-grid">
  {% assign clustering = site.machine_learning | where: "type", "clustering" %}
  {% for post in clustering %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<div class="section-title">
  <h2>Neural networks</h2>
</div>

<div class="entries-grid">
  {% assign neural_network = site.machine_learning | where: "type", "neural-network" %}
  {% for post in neural_network %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
