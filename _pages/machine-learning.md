---
title: Machine Learning
layout: single
permalink: /machine-learning/
author_profile: true
classes: wide
---

## Supervised Models

<div class="entries-grid">
  {% assign post = site.posts | where: "title", "Measuring distance" | first %}
  {% include archive-single.html post=post %}
  
  {% assign post = site.posts | where: "title", "Linear regression" | first %}
  {% include archive-single.html post=post %}
</div>

## Unsupervised Models

<div class="entries-grid">
  {% assign post = site.posts | where: "title", "Multiple linear regression" | first %}
  {% include archive-single.html post=post %}

  {% assign post = site.posts | where: "title", "Logistic regression" | first %}
  {% include archive-single.html post=post %}
</div>
