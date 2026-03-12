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

Let's implement a simple version of the algorithm. For this example, a dataset has been generated at random. 

## Choose number of clusters
Step 1 is to choose the number of clusters $K$ we would like to use. We will use 3 clusters to try and identify the 3 types of players. 

```python
k = 3
```
## Select initial centroids
In step 2 we randomly assign $K$ number of centroids within our dataset. 

```python
centroids_x = np.random.uniform(np.min(random_array[:, 0]), np.max(random_array[:, 0]), k)
centroids_y = np.random.uniform(np.min(random_array[:, 1]), np.max(random_array[:, 1]), k)

centroids = np.array(list(zip(centroids_x, centroids_y)))
```
Plotting this on the graph, we can see it selects three random positions within the dataset. 

<p align="center">
  <img src="/assets/images/K-means_goals_vs_assists_with_centroids.png" alt="K-means_goals_vs_assists_with_centroids" width="500">
</p>

## Assign each datapoint to the nearest centroid
Now we need to calculate which centroid each data point is closest to. To do this, we define an Euclidean distance function and then use it to calculate the distance between the centroids and data points. We then assign a label to each datapoint corresponding to the nearerst centroid.

```python
def distance(a, b):
  return ((a[0]-b[0])**2 + (a[1]-b[1])**2)**0.5

# Cluster labels for each point (either 0, 1, or 2)
labels = np.zeros(len(random_array))
distances= np.zeros(k)
# Distances to each centroid
for i in range(len(random_array)):
  for j in range(k):
    distances[j] = distance(random_array[i], centroids[j])
  
  cluster = np.argmin(distances)
  labels[i] = cluster

# Print labels
print(labels)
```
```python
[0. 0. 0. 0. 0. 0. 0. 1. 0. 1. 0. 0. 1. 0. 0. 0. 0. 0. 0. 0. 1. 0. 0. 0.
 1. 1. 2. 2. 1. 1. 2. 1. 1. 1. 2. 2. 1. 2. 1. 1. 1. 2. 1. 1. 2. 1. 2. 1.
 1. 1.]
```

## Recalculate centroids 
Next, we start to optimise our centroid placement by calculating the average position of all the data points for each centroid. These averages become the coordinates for the new centroids. 
```python
for i in range(k):
  points = [random_array[j] for j in range(len(random_array)) if labels[j] == i]
  centroids[i] = np.mean(points, axis=0)
```

We can see by averaging these positions our centroids move 
<p align="center">
  <img src="/assets/images/K-means_goals_vs_assists_after_update.png" alt="K-means_goals_vs_assists_after_update" width="500">
</p>
