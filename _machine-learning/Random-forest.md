---
layout: single
weight: 7
title: "Random forest"
excerpt: "Combining multiple decision trees to improve accuracy"
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

# Random forest
We've seen that decision trees can be a useful tool for classification., however, they’re not without their weaknesses. Previously, we discussed the limitations of decision trees and ways to reduce this, such as pruning, but sometimes that isn’t enough. To improve generalisation, we can use a random forest.

A random forest cosists of a series of decision trees working together to classify new points. When a random forest classifies a new point, it passes it to each decision tree so each tree can provide a classification.  The classification outputs from all the trees are then counted, with the most popular answer taken as the final classification. This helps to reduce the impact of under/over fitted trees as results are averaged. 
