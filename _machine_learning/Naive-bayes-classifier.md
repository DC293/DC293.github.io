---
layout: single
weight: 10
title: "Naive Bayes Classifier"
excerpt: "Classifier using Bayes' theorem and probabilities."
header:
    overlay_image: assets/images/Naive_bayes_cover.png
    overlay_filter: 0.5 # Optional: Adds a dark filter to improve readability
    teaser: assets/images/Naive_bayes_cover.png
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

# Bayes Theorem
Bayesian statistics are a branch of statistics that use the probability of prior events to calculate new probabilities. Of particular importance is Bayes Theorem. Before we get into how we can use Bayes Theorem to classify, we'll briefly run through some simple statistical principles. 

## Event dependancy
Events are a particular task by which we attribute a probability. For example, flipping a coin or rolling a dice. Series of events can either be classed as dependant or independant. For an event to be independant, its occurance does not affect the probability of another event. For example, Andy Murray wins Wimbledon; Liverpool win the Premier League. If two events are dependant, the probability of one event occuring changes in a predictable way depending on the outcome of the prior event. For example, I hit the fairway; I make par. 

## Conditional probability
Conditional probability is the probability that two events occur. The simplest calculation of conditional probability is when we have two independent events. In this instance, the probability of both events occuring is the product:

$$
P(A \cap B) = P(A) \cdot P(B)
$$

For example, if Andy Murray has a 1/10 chance of winning Wimbledon and Liverpool have a 1/7 chance of winning the premier league, the probability of both occuring is:

$$
\frac{1}{10} \cdot \frac{1}{7} = \frac{1}{70}
$$

However, when events are dependent, the outcome of one event affects the probability of the other. For instance, imagine there's a:
  - 1 in 2 chance of hitting the fairway.
  - 1 in 3 chance of making par if you hit the fairway.
  - 1 in 5 chance of making par if you land in the rough.

To find the overall probability of making par, you would sum:
  - The probability of hitting the fairway and making par. $P(par|fairway) = \frac{1}{2} \cdot \frac{1}{3} = \frac{1}{6}$
  - The probability of missing the fairway and making par. $P(par|rough) = \frac{1}{2} \cdot \frac{1}{5} = \frac{1}{10}$

Therefore:

$$
P(par) = \frac{1}{6} + \frac{1}{10} = \frac{4}{15}
$$

This reflects how conditional probability adapts based on the situation or prior outcome.
