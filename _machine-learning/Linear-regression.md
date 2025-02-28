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
The most common distance formula, the Euclidean distance calculates the length of a straight line between two points. A familiar example is Pythagoras theorm to find the length of the hypotenuse of a right angled triangle:

$$
d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}
$$

But what if we wanted to find the distance between points in 3D, or 4D or nD space? We can calculate the distance the same way:

$$
d = \sqrt{\sum_{i=1}^{n} (p_i - q_i)^2}
$$

### Manhattan distance
Manhattan Distance: the “city block” distance, useful in urban planning models


### Hamming distance
Hamming distance: used to measure distance between words in natural language processing
