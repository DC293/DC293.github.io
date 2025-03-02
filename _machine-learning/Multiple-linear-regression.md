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
            'drivewheel', 'enginelocation', 'enginetype', 'cylindernumber',
                'enginesize', 'fuelsystem', 'boreratio', 'stroke',
                   'compressionratio', 'horsepower', 'peakrpm', 'citympg', 'highwaympg']]
y = cars[['price']]

x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.8, test_size=0.2, random_state=10)

print(len(cars))
205

print(len(x_train))
164

print(len(x_test))
41
```















