# MLSS 2020 - Bayesian Inference

## Part 1 - Learning

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

**Bayes Rule**

Allows you to invert conditional probabilities.

$$
p(z|x) = \frac{p(x|z)p(z)}{p(x)} = \frac{p(x \cap z)}{p(x)} = \frac{p(x|z)p(z)}{\int p(x, z)dz}
$$

* Posterior $$p(z|x)$$: probability of z given x
* Likelihood $$p(x|z)$$: probability of x given z
* Prior $$p(z)$$: knowledge.
* Marginalization $$p(x), \int p(x,z)dz$$: marginal likelihood, model evidence

#### Parameterization

Theta $$\theta$$represents quantities, i.e. parameters, that we want to optimize.$$p_\theta(x|z) \equiv p(x|z; \theta)$$

#### Expectation

The expectation under some distribution $$p_\theta$$ of some quantity$$f$$ corresponds to averaging the function $$f$$under the distribution $$p$$by integration.$$\mathbb{E}_{p_{\theta}(x|z)}[f(x;\phi)] = \int p_\theta(x|z)\ f(x;\phi)\ dx$$

**Gradient**  
$$\nabla_\phi f(x; \phi) = \frac{\partial f(x;\phi)}{\partial \phi}$$

### Probability of a Sequence

#### Exchangeable Sequence of Events

Under a condition of **infinite exchangeability** of events, the joint probability is invariant to permutation of the indices. $$p(x_1, ..\ , x_n) = p(x_{\pi_1}, ..\ , p_{\pi_n})$$.   
_Examples: coin flip, drawing balls from an urn, some queuing systems_

> An exchangeable ****\(a.k.a. interchangeable\) sequences of random variables is a finite or infinite sequence of r.v. such that for any finite permutation of the indices the joint probability of the permuted sequence is the same as the joint probability distribution of the original sequence.

Formally, we can invoke **De Finetti's Theorem**. 

\*\*\*\*$$p(x_1, \ .. \ , x_N) = \int \Pi^N_{n=1}p(x_n| \theta)P(d\theta) =  \int \Pi^N_{n=1}p(x_n| \theta)p(\theta)d\theta$$\*\*\*\*

![](.gitbook/assets/image%20%2842%29.png)

We are thus working on a **model-based** approach since we represent the sequence using a parameterized model.

### Bayes Analysis

> Bayesian approach follows the idea that all components of a model should be probabilistic and can be described by probability distributions

Recalling the Bayes' Rule

$$
p(z|x) = \frac{p(x|z)p(z)}{\int p(x,z)dz}
$$

Bayesian analysis is an approach to modelling that follows:

1. Decide on _a priori_ beliefs
2. Posit an explanation of how the observed data is generated, i.e. provide a probabilistic description
3. Allows recursive updating in the light of new evidence

> In Bayesian analysis, things that are not observed must be integrated over, i.e. averaged out.

We are interested in reasoning about two quantities

* **Evidence** $$p(y|x)$$
* **Posterior** $$p(\theta | y, x)$$\*\*\*\*

## **Part 2 - Computation**

### **Hierarchical Model**

We define **hierarchical models** as models where the prior probability distributions can be decomposed into a sequence of conditional distributions

$$
p(z) = p(z_1|z_2)p(z_2|z_3)...p(z_{L-1}|z_L)
$$

### Likelihood Functions

In a **probabilistic model**, we specify the probability f the data we observe given some parameters $$p(y|x)=p(y|h(x);\theta)$$.   
The **Likelihood function** represents the likelihood of parameters, such as the log-likelihood $$L(\theta) = \sum\limits_n log\ p(y_n|x_n;\theta)$$. 

Prescribed likelihoods are:  
\* statistically efficient estimators for the parameters  
\* tests with good power \(able to construct small confidence regions\)  
 \* able to pool information \(e.g. different datasources, domain knowledge\)  
\* inefficient in the case of misspecification  
\* widely applicable \(e.g. correct biases, incomplete data\)

#### Maximum Likelihood

Learning the parameters with Max Likelihood as the optimization objective $$arg\  \underset{\theta}{max}\  L(\theta)$$is straightforward, natural way but it can be biased in finite sample size and may easily lead to overfitting the parameters.

#### Regularization

To overcome the limitations of MLE, we add a regularization term to the likelihood function

$$
L(\theta) = \sum\limits_n log\ p(y_n|x_n;\theta)\ +\ \frac{1}{\lambda}R(\theta)
$$

and with the **Maximum a Posteriori \(MAP\)** we should estimate this modified likelihood function which introduces the penalized term.

