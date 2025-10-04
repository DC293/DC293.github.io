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
collection: machine_learning
type: classification
author_profile: True
---

{% include mathjax.html %}

# Naive Bayes Classifier
Bayesian statistics are a branch of statistics that use the probability of prior events to calculate new probabilities. Of particular importance is Bayes Theorem. Before we get into how we can use Bayes Theorem to classify, we'll briefly run through some simple statistical principles. 

## Event dependancy
Events are a particular task by which we attribute a probability. For example, flipping a coin or rolling a dice. Series of events can either be classed as dependant or independant. For an event to be independant, its occurance does not affect the probability of another event. For example, Andy Murray wins Wimbledon; Liverpool win the Premier League. If two events are dependant, the probability of one event occuring changes in a predictable way depending on the outcome of the prior event. For example, I hit the fairway; I make par. 

## Conditional probability
Conditional probability is the probability of one event occurring given that another has already occurred. When two events are independent, the probability of both occurring is the product of their individual probabilities:

$$
P(A \cap B) = P(A) \cdot P(B)
$$

For example, if Andy Murray has a 1/10 chance of winning Wimbledon and Liverpool have a 1/7 chance of winning the premier league, the probability of both occuring is:

$$
\frac{1}{10} \cdot \frac{1}{7} = \frac{1}{70}
$$

However, when events are dependent, the outcome of one affects the likelihood of the other. For instance, consider the following example:

  - 2 in 3 chance of hitting the fairway.
  - 1 in 3 chance of making par if you hit the fairway.
  - 1 in 5 chance of making par if you land in the rough.

To calculate the overall probability of making par, we add the probabilities of the two scenarios:

Hitting the fairway and making par:

$$P(par \cap fairway) = \frac{2}{3} \cdot \frac{1}{3} = \frac{2}{9}$$

Landing in the rough and making par:

$$P(par \cap rough) = \frac{1}{3} \cdot \frac{1}{5} = \frac{1}{15}$$

Therefore the overall chance of hitting par is:

$$
P(par) = \frac{2}{9} + \frac{1}{15} = \frac{13}{45}
$$

## Bayes' Theorem
Above, we determined the overall probability of hitting par. We could also see that there is a much higher chance of hitting par if we first land on the fairway. But what if we wanted to know the odds that we hit the fairway given we made par? For this, we can use Bayes' Theorem: 

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

In our golf example this would mean:

$$
P(fairway \mid par) = \frac{P(par \mid fairway) \cdot P(fairway)}{P(par)} \\
\\
= \frac{\frac{1}{3} \cdot \frac{2}{3}}{\frac{13}{45}} = \frac{2}{9} \cdot \frac{45}{13} = \frac{10}{13}
$$

So, if a player makes par, there's a 10 in 13 chance they hit the fairway.

## Smoothing

In probability calculations, especially with small datasets, it is possible to encounter situations where a particular event has never occurred in the data. This leads to a probability of zero, which can cause problems when multiplying probabilities in the Naive Bayes classifier (since multiplying by zero will always result in zero). To address this, we use a technique called <strong>smoothing</strong> (or Laplace smoothing), where we add 1 to each count to ensure no probability is ever exactly zero.

### Example: Golf Scenario

Suppose in our golf example, we have observed the following outcomes over a round:


| Fairway | Par | Count |
|---------|-----|-------|
| Yes     | Yes |   5   |
| Yes     | No  |   7   |
| No      | Yes |   0   | <!-- This is where smoothing is needed -->
| No      | No  |   6   |

Notice that there were no instances where a player missed the fairway but still made par ("No" Fairway, "Yes" Par = 0).

If we want to calculate the probability of making par given that the player missed the fairway ("No"), without smoothing, it would be:

$$
P(par \mid \text{no fairway}) = \frac{0}{0+6} = 0
$$

This zero probability is problematic for Naive Bayes, as it would eliminate any chance of this outcome in future predictions.

With smoothing, we add 1 to each count:

$$
P(par \mid \text{no fairway}) = \frac{0+1}{(0+6)+2} = \frac{1}{8}
$$

Here, we add 1 to the numerator and add the number of possible outcomes (2: 'Yes' and 'No') to the denominator. This ensures that even if an event was not observed, it still has a small, non-zero probability.

Smoothing is especially important in real-world datasets where some combinations of features and outcomes may be rare or missing entirely.

## Classifing

### Worked Example: Classifying with Bayes’ Theorem (Golf Scenario)

Suppose you want to predict the probability that a player makes par, given that they missed the fairway on a hole. Using the smoothed probabilities from the table above:

| Fairway | Par | Count |
|---------|-----|-------|
| Yes     | Yes |   5   |
| Yes     | No  |   7   |
| No      | Yes |   0   |
| No      | No  |   6   |

**Step 1: Calculate Prior Probabilities**

- Probability of missing the fairway:
  $$
  P(\text{no fairway}) = \frac{0 + 6}{18} = \frac{6}{18} = \frac{1}{3}
  $$
- Probability of making par overall:
  $$
  P(\text{par}) = \frac{5 + 0}{18} = \frac{5}{18}
  $$

**Step 2: Calculate Likelihood with Smoothing**

- Probability of making par given missed fairway (with smoothing):
  $$
  P(\text{par} \mid \text{no fairway}) = \frac{0+1}{(0+6)+2} = \frac{1}{8}
  $$
- Probability of not making par given missed fairway (with smoothing):
  $$
  P(\text{not par} \mid \text{no fairway}) = \frac{6+1}{(0+6)+2} = \frac{7}{8}
  $$

**Step 3: Apply Bayes’ Theorem**

To find the probability that a player made par given they missed the fairway:

$$
P(\text{par} \mid \text{no fairway}) = \frac{P(\text{no fairway} \mid \text{par}) \cdot P(\text{par})}{P(\text{no fairway})}
$$

- $P(\text{no fairway} \mid \text{par})$ is the probability of missing the fairway given a par was made:
  $$
  P(\text{no fairway} \mid \text{par}) = \frac{0+1}{(5+0)+2} = \frac{1}{7}
  $$
  (Here, numerator is the smoothed count for [No, Yes], denominator is total smoothed par count.)

- $P(\text{par}) = \frac{5}{18}$ (from above)
- $P(\text{no fairway}) = \frac{6}{18} = \frac{1}{3}$

Plug in the values:

$$
P(\text{par} \mid \text{no fairway}) = \frac{\frac{1}{7} \cdot \frac{5}{18}}{\frac{1}{3}} = \frac{1}{7} \cdot \frac{5}{18} \cdot \frac{3}{1} = \frac{15}{126} = \frac{5}{42}
$$

**Interpretation:**  
Given a player missed the fairway, the (smoothed) probability that they still made par is $\frac{5}{42}$.






