---
layout: single
weight: 5
title: "K-Nearest neighbours (KNN)"
excerpt: "Classifying and predicting unknown data points"
header:
    overlay_image: assets/images/KNN_example_equal_axis.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/KNN_example_equal_axis.png
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

# KNN Classification
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

Min-max normalization is one of the most common ways to normalise data. For every variable, the minimum value gets transformed into a 0, the maximum value gets transformed into a 1, and every other value gets transformed into a decimal between 0 and 1. For example, if the variable had a range of 30-50, 30 would be 0 and 50 would be 1. Values falling between these would be scaled appropriately e.g. 40 would turn to 0.5.

Min-max normalisation has one major drawback: it struggles dealing with outliers. For instance, if you have 99 values ranging from 0 to 30, but a single value is 100, those 99 values will be scaled between 0 and 0.3. This means the data remains just as compressed as before.

$$
x' = \frac{x - \min(x)}{\max(x) - \min(x)}
$$

Where:  
- $x'$ = normalized value  
- $x$ = original value  
- $min(x)$ = minimum value in the dataset  
- $max(x)$ = maximum value in the dataset

To experiment with the KNN classifier, we have selected 3 variables from our cars dataset: engine size, horsepower and peak rpm. These variablesrelatively well distributed so we should have no problems using min-max normalisation. To simplify this, we can use the MinMaxScaler class from the sclearn library. After normalising the data, we can combine it back with our car names so we can identify the car in each row. 

```python
from sklearn.preprocessing import MinMaxScaler

data_2_norm = cars[['enginesize', 'horsepower', 'peakrpm']]
scaler = MinMaxScaler()
normalised_data = pd.DataFrame(scaler.fit_transform(data_2_norm), columns=data_2_norm.columns)

cars_dataframe_norm = dict(zip(cars['CarName'], normalised_data.values.tolist()))
```
### Z-score normalisation
Z-score normalisation provides an alternative to deal with data containing outliers. Instead of using the minimum and maximum of the dataset, we use the mean and standard deviation of the variable. If a value is equal to the mean it will be normalised to 0. If it is below the mean, it will be a negative number, and if it is above the mean it will be a positive number. How large those positive and negative numbers depends on the standard deviation. If the original data had a large standard deviation, the normalised values will be grouped closer to 0.

$$
Z = \frac{X - \mu}{\sigma}
$$

Where:
  - $X$ is the original value
  - $\mu$ is the mean of the dataset
  - $\sigma$ is the standard deviation of the dataset

## Finding nearest neighbours
In order to classify our unknown datapoint we need to determine the datapoints closest to it. To do this, we calculate the [distance between the points](/machine-learning/Distance/). We will use the Eculidean distance, which calculates the shortest distance. This method also allows us to use as many dimensions as we want, so we can add further variables to our model. 

```python
from scipy.spatial import distance

def classify(unknown, dataset, k):
  distances = []
  for car_name in dataset:
    distance_to_point = distance.euclidean(unknown, dataset[car_name])
    distances.append([distance_to_point, car_name])
  distances.sort()
  neighbors = distances[:k]
  return neighbors
```
If we make a fictional datapoint and feed it to our function along with our normalised cars dataframe, we should get the k-nearest cars along with the respective distances to each point. 
```python
print(classify([.4, .3, .7], cars_dataframe_norm, 3))

[[0.1896976281655098, 'audi 5000'],
 [0.1896976281655098, 'audi fox'],
 [0.1896976281655098, 'volkswagen rabbit']]
```
## Determining neighbour class
Now we have identified our nearest neighbours we need to determine whether the car are affordable or not. If more of the neighbors are affordable, then the algorithm
will classify the unknown point as affordable. Otherwise, it will classify it as non-affordable.

To do this, we will modify our function slightly to provide the classification labels for the cars database. If there are more afordable cars (1) than non-affordable (0) in the nearest data points we will return a 1, otherwise, we'll return 0. 

```python
labels = dict(zip(cars.CarName, cars.affordable))

def classify(unknown, dataset, labels, k):
    distances = []
    num_affordable = 0
    num_non_affordable = 0
    for car_name in dataset:
        distance_to_point = distance.euclidean(unknown, dataset[car_name])
        distances.append([distance_to_point, car_name])
        distances.sort()
        neighbors = distances[:k]
    
    for i, name in neighbors:
        if labels[name] == 1:
            num_affordable += 1
        else:
            num_non_affordable += 1
    
    if num_affordable > num_non_affordable:
        return 1
    else:
        return 0

classify([.4, .3, .7], cars_dataframe_norm, labels, 3)
```
```python
print(classify([.4, .3, .7], cars_dataframe_norm, labels, 3))
1
```
It is best practice to use odd numbers of classifiers to avoid a tie, however, if an even number is required, and there is a tie between classes, we need a way to select which class to pick. One method is to take the class of the nearerst data point. 

## Selecting K
Now we have created our function for finding the nearest neighbours and classifying our unknown point, but how do we select how many points we should use in our classification? A key concept for machine learning is training and validation of models. Below we have created some training and validation sets for our cars data. To do this we'll use sklearn's train_test_split function which takes a dataframe or series as an input. We'll then change these dataframes back into dictionaries to use in the functions we have defined above. 

```python
from sklearn.model_selection import train_test_split

x = pd.concat([cars[['CarName']], normalised_data], axis=1)
y = pd.concat([cars[['CarName']], cars.affordable], axis=1)

training_set, validation_set, training_labels, validation_labels = train_test_split(x, y, train_size=0.8, test_size=0.2, random_state=2)

training_set = dict(zip(training_set['CarName'], training_set.iloc[:, 1:].values.tolist()))
validation_set = dict(zip(validation_set['CarName'], validation_set.iloc[:, 1:].values.tolist()))
training_labels = dict(zip(training_labels['CarName'], training_labels.affordable))
validation_labels = dict(zip(validation_labels['CarName'], validation_labels.affordable))
```
If we take the points in our validation set as inputs to our K-nearest neighbour function and compare them to the data in our training set and classifications we can make a prediction of classification for these points. We can then look in our validation labels to see if it got the prediction correct. If we can count the number of times the classifier got the answer right and the number of times it got it wrong we can compute the validation accuracy.

```python
def find_validation_accuracy(training_set, training_labels, validation_set, validation_labels, k):
  num_correct = 0.0
  for car in validation_set:
    guess = classify(validation_set[car], training_set, training_labels, k)
    if guess == validation_labels[car]:
      num_correct += 1
  validation_error = num_correct/len(validation_set)
  return validation_error

print(find_validation_accuracy(training_set, training_labels, validation_set, validation_labels, 5))
0.7666666666666667
```
If k is very large, our classifier will suffer from underfitting. Underfitting occurs when your classifier doesn’t pay enough attention to the small quirks in the training set. Imagine you have 100 points in your training set and you set k = 100. Every single unknown point will be classified in the same exact way. The distances between the points don’t matter at all! This is an extreme example, however, it demonstrates how the classifier can lose understanding of the training data if k is too big. 

With K set to 5, we can see our validation accuracy was 77%, not bad! Lets take a moment to consider the implications of choosing a very small or very large k value. If we choose a small value such as k = 1, we will classify our point based on the single nearest neighbour. We call this overfitting, as the model assumes our unknown point will always perform as the training data. On the flip side, if we have a large k, we risk underfitting as our model will be taking so many points into consideration we miss the nuances in our data. 

To find our optimal K-value we can run our function multiple times and visualise the results. 

<p align="center">
  <img src="/assets/images/K-nearest-neighbours_validation_accuracy.png" alt="Car price - engine size v horsepower" width="500">
</p>

Based on the plot, it appears a k value between 10 and 15 would give us an optimal prediction. It should be noted, however, that this can fluctuate slightly depending on our training-validation test split. 

## sklearn Classification
We've learnt how K-Nearest Neighbours algorithm works, to save us from having to define the above functions, we can utilise sklearn's [KNeighborsClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html). The object takes one argument, the K-vlaue. We then need to train the model using our dataset. 

```python
from sklearn.neighbors import KNeighborsClassifier

max_k = 30
accuracies = []
for k in range(1, max_k):
  classifier = KNeighborsClassifier(n_neighbors=k)
  classifier.fit(training_set, training_labels)
  accuracies.append(classifier.score(validation_set, validation_labels))
```

<p align="center">
  <img src="/assets/images/KNN_validation_accuracy_sklearn.png" alt="Car price - engine size v horsepower" width="500">
</p>

Plotting our validation graph from our model fit shows it has good aggreement with our functions with approximately 15 points providing the best results. 

# KNN Regression
We now know how we can classify whether our car is affordable, but what if we wanted to predict the actual price? For this, we can use the KNN regressor. The process is almost identical to classification, except instead of counting the number of affordable and non-affordable neighbors, the regressor averages their prices.

Below we have adjusted our classification algorithm to provide the average price of our nearest neighbours instead of the affordability classification. When we test our new function, we can see it outputs a price of $14,792.

```python
def predict(unknown, dataset, car_prices, k):
  distances = []
  for car_name in dataset:
    car = dataset[car_name]
    distance_to_point = distance.euclidean(unknown, dataset[car_name])
    distances.append([distance_to_point, car_name])
  distances.sort()
  neighbors = distances[0:k]

  sum_of_ratings = 0
  for dist, car in neighbors:
    sum_of_ratings += car_prices[car]

  return sum_of_ratings/len(neighbors)

predict([.4, .3, .7], cars_dataframe_norm, dict(zip(cars.CarName, cars.price)), 15)
14792.466666666667
```
## Weighted regression
We've managed to estimate the car price using the average of our nearest neighbours, but what if we can be even smarter with our calculation. Instead of calculating an average, we can apply weighting to our points so the nearest points influence our result to greater degree.

We can use the equation below to create the weighting. The numerator is the sum of every price divided by their respective distances. The denominator is the sum of one over every distance. Even though the prices are the same as before, the weighted average has now gone up to $15,000.

$$
\hat{y} = \frac{\sum\limits_{i=1}^{k} \frac{y_i}{d_i}}{\sum\limits_{i=1}^{k} \frac{1}{d_i}}
$$

Where:
  - $\hat{y}$ is the weighted prediction
  - $y_i$ is the rating of the $i-th neighbour
  - $d_i$ is the distance of the $i-th neighbour
  - $k$ is the number of neighbours

Replacing the sum of ratings loop in our predict() function gives us the weighted average:
```python
def predict(unknown, dataset, car_prices, k):
  distances = []
  for car_name in dataset:
    car = dataset[car_name]
    distance_to_point = distance.euclidean(unknown, dataset[car_name])
    distances.append([distance_to_point, car_name])
  distances.sort()
  neighbors = distances[0:k]

  numerator = 0
  denominator = 0
  for dist, car in neighbors:
    numerator += car_prices[car]/dist
    denominator += 1/dist

  return numerator/denominator

predict([.4, .3, .7], cars_dataframe_norm, dict(zip(cars.CarName, cars.price)), 15)
15000.968322654364
```
## sklearn Regression
Using the sklearn K-Neighbours Regressor is very similar to the K-Neighbours classifier, we first must import and create the regressor. When defining the regressor we can also choose whether or not to use a weighted average using the parameter weights. We can either set the keyword to "uniform" for equal weighting or "distance" for a weighted average. 

We have fitted the model using our data once again, and we find a good match to our user defined wighted function. 

```python
from sklearn.neighbors import KNeighborsRegressor

regressor = KNeighborsRegressor(n_neighbors = 15, weights = "distance")
regressor.fit(normalised_data, cars.price)

print(regressor.predict([[.4, .3, .7]]))
15000.96832265
```








