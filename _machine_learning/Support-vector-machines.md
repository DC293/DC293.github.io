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


Decision boundaries are easiest to wrap your head around when the data has two features. In this case, the decision boundary is a line. Take a look at the example below.
