---
layout: single
weight: 10
title: "K-Means Clustering"
excerpt: "Finding natural groupings in data using unsupervised learning."
header:
    overlay_image: assets/images/kmeans_cover.png
    overlay_filter: 0.5
    teaser: assets/images/kmeans_cover.png
toc: true
toc_sticky: true 
published: true
toc_label: "Contents:"
collection: machine_learning
type: clustering
author_profile: True
---

{% include mathjax.html %}

# K-Means Clustering
We can't always rely on having labelled training data. In many situations, we simply have a dataset and want to understand the patterns within it.
This is where **clustering** algorithms become useful. Clustering is a type of **unsupervised learning**, where the goal is to group similar data points together without knowing the correct labels beforehand. One of the most widely used clustering algorithms is **K-Means**. Rather than predicting labels, K-Means tries to identify natural groupings within the data.

# Background
The objective of clustering is to separate data into groups that are similar. To do this, we must define two things:

- How many groups should the data be separated into?
- How do we measure similarity between data points?

In K-Means, similarity is usually measured using Euclidean distance, which is simply the straight-line distance between two points. More information on how this distance is calculated can be found in the [measuring distance page](../Distance).

## Process overview
The algorithm follows a simple iterative process:

1. Choose the number of clusters $K$
2. Randomly initialise $K$ cluster centres (called **centroids**)
3. Assign each data point to the closest centroid
4. Recalculate each centroid as the average of the assigned points
5. Repeat steps 3–4 until the centroids stop changing

The goal is to minimise the within-cluster variance, meaning points within each cluster are as close together as possible.

# Sporting Example: Football Player Styles
Imagine we want to group football players based on their playing style. We record two simple statistics:

- Goals scored
- Assists made

Our dataset might look like this:
<p align="center">
  <img src="/assets/images/K-means_goals_vs_assists.png" alt="Player Performance - Goals v Assists" width="500">
</p>

We can see there is a general negative correlation in the data, from which we may categorise plyers as:

- Strikers (high goals)
- Playmakers (high assists)
- Balanced attackers (equal goals and assists)

Rather than doing this ourselves, K-Means can discover these groups automatically.

# Implementing K-Means in Python

Let's implement a simple version of the algorithm.

```python
import numpy as np

data = np.array([
    [25,4],
    [22,6],
    [5,18],
    [6,20],
    [14,10],
    [13,9]
])

k = 3

# Randomly choose centroids
centroids = data[np.random.choice(len(data), k, replace=False)]

for _ in range(10):

    clusters = []

    for point in data:
        distances = np.linalg.norm(point - centroids, axis=1)
        cluster = np.argmin(distances)
        clusters.append(cluster)

    clusters = np.array(clusters)

    new_centroids = []

    for i in range(k):
        new_centroids.append(data[clusters == i].mean(axis=0))

    centroids = np.array(new_centroids)

print("Centroids:")
print(centroids)
