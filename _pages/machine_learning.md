---
title: Machine Learning
layout: single
permalink: /machine_learning/
author_profile: true
classes: wide
sort_by: weight
---

<!-- Supervised Models -->
<div class="section-title">
  <h2>Supervised models</h2>
</div>

<h3>Regression</h3>
<div class="entries-grid">
  {% assign regression = site.machine_learning | where: "type", "regression" %}
  {% for post in regression %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<h3>Classification</h3>
<div class="entries-grid">
  {% assign classification = site.machine_learning | where: "type", "classification" %}
  {% for post in classification %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<!-- Unsupervised Models -->
<div class="section-title">
  <h2>Unsupervised models</h2>
</div>

<h3>Clustering</h3>
<div class="entries-grid">
  {% assign clustering = site.machine_learning | where: "type", "clustering" %}
  {% for post in clustering %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<!-- Neural Networks -->
<div class="section-title">
  <h2>Neural networks</h2>
</div>
<div class="entries-grid">
  {% assign neural_network = site.machine_learning | where: "type", "neural-network" %}
  {% for post in neural_network %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>
