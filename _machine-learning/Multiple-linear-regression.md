---
layout: single
weight: 1
title: "Multiple linear regression"
excerpt: "Fundamentals of multiple linear regression"
header:
    overlay_image: assets/images/engine_vs_hp_header.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/engine_vs_hp_header.png
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

# Multiple linear regression
We previously learnt how we can use linear regression to predict the value of a single independant variable from its relationship to a dependant variable. 
What if we want to see the impact of multiple independant variables on the dependant variable? We can use multiple linear regression. 

For example, lets say we want to buy a new car and we want to see what factors impact the price. This could include a number of variables such as size, number of doors, fuel type and engine size. As with linear regression, we will be optimising the slope $m$ and intercept $b$ of a line, but with more degrees of freedom:

$$
y = b + m_1x_1 + m_2x_2 + m_nx_n
$$

In this article we will use the [car price database](https://www.kaggle.com/datasets/hellbuoy/car-price-prediction) to investigate which variables impact car price. 

## Training vs Test data
Most machine learning models requrire the dataset to be split into a training and test dataset. As the names suggest, the training set is used to train the model to fit to the data whilst the test dataset is used to validate the model fitting. It is up to the user how to split their data into training and test data, however as a general rule, we should use 80% for the training and 20% for the test. 

Below we have imported our [cleaned cars database](/data-science/Data-cleaning/) and split it into a 80% training and 20% test set using scipy. The random_state variable tells the function to always split the data the same way, no matter how many times we call the function. If we wanted a completely random split, we could remove it or set it to None. 

```python
import pandas as pd
from sklearn.model_selection import train_test_split

cars = pd.read_csv('car_prices.csv')

x = cars[['fueltype', 'aspiration', 'doornumber', 'carbody',
            'drivewheel', 'enginelocation', 'cylindernumber',
            'enginesize', 'stroke', 'compressionratio',
            'horsepower' 'citympg', 'highwaympg']]
y = cars[['price']]

x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.8, test_size=0.2, random_state=10)

print(len(cars))
205

print(len(x_train))
164

print(len(x_test))
41
```

## Model fitting
One we have defined our training and test sets, we can build our model. 

The steps for multiple linear regression using scipy are identical to the steps for simple linear regression, we must first import and define our model before we fit the data.
```python
from sklearn.linear_model import LinearRegression

m_lr = LinearRegression()

m_lr.fit(x_train, y_train)
y_predict = m_lr.predict(x_test)
```
Lets see how well our model has estimated the price based on the variables we have given. The table below shows information for an audi 100 ls which has a price of $13,950.

{:.text-center}    
|    Variable    | Original | Encoded | Model coefficient |
|--------------: |:--------:|:-------:|:-----------------:|
|fueltype        |gas       | 1       |7969               |
|aspriation      |std       | 0       |-54                |
|doornumber      |four      | 0       |-897               |
|carbody         |sedan     | 3       |-203               |
|drivewheel      |fwd       | 1       |255                |
|enginelocation  |front     | 0       |4468               |
|cylindernumber  |four      | 2       |58                 |
|enginesize      |109       | 109     |127                |
|stroke          |3.40      | 3.4     |-2263              |
|compressionratio|10.0      | 10.0    |846                |
|horsepower      |102       | 102     |59                 |
|citympg         |24        | 24      |-98                |
|highwaympg      |30        | 30      |-22                |

Database values for Audi 100 LS.

Feeding these values into our model produces a price of $11,746, slightly lower than the true value. 
```python
print(m_lr.predict([[1, 0, 0, 3, 1, 0, 2, 109, 3.4, 10.0, 102, 24, 30]]))
11746.66581913
```

By plotting our test set against our model predictions, we can get an idea of how the model is fitting. With the exception of some noticiable outliers, the data tends to cluster around the y=x line. 

<p align="center">
  <img src="/assets/images/test_set_v_model_prediction.png" alt="Engine Size vs Horse Power" width="500">
</p>

As with linear regression, we can view our coefficient and intercept, however, because weve used multiple independant variables, we will have multiple coefficients. What can we take from these coefficients? Coefficients that are larger imply they have a greater influence on the dependant variable.  

<p align="center">
  <img src="/assets/images/assessing_relationships.png" alt="Engine Size vs Horse Power" width="500">
</p>

Evaluating our graphs, it appears engine size, horse power, city mpg and highway mpg have the biggest influence on our price. Engine location also carries some weight, however the data for cars with rear engines is limited. 

## Residual analysis
To assess our model accuracy, we need a method to measure the distance of our prediction from our true values. One technique we can use is residual analysis. We can think of residual analysis as:

$$
e = y - \hat{y}
$$

In the car price dataset, $y$ is the actual price and $\hat{y}$ is the predicted price.

To help us calculate the residual, we can use scikit's linear regression method .score(). The score returns the coefficient of determination (R²) of the model prediction.

 that returns the coefficient of determination R² of the prediction.

The coefficient R² is defined as:
