---
layout: single
weight: 8
title: "Classification evaluation metrics"
excerpt: "Evaluating a model’s predictive power"
header:
    overlay_image: assets/images/Evaluation metrics cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Evaluation metrics cover.png
toc: true
toc_sticky: true 
published: true
toc_label: "Contents:"
#classes: wide
collection: machine_learning
type: classification
author_profile: True
---

{% include mathjax.html %}

# Evaluation metrics

## Confusion matrix
When creating a machine learning model, how can we check how well it can make predictions? After splitting the data into training and validation sets we can check the model outputs against the true values. 

Let's say we are trying to predict whether a student will pass or fail their final exam. After training a model with a training data set, we can pass a validation data set into the trained model. The model will output a series of predictions which we can then compare to our validation labels. From these comparisons there are four possible categories the results can fall under: 

True Positive (TP): The algorithm predicted pass and it was pass
True Negative (TN): The algorithm predicted fail and it was fail
False Positive (FP): The algorithm predicted pass and it was fail
False Negative (FN): The algorithm predicted fail and it was pass

To understand this better, we can visualise this as a confusion matrix. In the matrix, predicted classes are represented as columns and the actual classess are represented as rows. Ideally, we want the numbers along the main diagonal (true positives and true negatives) to be as high as possible, indicating fewer classification errors.

<p align="center">
  <img src="/assets/images/Confusion matrix.png" alt="Confusion matrix" width="500">
</p>

Lets see this in action:

```python
actual =    [1, 0, 0, 1, 1, 1, 0, 1, 1, 1]
predicted = [0, 1, 1, 1, 1, 0, 1, 0, 1, 0]

true_positives = 0
true_negatives = 0
false_positives = 0
false_negatives = 0

for i in range(len(actual)):
  if actual[i]==1 and predicted[i]==1:
    true_positives += 1
  elif actual[i]==0 and predicted[i]==1:
    false_positives += 1
  elif actual[i]==0 and predicted[i]==0:
    true_negatives += 1
  elif actual[i]==1 and predicted[i]==0:
    false_negatives += 1
  
print(true_negatives, false_positives, false_negatives, true_positives)
0 3 4 3
```

Instead creating our own function, we can utilise scikit's confusion matrix function. 
```python
from sklearn.metrics import confusion_matrix
conf_matrix = confusion_matrix(actual, predicted)
print(conf_matrix)
[[0 3]
 [4 3]]
```
## Accuracy
The first metric we can use is the accuracy score. Accuracy is calculated by taking the total number of correctly classified predictions (true positives and true negatives) and dividing by the total number of predictions.

$$
\text{Accuracy} = \frac{\text{TP} + \text{TN}}{\text{TP} + \text{FP} + \text{TN} + \text{FN}}
$$

```python
accuracy = (true_positives+true_negatives)/(true_positives+true_negatives+false_positives+false_negatives)
print('Accuracy:', accuracy)
```
```python
Accuracy: 0.3
```

## Recall
Accuracy can sometimes be a misleading measure depending on the context of our data. Take our student test pass/fail example. If we created a model to predict whether students in a high-performing class would pass or fail, we’d expect most to pass. A model that always predicts “pass” would be highly accurate, but it would fail to identify the students at risk of failing — those who may need extra support.

In this situation, a helpful statistic to consider is recall. Specifically, if we care about catching failing students, recall measures the proportion of actual failing students that our model correctly identifies. This gives us a better sense of how well we're detecting the minority class.

$$
\text{Recall} = \frac{\text{TP}}{\text{TP} + \text{FN}}
$$

```python
recall = true_positives/(true_positives+false_negatives)
print('Recall:', recall)
```
```python
Recall: 0.43
```

## Precision
Unfortunately, recall isn’t a perfect statistic either. Suppose we created a model that predicts many students as likely to fail — just to make sure we don’t miss any. While this might give us a high recall (we catch most of the failing students), we might also be incorrectly labeling many students who would actually pass.

This is where precision becomes useful. Precision tells us, of all the students we predicted would fail, how many actually did fail. It helps us understand how trustworthy our "fail" predictions are.

$$
\text{Precision} = \frac{\text{TP}}{\text{TP} + \text{FP}}
$$

```python
precision = true_positives/(true_positives+false_positives)
print('Precsion:', precision)
```
```python
Precsion: 0.5
```

## F1 score
Instead of relying on a single measure, we can combine both precision and recall in a single measure. The F1-score uses the harmonic mean of both measures to better describe the model's efffectiveness. 

F1-score is defined as:

$$
F_1 = \frac{2 \cdot \text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}
$$

```python
f_1 = (2*(precision*recall))/(precision+recall)
print('f1-score:', f_1)
```
```python
f1-score: 0.46
```

### Harmonic mean
We use the harmonic mean rather than the arithmetic mean because we want the F1-score to be low when either precision or recall is low. This makes it a more balanced indicator of overall model performance.

Suppose we have a classifier with:

  - Recall = 1.00 (very high)
  - Precision = 0.02 (very low)

Despite the perfect recall, this classifier is likely problematic because it makes many incorrect positive predictions.

If we calculate the arithmetic mean:

$$
\frac{1 + 0.02}{2} = 0.51
$$

That’s misleadingly high — it doesn’t reflect how poor the precision is. Now compare that to the harmonic mean (F1-score):

$$
\frac{2 \cdot 1 \cdot 0.02}{1 + 0.02} = 0.039
$$

The F1-score appropriately reflects the weakness of the model — it's only as good as its weakest link (precision in this case).
 
## sklearn
Now that we have an understanding of accuracy, precision, recall, and F1-score, we can assess which metric best reflects our model’s effectiveness.

In this case, we want to ensure that students who are at risk of failing are identified, so we can give them additional support. It's okay if some students are incorrectly flagged as at-risk (i.e., false positives), but we don't want to miss those who truly need help.

This means that recall is especially important in our context — we want to minimise false negatives.

As long as we have an understanding of the context, we should be able to determine which statistic is most relevant.

To make this easier going forward, we can utilise the sklearn metrics library. 

```python
from sklearn.metrics import accuracy_score, recall_score, precision_score, f1_score

actual = [1, 0, 0, 1, 1, 1, 0, 1, 1, 1]
predicted = [0, 1, 1, 1, 1, 0, 1, 0, 1, 0]

print('Accuracy:', accuracy_score(actual, predicted))
print('Recall:', recall_score(actual, predicted))
print('Precision:', precision_score(actual, predicted))
print('F1-score:', f1_score(actual, predicted))
```
```python
Accuracy: 0.3
Recall: 0.43
Precision: 0.5
F1-score: 0.46
```


