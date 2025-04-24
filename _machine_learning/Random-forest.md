---
layout: single
weight: 3
title: "Random forest"
excerpt: "Combining multiple decision trees to improve accuracy"
header:
    overlay_image: assets/images/Randon forest cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Random forest cover.png
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

# Random forest
Decision trees can be a useful tool for classification, however, there are limitations which can lead to sub-optimal results. One way to increase the effectivness of our model is to make a forest full of decision trees, otherwise known as a random forest. 

In order to make sure we find our optimal result, when creating each tree, we can put on constraints, forcing the tree to branch on a smaller subset of variables. This forces the decision trees to find different solutions to splitting the data. Once we have a forest full of trees, we classify our data with each one. The classification output that occurs most frequently from our forest is the result. 

## Bagging
When creating a decision tree, we learnt it is deterministic, thus, with the same training data, the tree will always be the same. 

In order to circumvent this random forests can utlise a process called bagging. This process splits the training set into multiple subsets by which different decision trees can be created. For example, if we had 1000 rows of training data, we can make decision trees using 100 rows selected at random. Each time a random subset is created, a random row is selected from the full training set. This means we can end up with the same row multiple times across subsets and even in the same subset. Even if the subset size was large, its likely each subset would still be unique.

The code below demonstrates how this can be done. If we wanted to create more unique training sets we can encase the code in a loop, saving each data and labels subset each time. 

```python
import random

# Create array of random indexes, with replacement, up to the length of the training set
indices = random.choices(range(len(car_data)), k=len(car_data))

# Use indices to create training sets
data_subset = car_data.iloc[indices].reset_index(drop=True)
labels_subset = car_labels.iloc[indices].reset_index(drop=True)
```

## Bagging features
Bagging our training set provides one way of forcing the tree to look at alternative branches, but we can go one step further and use feature bagging too. 

Currently, our car data uses the following fetures:

  - Fuel type
  - Engine turbocharger
  - Number of doors
  - Number of drive wheels
  - Fuel system

When we create our decision trees, we currently take all features into account when decising how to split the data based off the amount of information gain that can be produced. By feature bagging, we can restict the number of features the tree can use to decide each split. 

For example, when deciding on a feature to split the data on first, we can randomly choose to only consider the fuel type and number of doors. We'll then want to split again using another random subset of features. This is continued until the tree is complete. 

One thing to consider is how to choose the number of features to randomly select. A general rule of thumb is to randomly select the square root of the total number of features. Our car dataset is relatively small therefore limiting the impact of feature bagging. If there we're more features e.g. 16, this would provide a greater diversity in our decision trees as there are many more combinations for bagging. 

To demonstrate this, we've altered our find_best_split() function to randomly select two features instead of using all of the available features. The function then proceeds as before, deciding how to split based on which feature provides the best information gain. 

```python
def find_best_split(dataset, labels):
    best_gain = 0
    best_feature = None
    
    # Choose random column names instead of indices
    features = np.random.choice(dataset.columns, 2, replace=False)
    
    for feature in features:
        data_subsets, label_subsets = split(dataset, labels, feature)
        gain = information_gain(labels, label_subsets)
        if gain > best_gain:
            best_gain, best_feature = gain, feature
            
    return best_gain, best_feature
indices = random.choices(range(len(car_data)), k=len(car_data))

# Use indices to create training sets
data_subset = car_data.iloc[indices].reset_index(drop=True)
labels_subset = car_labels.iloc[indices].reset_index(drop=True)

(best_gain, best_feature) = find_best_split(data_subset, labels_subset)

print(best_gain, best_feature)
```

Running this three times gives us different answers as we randomly select which features to split on. 

```python
0.0024260608618576485 drivewheel
0.0002386232706932112 aspiration
0.013303063384313751 fuelsystem
```

## Classification
Now we have learnt how we can bag and feature bag. We can put this together to create trees for our random forest. To do this, we can use scikit-learn RandomForestClassifier. When creating a RandomForestClassifier, we can define the number of trees by using the n_estimators variable. 

```python
from sklearn.ensemble import RandomForestClassifier

classifier = RandomForestClassifier(n_estimators = 100, random_state=0)

classifier.fit(training_set, training_labels)

print(classifier.score(validation_set, validation_labels))
0.7
```
