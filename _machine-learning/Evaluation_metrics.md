---
layout: single
weight: 8
title: "Classification evaluation metrics"
excerpt: "Evaluating a model’s predictive power"
header:
    overlay_image: assets/images/Evaluation metrics cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Evaluation metrics cover.png
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

# Evaluation metrics

## Confusion matrix
When creating a machine learning model, how can we check how well it can make predictions? After splitting the data into training and validation sets we can check the model outputs against the true values. 

Let's say we are trying to predict whether a student will pass or fail their final exam. After training a model with a training data set, we can pass a validation data set into the trained model. The model will output a series of predictions which we can then compare to our validation labels. From these comparisons there are four possible categories the results can fall under: 

True Positive (TP): The algorithm predicted pass and it was pass
True Negative (TN): The algorithm predicted fail and it was fail
False Positive (FP): The algorithm predicted pass and it was fail
False Negative (FN): The algorithm predicted fail and it was pass

To understand this better, we can visualise this as a confusion matrix. In the matrix, predicted classes are represented as columns and the actual classess are represented as rows. 

<p align="center">
  <img src="/assets/images/Confusion matrix.png" alt="Confusion matrix" width="500">
</p>

