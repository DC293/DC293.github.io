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
Now we have learnt about how we can measure distance between points, lets see how we can use that in practice to determine the accuracy of a model. One of the simplest models we can use is a line. Many will be familiar with linear regression in the form of a line of best fit between two variables. A line can be determined by its slope and its intercept. We can represent this as:

$$
y = mx + b
$$
where:
$y$ is a given point on the y-axis
$m$ is the slope of the line
$x$ is a point on the x-axis
$b$ is the intercept of the line

To demonstrate this in action, we're going to use a [free dataset](https://www.kaggle.com/datasets/hellbuoy/car-price-prediction) containing car information. Below is a plot of engine size against horse power. I've attempted to fit a line to the data using $m = 0.85$ and $b = 0$. 

<p align="center">
  <img src="/assets/images/engine_vs_hp.png" alt="Engine Size vs Horse Power" width="500">
</p>

### Loss
The line we've fitted generally fits the data, but how can we measure how well it fits? To do this, we can calculate the loss, a number that measures how good the model’s fit is. We can think of loss as the squared distance from the point to the line. 

<p align="center">
  <img src="/assets/images/loss.png" alt="loss" width="500">
</p>

Lets see how well our line fits to our engine size v horse power plot. We can loop through all of our predicted y values comparing them against the plotted y values. The sum of all these differences gives us our total loss. 

```python
m = 0.8
b = 0
x = cars.enginesize

y_predicted = [m*x_value + b for x_value in x]

total_loss = 0
for i in range(len(y_predicted)):
  total_loss += (y[i] - y_predicted[i])**2

print(total_loss)
```
```python
111542
```

### Gradient decent
Our line of best fit will be one that gives us the lowest loss. It would take us a while to do this manually, so instead we can use tools to help us. 

To minimize loss, we take each parameter we are changing e.g. $m$ and $b$, and change it as long as we are decreasing loss. As long as loss is decreasing, we can carry on incrementing until we reach the optimal value upon which we see no further decreases in loss. This is known as gradient decent and can be visualised as:

<p align="center">
  <img src="/assets/images/gradient_descent.gif" alt="loss" width="500">
</p>

To find the gradient of loss as the intercept changes, we can use the formula:

$$
\frac{-2}{N} \sum_{i=1}^N (y_i - (mx_i + b))
$$

To find the gradient of loss as the slope changes we can use:

$$
\frac{-2}{N} \sum_{i=1}^N x_i(y_i - (mx_i + b))
$$

Where for both equations:
  - $N$ is the number of points you have in your dataset
  - $m$ is the current gradient guess
  - $b$ is the current intercept guess



