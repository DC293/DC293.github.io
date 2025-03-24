---
layout: single
weight: 6
title: "Decision trees"
excerpt: "split data recursively to make predictions"
header:
    overlay_image: assets/images/Decision tree.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Decision tree.png
toc: true
toc_sticky: true 
published: true
toc_label: "Contents:"
#classes: wide
categories: machine-learning
tags: machine-learning
author_profile: True
---

{% include mathjax.html %}

# Decision trees
Decision trees are machine learning models that predict outcomes by splitting data based on different features. For example, when providing mortgage options, a bank might ask questions about your income, credit score, and salary. Each answer helps the bank assess financial security and predict the risk of default, enabling them to offer more personalized mortgage options.

## Creating trees
Decision trees are supervised machine learning models built using a training set of labeled data. The learning process begins by placing all data points at the top of the tree, where each point is labeled (e.g., whether a person received a mortgage offer).

Next, we split the data based on a feature, such as credit score. People with good credit scores go into one subset, while those with bad scores go into another.

This process of splitting continues recursively, using different features at each step. Eventually, we reach a point where further splitting is unnecessary, and we arrive at a leaf node. At this point, we count the majority label in the leaf, and any new, unlabeled data point reaching that leaf is classified according to the majority label.

<p align="center">
  <img src="/assets/images/Decision tree.png" alt="loss" width="500">
</p>

```python
```
