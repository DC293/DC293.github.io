---
layout: single
weight: 4
title: "Logistic regression"
excerpt: "Predicting binary outcomes using a sigmoid function"
header:
    overlay_image: assets/images/Logistic regression - cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Logistic regression - cover.png
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

# Logistic Regression
Logistic regression is a supervised machine learning algorithm that predicts the probability, ranging from 0 to 1, of a datapoint belonging to a specific category, or class. These probabilities can then be used to assign, or classify, observations to the more probable group.

For example, we could use a logistic regression model to predict the probability that you'll enjoy a certain movie. If that probability is greater than 0.5, we could add it to your recommended list. This is called binary classification because there are only two groups (eg., reccomended or not reccomended).

## Linear regression approach
Lets look at an example where we are looking to predict the chance of a person making a purchased based on the amount of time they're spent browsing on the site. The chance of making a purchase is a binary variable with 0 indicating no and 1 indicating yes. Using a linear regression model we can see the line does not fit the data very well. What's more, our line of best fit extends from positive to negative infinity. 

<p align="center">
  <img src="/assets/images/Logistic regression - linear.png" alt="Linear regression example" width="500">
</p>

## Logit link function
To provide a better fit to our data we can use logistic regression. To help understand how this works, lets first review our linear regression function:

$$
y = b_0 + b_1x_1 + b_2x_2 + b_nx_n
$$

To turn this into a logistic regression model, we apply a logit link function to the left hand side of the equation:

$$
ln(\frac{y}{1-y}) = b_0 + b_1x_1 + b_2x_2 + b_nx_n
$$

The impact of this change means we can now fit a line that stays between 0 and 1 on the y-axis. 

<p align="center">
  <img src="/assets/images/Logistic regression - logit.png" alt="Linear regression example" width="500">
</p>

### Log-odds
In logistic regression, instead of treating classification as strictly binary (0 or 1), we model the probability $p$ that an event occurs. The log-odds transformation allows us to express this probability in a way that can be modeled linearly.

The odds of an event occurring is defined as:

$$
Odds = \frac{p}{1-p} = \frac{P(\text{event occuring})}{P(\text{event not occuring})}
$$

For example, suppose the probability of a person making a purchase is 0.7. The probability of not making a purchase is 1 - 0.7 = 0.3. Thus, the odds of making a purchase are:

$$
\text{Odds of purchase} = \frac{0.7}{0.3} = 2.33
$$

This means a person is 2.33 times more likely to make a purchase than not.

Since odds are always positive, taking the logarithm of the odds allows us to transform probabilities into a scale that ranges from negative to positive infinity:

$$
\text{Log-odds} = log(\frac{p}{1-p})
$$

A negative log-odds value means the probability is below 50%, while a positive log-odds value means the probability is above 50%. If the log-odds is 0, the probability is exactly 50%.

### Sigmoid function
To convert log-odds back into a probability, we apply the sigmoid function, which is the inverse of the log-odds transformation:

$$
P(y = 1) = \frac{e^{\text{log-odds}}}{1+e^{\text{log-odds}}}
$$

This function ensures that any real-valued number is mapped to a probability between 0 and 1.

```python
import numpy as np
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(C=1e5)
model.fit(x, y)

min_on_site = [0, 2, 1.5, 4, 2.5, 5.25]
log_odds = model.intercept_ + model.coef_ * min_on_site 

print(np.exp(log_odds)/(1+ np.exp(log_odds)))
[0.000, 0.045, 0.002, 0.99, 0.506, 0.999]
```

This shows how the sigmoid function converts log-odds into probabilities, which can then be used for classification decisions.

## sklearn predictions
Once we’ve trained a logistic regression model, we can use it to classify new data points. The .predict() method returns binary labels (0 or 1), indicating whether each sample belongs to the positive class.

```python
from sklearn.model_selection import train_test_split

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, random_state = 0)

# Create and fit the logistic regression model
from sklearn.linear_model import LogisticRegression
cc_lr = LogisticRegression()
cc_lr.fit(X_train,y_train)

# Print out the predicted outcomes
print(cc_lr.predict(X_test))

[0. 1. 1. 1. 0.]
```
If we need more than just class labels, we can use .predict_proba(), which returns the predicted probabilities for both classes (0 and 1). This method helps in understanding the model's confidence in its predictions.
 
```python
print(cc_lr.predict_proba(X_test))

[[0.5717 0.4283] # 57.17% chance of class 0, 42.83% chance of class 1
 [0.0168 0.9832]
 [0.045  0.955 ]
 [0.2348 0.7652]
 [0.8186 0.1814]]
```

By default, sklearn classifies a sample as 1 if its predicted probability for class 1 is greater than 0.5. This process is called thresholding. In the example above, the first and fifth datapoints have probabilities above 0.5 for class 0, so .predict() classifies them as 0s.

If needed, we can adjust this threshold to make the model more or less sensitive to positive classifications.

### Thresholding
By default, sklearn uses a threshold of 0.5. This means that if a sample has a predicted probability of 0.5 or higher, it is classified as 1; otherwise, it is classified as 0. This threshold, however, isn't fixed. We can adjust it based on the specific needs of our model.

Consider a logistic regression model that predicts whether an individual tests positive for Covid based on symptoms and other health indicators. If we stick to the default 0.5 threshold, we might miss some true positive cases, leading to undiagnosed infections and further spread.

To minimize the risk of false negatives (people who have COVID but are classified as negative), we could lower the threshold to 0.3 or 0.4. This makes the model more sensitive, meaning it will classify more people as positive, reducing the chance of missing actual cases.

The tradeoff? A lower threshold may also increase false positives, meaning some healthy individuals might be incorrectly classified as positive. However, in this case, it’s better to err on the side of caution—false positives can be confirmed with further testing, but false negatives could contribute to wider outbreaks.

Adjusting the threshold is a key decision in classification tasks, and the optimal value depends on the specific consequences of false positives versus false negatives in a given scenario.

## Confusion matrix
When evaluating our regression model, we need a way to validate its performance. For our logistic regression model we can use a confusion matrix to assess the performance. 

The matrix helps summarise the model’s performance by displaying the number of:

  - True Positives (TP) – Correctly predicted positive cases
  - False Positives (FP) – Incorrectly predicted positive cases
  - True Negatives (TN) – Correctly predicted negative cases
  - False Negatives (FN) – Incorrectly predicted negative cases

We can generate a confusion matrix in sklearn like this:

```python
from sklearn.metrics import confusion_matrix
print(confusion_matrix(y_test, cc_lr.predict(X_test)))
[[ 9  1]
 [ 2 13]]
```
This tells us:

  - 9 True Negatives (TN) – Correctly predicted as negative
  - 1 False Positive (FP) – Incorrectly predicted as positive
  - 2 False Negatives (FN) – Incorrectly predicted as negative
  - 13 True Positives (TP) – Correctly predicted as positive

Ideally, we want the numbers along the main diagonal (true positives and true negatives) to be as high as possible, indicating fewer classification errors.

### Accuracy, Recall, Precision, F1 Score
Once we have a confusion matrix, we can compute several key metrics to evaluate our model’s performance. Using the number of true positive (TP), true negative (TN), false positive (FP) and false negative (FN) we can calculate the following:

  - Accuracy: The proportion of correctly classified instances.

$$
\frac{TP + TN}{TP + FP + TN + FN}
$$

  - Precision: The proportion of predicted positives that are actually positive.

$$
\frac{TP}{TP + FP}
$$
  
  - Recall (Sensitivity): The proportion of actual positives that are correctly identified.

$$
\frac{TP}{TP + FN}
$$

 - F1 score: The weighted mean of precision and recall.


In sklearn, we can calculate these as follows:

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

# Accuracy
print("Accuracy:", accuracy_score(y_test, cc_lr.predict(X_test)))

# Precision
print("Precision:", precision_score(y_test, cc_lr.predict(X_test)))

# Recall
print("Recall:", recall_score(y_test, cc_lr.predict(X_test)))

# F1 Score
print("F1 Score:", f1_score(y_test, cc_lr.predict(X_test)))
```
```python
Accuracy: 0.88  
Precision: 0.93  
Recall: 0.87  
F1 Score: 0.90
``` 
