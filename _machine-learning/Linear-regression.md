---
layout: single
weight: 1
title: "Linear regression"
excerpt: "Fundamentals of linear regression"
header:
    overlay_image: assets/images/AOC_2023/AOC_2023_Day4.jpg
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/AOC_2023/AOC_2023_Day4.jpg
toc: true
published: true
toc_label: "Contents:"
classes: wide
categories: machine-learning
tags: machine-learning
author_profile: True
---

{% include mathjax.html %}

## Measuring distance
Machine learning is often used to predict numeric values. For example, sports teams may try to predict an athletes risk of injury based on their workload or the likelyhood of scoring a goal from a certain position. When building a model, its important to understand its accuracy. An common way of doing this is to measure the distance between the model's predictions and the true values. The smaller the difference, the more accrate the model.

But not all distance is the same. Depending on the model, we may want to assess distance in different ways. Below, we will explore three common distance measurements used to assess machine learning model accuracy.

### Euclidean distance
Euclidean Distance: the most common distance formula, the length of a straight line between two points

$$
t_1 = \frac{\left(p^{(2)}_x - p^{(1)}_x\right)v^{(2)}_y - \left(p^{(2)}_y - p^{(1)}_y\right)v^{(2)}_x}
{v^{(1)}_x v^{(2)}_y - v^{(1)}_y v^{(2)}_x},
$$


### Manhattan distance
Manhattan Distance: the “city block” distance, useful in urban planning models


### Hamming distance
Hamming distance: used to measure distance between words in natural language processing
