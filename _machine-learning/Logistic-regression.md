---
layout: single
weight: 5
title: "Logistic Regression"
excerpt: "Predicting binary outcomes using a sigmoid function"
header:
    overlay_image: assets/images/KNN_example_equal_axis.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/KNN_example_equal_axis.png
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

# Logistic Regression
Logistic regression is a supervised machine learning algorithm that predicts the probability, ranging from 0 to 1, of a datapoint belonging to a specific category, or class. These probabilities can then be used to assign, or classify, observations to the more probable group.

For example, we could use a logistic regression model to predict the probability that you'll enjoy a certain movie. If that probability is greater than 0.5, we could add it to your recommended list. This is called binary classification because there are only two groups (eg., reccomended or not reccomended).

## Linear regression approach
Lets look at an example where we are looking to predict the chance of a person making a purchased based on the amount of time they're spent browsing on the site. The chance of making a purchase is a binary variable with 0 indicating no and 1 indicating yes. Using a linear regression model we can see the line does not fit the data very well. What's more, our line of best fit extends from positive to negative infinity. 

<p align="center">
  <img src="/assets/images/Logistic regression - linear.png" alt="Linear regression example" width="500">
</p>

## Logit link function
To provide a better fit to our data we can use logistic regression. To help understand how this works, lets first review our linear regression function:

$$
y = b_0 + b_1x_1 + b_2x_2 + b_nx_n
$$

To turn this into a logistic regression model, we apply a logic link function to the left hand side of the equation:

$$
ln(\frac{y}{1-y}) = b_0 + b_1x_1 + b_2x_2 + b_nx_n
$$
