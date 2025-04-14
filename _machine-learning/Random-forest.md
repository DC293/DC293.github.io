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
Decision trees can be a useful tool for classification, however, there are limitations which can lead to sub-optimal results. One way to increase the effectivness of our model is to make a forest full of decision trees, otherwise known as a random forest. 

In order to make sure we find our optimal result, when creating each tree, we can put on constraints, forcing the tree to branch on a smaller subset of variables. This forces the decision trees to find different solutions to splitting the data. Once we have a forest full of trees, we classify our data with each one. The classification output that occurs most frequently from our forest is the result. 

## Bagging
You might be wondering how the trees in the random forest get created. After all, right now, our algorithm for creating a decision tree is deterministic — given a training set, the same tree will be made every time.

Random forests create different trees using a process known as bagging. Every time a decision tree is made, it is created using a different subset of the points in the training set. For example, if our training set had 1000 rows in it, we could make a decision tree by picking 100 of those rows at random to build the tree. This way, every tree is different, but all trees will still be created from a portion of the training data.

One thing to note is that when we’re randomly selecting these 100 rows, we’re doing so with replacement. Picture putting all 100 rows in a bag and reaching in and grabbing one row at random. After writing down what row we picked, we put that row back in our bag.

This means that when we’re picking our 100 random rows, we could pick the same row more than once. In fact, it’s very unlikely, but all 100 randomly picked rows could all be the same row!

Because we’re picking these rows with replacement, there’s no need to shrink our bagged training set from 1000 rows to 100. We can pick 1000 rows at random, and because we can get the same row more than once, we’ll still end up with a unique data set.

Let’s implement bagging! We’ll be using the data set of cars that we used in our decision tree lesson.
