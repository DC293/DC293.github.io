---
layout: single
weight: 6
title: "Decision trees"
excerpt: "Split data recursively to make predictions"
header:
    overlay_image: assets/images/Decision tree cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Decision tree cover.png
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

## Gini impurity
Consider the two trees below. Which tree would be more useful as a model that tries to predict whether someone would get an A in a class?

<p align="center">
  <img src="/assets/images/Decision tree.png" alt="loss" width="500">
</p>

Let’s say you use the top tree. You’ll end up at a leaf node where the label is up for debate. The training data has labels from both classes! If you use the bottom tree, you’ll end up at a leaf where there’s only one type of label. There’s no debate at all! We’d be much more confident about our classification if we used the bottom tree.

This idea can be quantified by calculating the Gini impurity of a set of data points. To find the Gini impurity, start at 1 and subtract the squared percentage of each label in the set. For example, if a data set had three items of class A and one item of class B, the Gini impurity of the set would be

$$
1-(\frac{3}{4})^2\-(\frac{1}{4})^2\=0.35
$$

If a data set has only one class, you’d end up with a Gini impurity of 0. The lower the impurity, the better the decision tree!

```python
```
