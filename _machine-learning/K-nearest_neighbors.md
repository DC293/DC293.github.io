---
layout: single
weight: 4
title: "K-Nearest neighbours"
excerpt: "Classifying unknown data points"
header:
    overlay_image: assets/images/K-nearest-neighbours_cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/K-nearest-neighbours_cover.png
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

# K-Nearest Neighbours (KNN) 
The K-nearest neighbours is a classification algorithm that works on the concept that data points with similar attributes tend to fall into similar categories.

The figure below provides a simple example. Lets assume the red marker is an unknown point of x-y that we want to classify. To gain more information about our point, we look to the nearest points surrounding it. If we expand our circle around our unknown point to encapsule the nearest 3 points (n = 3), we can see there are two orange triangles and one green square. In this case, we would classify our unknown point as a orange triangle. But what if we expanded our cicle to encapsule 6 points (n = 6). This adds an additional 2 green squares and a blue circle shifting the dominant category to the green squares, thus changing the classification of our point to a green square. 

This is the core concept of the K-Nearest Neighbour algorithm. Given a dataset where each point's class is known, you can classify a new point by identifying its nearest neighbours and assigning it the most common class among them.

<p align="center">
  <img src="/assets/images/K-nearest-neighbours_example.png" alt="K-nearest neighbours example" width="500">
</p>

```python
```
