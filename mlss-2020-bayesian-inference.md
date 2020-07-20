# MLSS 2020 - Bayesian Inference

## Part 1

### Probability

The definition of probability is multi-fold. We can talk about:

* **Statistical Probability:** frequency ratio of items
* **Logical Probability:** degree of confirmation of a hypothesis based on logical analysis
* **Probability as Propensity**: probability used for predictions
* **Subjective Probability:** probability as a degree of belief. 
  * Measurement of the belief in a proposition given evidence. 
  * A description of a state of knowledge.

> Generally, probability is sufficient for reasoning under uncertainty.

Thus, there is no such thing as **the probability** of an event since the value depends on the evidence. The probability is thus inherently subjective depending on the believer's information - two observers with same information will have same probability. On the flip side, different observers with different information will have different beliefs.

### Probabilistic Models

In DS/ML environment, a **model** is a description of the world, data, potential potential scenarios, processes. A **probabilistic model** then writes builds such models using the language of probability, allowing you to learn probability distributions of data.

### Probabilistic Quantities

#### Probabilities $$p(x) \quad p^*(x) \quad q(x)$$\*\*\*\*

#### Conditions $$p(x)\geq 0 \quad \int p(x) d(x) = 1$$

#### **Bayes Rule**

Allows you to invert conditional probabilities.  $$p(z|x) = \frac{p(x|z)p(z)}{p(x)}$$. Composed by:

* Posterior $$p(z|x)$$: probability of z given x
* Likelihood $$p(x|z)$$: probability of x given z
* Prior $$p(z)$$: knowledge.
* Marginalization $$p(x)$$

#### Parameterization





