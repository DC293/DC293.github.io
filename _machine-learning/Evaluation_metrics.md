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

To understand this better, we can visualise this as a confusion matrix. In the matrix, predicted classes are represented as columns and the actual classess are represented as rows. Ideally, we want the numbers along the main diagonal (true positives and true negatives) to be as high as possible, indicating fewer classification errors.

<p align="center">
  <img src="/assets/images/Confusion matrix.png" alt="Confusion matrix" width="500">
</p>

Lets see this in action:

```python
actual =    [1, 0, 0, 1, 1, 1, 0, 1, 1, 1]
predicted = [0, 1, 1, 1, 1, 0, 1, 0, 1, 0]

true_positives = 0
true_negatives = 0
false_positives = 0
false_negatives = 0

for i in range(len(actual)):
  if actual[i]==1 and predicted[i]==1:
    true_positives += 1
  elif actual[i]==0 and predicted[i]==1:
    false_positives += 1
  elif actual[i]==0 and predicted[i]==0:
    true_negatives += 1
  elif actual[i]==1 and predicted[i]==0:
    false_negatives += 1
  
print(true_negatives, false_positives, false_negatives, true_positives)
0 3 4 3
```

Instead creating our own function, we can utilise scikit's confusion matrix function. 
```python
from sklearn.metrics import confusion_matrix
conf_matrix = confusion_matrix(actual, predicted)
print(conf_matrix)
[[0 3]
 [4 3]]
```
## Accuracy
Once we have a confusion matrix, we can compute several key metrics to evaluate our model’s performance. Using the number of true positive (TP), true negative (TN), false positive (FP) and false negative (FN) we can calculate the following:

  - Accuracy: The proportion of correctly classified instances.

$$
\frac{TP + TN}{TP + FP + TN + FN}
$$

## Precision

  - Precision: The proportion of predicted positives that are actually positive.

$$
\frac{TP}{TP + FP}
$$

## Recall
  - Recall (Sensitivity): The proportion of actual positives that are correctly identified.

$$
\frac{TP}{TP + FN}
$$

## F1 score
 - F1 score: The weighted mean of precision and recall.



 
