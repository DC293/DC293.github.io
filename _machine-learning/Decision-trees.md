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
Consider the two decision trees below. Which tree would be more useful for predicting whether someone would get a mortgage offer?

<p align="center">
  <img src="/assets/images/Decision tree.png" alt="Decision tree" width="500">
</p>

The top tree creates some uncertainty as the training data has labels from both classes. The bottom tree splits the data much more evenly and ends up with a leaf with only one class label. We’d be much more confident about our prediction using the bottom tree.

To measure the level of uncertainty we can calculate the Gini impurity. To calculate the Gini impurity of a set of data points, start at 1, then subtract the sum of the squared percentages of each class in the set. For example, if a dataset contains three items from class A and one item from class B, the Gini impurity would be:

$$
1 - \left(\frac{3}{4}\right)^2 - \left(\frac{1}{4}\right)^2 = 0.375
$$

Values with low impurity are most desirable, which is indicated by a value close to 0. If a data set has only one class, like our bottom tree example, you’d end up with a Gini impurity of 0. 

```python
from collections import Counter

def gini(dataset):
  impurity = 1
  label_counts = Counter(dataset)
  for label in label_counts:
    prob_of_label = label_counts[label] / len(dataset)
    impurity -= prob_of_label ** 2
  return impurity
```

### Information gain
We want our decision tree to split data in a way that reduces impurity as much as possible. But how do we decide which feature to split on? To answer this, we calculate the information gain, which measures the reduction in impurity after a split.

For example, say we start with a dataset with an initial impurity of 0.5. We split the dataset based on a feature (e.g. credit score) and end up with three subsets with impurities 0, 0.375, and 0.

<p align="center">
  <img src="/assets/images/Decision tree impurity.png" alt="Decision tree impurity" width="500">
</p>

Splitting the data helps reveal patterns by creating purer subsets. The higher the information gain, the better the split. If it's 0, the split didnt provide any further information.

$$
\text{information gain} = 0.5-(0.0+0.375+0.0)=0.125
$$

### Weighted information gain
What we havent yet considered is the size of the groups after splitting. For example, the groups below have the same impurity however the groups with more items are much more useful. 

<p align="center">
  <img src="/assets/images/Weighted impurity.png" alt="Decision tree impurity group size" width="500">
</p>

To account for this, we can caluclate the weighted information gain. If the group before the split contained 10 items and one of the splits contained 2 items, then the weighted impurity of that group would be 2/10 multiplied by the impurity. By doing this, the influence of groups with fewer items are reduced. 

<p align="center">
  <img src="/assets/images/Decision tree impurity.png" alt="Decision tree impurity weighted" width="500">
</p>

$$
\text{weighted information gain} = 0.5-(0.0x\frac{4}{10}+0.375x\frac{4}{10}+0.0x\frac{2}{10})=0.35
$$




