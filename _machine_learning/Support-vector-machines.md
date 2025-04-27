---
layout: single
weight: 9
title: "Support vector machines (SVM)"
excerpt: "Utilising kernels for margin-based classification"
header:
    overlay_image: assets/images/Evaluation metrics cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Evaluation metrics cover.png
toc: true
toc_sticky: true 
published: true
toc_label: "Contents:"
#classes: wide
categories: machine_learning
type: classification
author_profile: True
---

{% include mathjax.html %}

# Support Vector Machine (SVM)
A Support Vector Machine (SVM) is a machine learning model used primarily for binary classification tasks. It works by finding a decision boundary that separates data points into two classes, while maximising the margin between the nearest points.

While SVMs can use multiple features to build a model, it's easier to visualise when we limit ourselves to just two features.

To demonstrate how SVMs work, we'll use Fisher's Iris dataset. This classic dataset includes four features: the length and width of the sepals and petals, which can help distinguish between three species of Iris flowers.

Below, we have plotted the sepal length versus sepal width for two species — Setosa and Versicolor. An approximate linear decision boundary is also plotted. New flowers can be classified based on their position relative to this line: if a point falls above the line, it will be classified as a Setosa; if it falls below, it will be classified as a Versicolor.

<p align="center">
  <img src="/assets/images/SVM_bivariable_plot.png" alt="SVM_bivariable_plot" width="500">
</p>
