---
layout: single
weight: 9
title: "Support vector machines (SVM)"
excerpt: "Utilising kernels for margin-based classification"
header:
    overlay_image: assets/images/Evaluation metrics cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Evaluation metrics cover.png
toc: true
toc_sticky: true 
published: true
toc_label: "Contents:"
#classes: wide
categories: machine_learning
type: classification
author_profile: True
---

{% include mathjax.html %}

# Support Vector Machine (SVM)
A Support Vector Machine (SVM) is a machine learning model that allows for classification of binary data. The model creates a boundry, splitting the data points with the largest possible margin. Unknown points are classified dependant on what side of the boundry line they fall. Whilst we can use any number of features, it is easier to visualise when we use two features. 

To demonstate SVM, we'll use Fisher's Iris data set. The dataset contains four features: the length and the width of the sepals and petals, which can be used to distinguish between three species of Iris flower. 

Below, we have plotted the sepal length vs width of two of the flowers. To distiguish between the two species an approximate linear decision boundry has been added. We can see how we can use this boundry to classify flowers on the sepal width and length. If it falls above the line, the model will classify as a Setosa, if its below, it'll classify as Versicolor. 

<p align="center">
  <img src="/assets/images/SVM_bivariable_plot.png" alt="SVM_bivariable_plot" width="500">
</p>
