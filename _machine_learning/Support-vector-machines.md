---
layout: single
weight: 9
title: "Support vector machines (SVM)"
excerpt: "Utilising kernels for margin-based classification"
header:
    overlay_image: assets/images/SVM_rbf_cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/SVM_rbf_cover.png
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

# Support Vector Machine (SVM)
A Support Vector Machine (SVM) is a machine learning model used primarily for binary classification tasks. It works by finding a decision boundary that separates data points into two classes, while maximising the margin between the nearest points.

While SVMs can use multiple features to build a model, it's easier to visualise when we limit ourselves to just two features.

To demonstrate how SVMs work, we'll use Fisher's Iris dataset. This classic dataset includes four features: the length and width of the sepals and petals, which can help distinguish between three species of Iris flowers.

Below, we have plotted the sepal length versus sepal width for two species — Setosa and Versicolor. An approximate linear decision boundary is also plotted. New flowers can be classified based on their position relative to this line: if a point falls above the line, it will be classified as a Setosa; if it falls below, it will be classified as a Versicolor.

<p align="center">
  <img src="/assets/images/SVM_bivariable_plot.png" alt="SVM_bivariable_plot" width="500">
</p>

We can model more than two features, however, the more we add, the harder it becomes to visualise. For three features, we'd use a 3D plane, after that, it becomes a lot more complex!

## Support vectors and margins
To give the model the greatest chance of success, we need to optimise the placement of our decision boundary. There are infinitely many ways we could separate our features, but we want the boundary that gives us the highest chance of correct classification. This means placing the boundary as far away as possible from the nearest data points from each class.

To achieve this, SVM identifies specific data points — called support vectors — that are closest to the decision boundary. These points "support" the boundary: they determine its exact position and orientation. In two dimensions, at least three support vectors are typically needed to define a boundary, although in real-world datasets, there are often more. The key idea is that only these support vectors matter when constructing the SVM decision boundary — not the rest of the data points. This makes SVM one of the most efficient machine learning models, as it only needs to use the support vectors to train. 

Below, we can see the support vectors from our original example. See how only the closest points to the decision boundary are supported.

<p align="center">
  <img src="/assets/images/SVM_support_vectors.png" alt="SVM_support_vectors" width="500">
</p>

The distance between the decision boundry and the support vectors is known as the margin. In order to have the highest success rate of classification, we want to maximise the size of the margin. 

<p align="center">
  <img src="/assets/images/SVM_margin.png" alt="SVM_margin" width="500">
</p>

## scikit-learn
In previous articles, we have created our own functions to demonstate the workings of the scikit machine learning models. However, as we move onto more complex machine learning models, this becomes much harder! Whilst, we wont be building our own function, we will explore what the scikit SVM model is doing with our data. The objective of the model being to optimise the placement of the decision boundary to give us the largest margin.

Similarly to the other machine learning models, we need to create a SVC object. Note the slight change in name, the 'C' standing for Classifier instead of Machine. 

```python
from sklearn import datasets
from sklearn.svm import SVC

# Load the Iris dataset and filter to only Setosa and Versicolor
iris = datasets.load_iris()
mask = (iris.target == 0) | (iris.target == 1)
X = iris.data[mask][:, :2]  # Only take sepal length and width
y = iris.target[mask]

# Train a linear SVM
model = SVC(kernel='linear') 
model.fit(X, y)
```

You may notice we have also defined a kernel whilst creating the SVC model. The linear model selected will attempt to fit a straight line through our datapoints. We'll cover different kernels and when to use them later. We have fitted our model to the datapoints in the Iris dataset but have masked one of the flowers so we only have two classifications: Setosa and Versicolor. Each of these classifications is represented by either a 0 or 1, if we wanted to add in the reaming flower to our dataset it would be represented by the label 2.

Using our model, we can now predict the flower based upon the sepal length and width:

```python
print(model.predict([[5.5, 4.0], [6.0, 3.0], [6.5, 3.5]]))
[0 1 1]
```

Our model has given us classifications of Setosa, Versicolor, Versicolor. A quick check of these datapoints against the plot above provides confidence these classifications are correct. 

## Outliers
The example we have explored so far has two distinct groupings based on the speal length and width, but what happends when our data isn't so clear cut? For example, if we plot the petal width v petal length of the Versicolor and Virginica, we can see the two groupings have some overlap. 

<p align="center">
  <img src="/assets/images/SVM_outliers.png" alt="SVM_outliers" width="500">
</p>

To allow the SVM model to fit to the data, we can define how strict we would like the margin critera to be. To define this, the SVC model has perameter $\text{C}$ which instructs the model how much error should be allowed during fitting. A large $\text{C}$ value will create a hard margin with no or few data points inside. This may cause the margin to be very small and runs the risk of model overfitting to the training data. On the flipside, a small $\text{C}$ allows for many points to be inside the margin. This lessens the influence of outliers but runs the risk of underfitting.

Lets take our example from above and compare the results using two different $\text{C}$ values. Using a large $\text{C}$ value to create a hard margin, the model used fewer support vectors and misclassified 6 data points. With a small $\text{C}$ value, the moded had a soft margin utilising 25 support vectors from each flower resulting in only 5 misclassifications. 

{:.text-center}
|                          |Hard margin|Soft margin|
|:------------------------:|:---------:|:---------:|
|C value                   |10,000     |0.1        |
|Versicolor support vectors|6          |25         |
|Virginica support vectors |6          |25         |
|Misclassifications        |6          |5          |

<p align="center">
  <img src="/assets/images/SVM_highC.png" alt="SVM_hard_margin" width="500">
</p>

<p align="center">
  <img src="/assets/images/SVM_lowC.png" alt="SVM_soft_margin" width="500">
</p>

## Kernels
We have so far focused only on splitting our data linearly. But what happens if there is no clear seperation of our datapoints no matter how soft the margin?

We can still use the SVM to create a decision boundry, however we need to utilise a different kernel. Kernels transform the datapoints in order to create distinguishable groups that can be seperated linearly. There is some clever maths behind this that we wont get into in this article, however having an understanding of what the kernels are doing and the differences in their behavious will help us select the most appropriate kernel for our datasets. 

### Polynomial
The polynomial kernel enables the model to create non-linear decision boundaries. Instead of drawing a straight line like the linear kernel, the polynomial kernel maps the data into a higher-dimensional space, where the classes may become linearly separable. This transformation allows the SVM to fit more complex boundaries.

In simple terms, you can visualise the kernel as "lifting" the data into a curved space where a flat plane (decision boundary) can now cleanly divide the data points that were mixed in 2D.

Below is an example of a polynomial kernel splitting the Versicolor and Virginica flowers as we did above with a linear kernal. Compared to a linear kernel, this model doesn't necessarily reduce the number of misclassifications, but it achieves the decision boundary using fewer support vectors.

This can suggest a more efficient margin, but it's important to be cautious: increasing the polynomial degree or the regularisation parameter $\text{C}$ too much can lead the model to overfit.

{:.text-center}
|                          |Polynomial |
|:------------------------:|:---------:|
|Degree                    |3          |
|C value                   |1          |
|Versicolor support vectors|7          |
|Virginica support vectors |8          |
|Misclassifications        |5          |

<p align="center">
  <img src="/assets/images/SVM_polynomial_margin.png" alt="SVM_polynomial" width="500">
</p>

### Radial basis function (RBK)
For more complex or non-linearly separable data, we can use the Radial Basis Function (RBF) kernel. This is one of the most popular kernels used in SVM's, and it's also the default kernel in the SVC classifier.

Unlike the polynomial kernel, which transforms data into 3D, the RBF kernel maps the input features into an infinite-dimensional space. This transformation allows the model to draw highly flexible and curved decision boundaries.

It can be difficult to visualise how the RBF kernel transforms the data, but its power lies in its ability to create boundaries that closely wrap around complex clusters of data.

#### Gamma Parameter
A key hyperparameter for the RBF kernel is gamma ($\gamma$). Gamma determines how far the influence of a single training example reaches:

  - A high gamma value makes the model very sensitive to individual data points, which can lead to overfitting.

  - A low gamma value causes the model to consider points further away, resulting in a smoother, more generalised boundary.

While $\text{C}$, the regularization parameter, controls the trade-off between achieving a low training error and maintaining a smooth decision boundary, gamma specifically controls the curvature of the decision boundary itself.

In the plot below, we use an SVM with an RBF kernel to separate Versicolor and Virginica flowers using only the sepal length and width. These two species overlap significantly in this feature space, making linear or even polynomial decision boundaries less effective.

By using a $\text{C}$ value of 14 and a gamma of 8, the RBF kernel is able to fit a flexible boundary that better distinguishes between the two classes.

<p align="center">
  <img src="/assets/images/SVM_rbf.png" alt="SVM_rbf" width="500">
</p>

Whilst we could tune the model further to fit the data more strictly, we would risk overfitting to the training data. The current model fit captures the main groupings of data without focusing too heavily on individual data points. 
