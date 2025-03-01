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
Machine learning is often used to predict numeric values. For example, sports teams may try to predict an athletes risk of injury based on their workload or the likelyhood of scoring a goal from a certain position. When building a model, its important to understand its accuracy. 

An common way of doing this is to measure the distance between the model's predictions and the true values. The smaller the difference, the more accrate the model.

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
Manhattan distance is similar to Euclidean distance. It's called the Manhattan distance because it can be viewed as the distance whilst navigating city blocks. Rather than summing the squared difference between each dimension, we instead sum the absolute value of the difference between each dimension.

In 2D this looks like:

$$
d = |x_2 - x_1| + |y_2 - y_1|
$$

For the nth degree:

$$
d = \sum_{i=1}^{n} |p_i - q_i|
$$

### Hamming distance
The Hamming distance is commonly used to compare words. For example, comparing the words “sport” and “spork” would give a distance of one as each letter represents a dimension.

For two words $p$ and $q$, each of length $n$, the Hamming distance can be calculated as:

$$
d_H(p, q) = \sum_{i=1}^{n} \delta(p_i, q_i)
$$

where:

$$
\delta(p_i, q_i) =
\begin{cases} 
1, & \text{if } p_i \neq q_i \\
0, & \text{otherwise}
\end{cases}
$$

### scipy library
Instead of writing these equations out manually, we can use the [scipy](https://scipy.org/) library. There are a couple of key differences to be aware of, firstly, Manhattan distance is called by the .cityblock() method. Secondly, the scipy calculation of Hamming distance will always return a number between 0 an 1 by summing the number of differences and dividing by the number of dimentions. 

```python
from scipy.spatial import distance

print(distance.euclidean([1, 2], [4, 0]))
print(distance.cityblock([1, 2], [4, 0]))
print(distance.hamming([5,4,9],[3,7,9]))
```
```python
3.605551275463989
5
0.6666666666666666
```

## Linear regression
Now we have learnt about how we can measure distance between points, lets see how we can use that in practice to determine the accuracy of a model. One of the simplest models we can use is a line. Many will be familiar with linear regression in the form of a line of best fit between two variables. 





