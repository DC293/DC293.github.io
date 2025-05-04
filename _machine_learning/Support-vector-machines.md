---
layout: single
weight: 9
title: "Support vector machines (SVM)"
excerpt: "Utilising kernels for margin-based classification"
header:
    overlay_image: assets/images/SVM_cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/SVM_cover.png
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
A Support Vector Machine (SVM) is a machine learning model used primarily for binary classification tasks. It works by finding a decision boundary that separates data points into two classes, while maximising the margin between the nearest points.

While SVMs can use multiple features to build a model, it's easier to visualise when we limit ourselves to just two features.

To demonstrate how SVMs work, we'll use Fisher's Iris dataset. This classic dataset includes four features: the length and width of the sepals and petals, which can help distinguish between three species of Iris flowers.

Below, we have plotted the sepal length versus sepal width for two species — Setosa and Versicolor. An approximate linear decision boundary is also plotted. New flowers can be classified based on their position relative to this line: if a point falls above the line, it will be classified as a Setosa; if it falls below, it will be classified as a Versicolor.

<p align="center">
  <img src="/assets/images/SVM_bivariable_plot.png" alt="SVM_bivariable_plot" width="500">
</p>

We can model more than two features, however, the more we add, the harder it becomes to visualise. For three features, we'd use a 3D plane, after that, it becomes a lot more complex!

## Support vectors and margins
To give the model the greatest chance of success, we need to optimise the placement of our decision boundary. There are infinitely many ways we could separate our features, but we want the boundary that gives us the highest chance of correct classification. This means placing the boundary as far away as possible from the nearest data points from each class.

To achieve this, SVM identifies specific data points — called support vectors — that are closest to the decision boundary. These points "support" the boundary: they determine its exact position and orientation. In two dimensions, at least three support vectors are typically needed to define a boundary, although in real-world datasets, there are often more. The key idea is that only these support vectors matter when constructing the SVM decision boundary — not the rest of the data points. This makes SVM one of the most efficient machine learning models, as it only needs to use the support vectors to train. 

Below, we can see the support vectors from our original example. See how only the closest points to the decision boundary are supported.

<p align="center">
  <img src="/assets/images/SVM_support_vectors.png" alt="SVM_support_vectors" width="500">
</p>

The distance between the decision boundry and the support vectors is known as the margin. In order to have the highest success rate of classification, we want to maximise the size of the margin. 

<p align="center">
  <img src="/assets/images/SVM_margin.png" alt="SVM_margin" width="500">
</p>

## scikit-learn
In previous articles, we have created our own functions to demonstate the workings of the scikit machine learning models. However, as we move onto more complex machine learning models, this becomes much harder! Whilst, we wont be building our own function, we will explore what the scikit SVM model is doing with our data. The objective of the model being to optimise the placement of the decision boundary to give us the largest margin.

Similarly to the other machine learning models, we need to create a SVC object. Note the slight change in name, the 'C' standing for Classifier instead of Machine. 


