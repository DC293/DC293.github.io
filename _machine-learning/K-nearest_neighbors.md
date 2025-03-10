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

Consider our dataset of car prices. Let’s take some continuous variables of the cars we can plot, for example:

  - The size of the engine
  - The horsepower of the car

Each of the datapoints could also contain some boolean information about the car, for example, whether it is a 2-door or 4-door. 

Now, let’s consider how we might classify the price of the cars. For the purpose of this exercise we’ll classify the cars as affordable or expensive. In our dataset, we’ve classified a car as affordable if it is $10,000 or lower. Every “affordable” car will have a class of 1, while every expensive car will have a class of 0.

Take a moment to look at the plot below. How easy do you think it would be to classify the price of a car based on the engine size and horsepower?
<p align="center">
  <img src="/assets/images/K-nearest-neighbours_enginesize_v_horsepower.png" alt="Car price - engine size v horsepower" width="500">
</p>

## Normalising data
Before we begin classifying our data, we need to normalise it. Normalisation is an important step to ensure all dimentions are treated equally. Imagine we added the peak rpm to our dataset to classify a car price. The difference in the peak rpm has a range of a few thousand, whilst the range in engine size and horsepower is barely larger than 100. If we did not normalise the data, the peak rpm would dominate the calculation. 

To resolve this, we'll normalise the data in each dimension so they are all values between 0 and 1. There are a couple of ways we can do this depending on our data.

### Min-max normalisation
Min-max normalisation first finds the minimum and maximum of the data. The formula then subtracts the minimum value from each data point, effectively shifting the minimum value to zero. Finally, the resulting value is divided by the range to produce a value between 0 and 1. 

$$
x' = \frac{x - \min(x)}{\max(x) - \min(x)}
$$

Where:  
- $x'$ = normalized value  
- $x$ = original value  
- $min(x)$ = minimum value in the dataset  
- $max(x)$ = maximum value in the dataset

To do this with our cars dataset, we can use the MinMaxScaler class from the sclearn library. We have selected 3 variables from our cars database to normalise: engine size, horsepower and peak rpm. After normalising the data, we can combine it back with our car names so we can identify the car in each row. 
```python
from sklearn.preprocessing import MinMaxScaler

data_2_norm = cars_df[['enginesize', 'horsepower', 'peakrpm']]
scaler = MinMaxScaler()
normalised_data = pd.DataFrame(scaler.fit_transform(data_2_norm), columns=data_2_norm.columns)

cars_df_norm = pd.concat([cars[['CarName']], normalised_data], axis=1)

cars_df_norm
```

## Finding nearest neighbours
In order to classify our unknown datapoint we need to determine the datapoints closest to it. To do this, we calculate the [distance between the points](/machine-learning/Distance/). We will use the Eculidean distance, which calculates the shortest distance. This method also allows us to use as many dimensions as we want, so we can add further variables to our model. 

```python
from scipy.spatial import distance

def classify(unknown, dataset, k):
  distances = []
  for title in dataset:
    distance_to_point = distance(unknown, dataset[title])
    distances.append([distance_to_point, title])
  distances.sort()
  neighbors = distances[:k]
  return neighbors

```
If we make a fictional datapoint and feed it to our function along with our normalised cars dataframe, we should get the k-nearest cars along with the respective distances to each point. 
```python
print(classify([.4, .3, .7], cars_dataframe, 'CarName', 3))

[[0.16158692555731724, 'volvo 244dl'],
 [0.18714799485945904, 'porsche macan'],
 [0.18870444539085077, 'peugeot 604sl']]
```
## Determining neighbour class
Now we have identified our nearest neighbours we need to determine whether the car are affordable or not. If more of the neighbors are affordable, then the algorithm
will classify the unknown point as affordable. Otherwise, it will classify it as non-affordable.

To do this, we will modify our function slightly to provide the classification labels for the cars database. If there are more afordable cars (1) than non-affordable (0) in the nearest data points we will return a 1, otherwise, we'll return 0. 

```python
labels = dict(zip(cars.CarName, cars.affordable))

def classify(unknown, dataset, colname, labels, k):
    distances = []
    num_affordable = 0
    num_non_affordable = 0
    for row in range(len(dataset)):
        distance_to_point = distance.euclidean(unknown, list(dataset.iloc[row, 1:]))
        distances.append([distance_to_point, dataset[colname].iloc[row]])
        distances.sort()
        neighbors = distances[:k]
    
    for i, name in neighbors:
        if labels[name] == 0:
            num_affordable += 1
        else:
            num_non_affordable += 1
    
    if num_affordable > num_non_affordable:
        return 1
    else:
        return 0
```
```python
print(classify([.4, .3, .7], cars_dataframe, 'CarName', labels, 3))
1
```
It is best practice to use odd numbers of classifiers to avoid a tie, however, if an even number is required, and there is a tie between classes, we need a way to select which class to pick. One method is to take the class of the nearerst data point. 

## Training and validation sets

