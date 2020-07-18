---
description: >-
  Notes on the keynotes presented by Bernhard Schölkopf during the Machine
  Learning Summer School 2020. https://www.youtube.com/watch?v=btmJtThWmhA
---

# MLSS 2020 - Causality

## Part 1

> The causal generative process is composed of autonomous modules that do not inform or influence each other.

### Interventions

The relationship between altitude \($$A$$\) and temperature \($$T$$\) provides a good way to understand the concept of interventions and their part in Causality. We expect to have a negative correlation between the variables \(even from Physics 101\) since higher altitudes are expected to have lower temperatures. But can we say the same for the reverse? Do lower temperatures cause a rise in the altitude? Or do they have a cofounding factor that causes both?

$$
\begin{matrix}  p(a,t) & = p(a|t) p(t)  & T \rightarrow A\\ &= p(t|a)p(a)  & A \rightarrow T\end{matrix}
$$

To understand the causality relationship, we need to make an intervention. An intervention can be described as a manipulation of the values of a variable \(and not the variable itself, i.e.$$a$$, not $$A$$\). 

Examples of intervention:

* $$a$$: raise the city, find that $$t$$ changes; _e.g. an elevator for the weather station_
* $$t$$: raise the temperature, find if $$a$$ doesn't change; _e.g. a giant heater around the weather station_

During this hypothetical thought experiment, we are expecting the altitude to be a causing factor for the change in the temperature, but not the other way around. As such, we are expecting $$p(t|a)$$to be invariant accross stations in similar climate zones. Given such altitude, the probability distribution for the temperatures in similar wheather conditions should be similar. Additionally, we are expecting the mechanism $$p(t|a)$$to be independent of $$p(a)$$, i.e. the mechanism that determines the temperature given the altitude should work independently of the mechanism that generates the altitude.

### Reichenbach's Common Cause Principle

> If X and Y are dependent, that means it exists a common cofounder Z which causally influences both. As such, Z screens X and Y from each other, i.e. given Z, X and Y are independent.

Additionally, we can extend such concept so that the Z coincides with either X or Y to allow the cases where one variable is a cause for the other. Interestingly, we cannot distinguish between these 3 cases \(cofounded, $$X \rightarrow Y$$, $$Y \rightarrow X$$\) since they all lead to the same observable consequences $$P(X, Y)$$.

![Screenshot for Common Cause Principle](.gitbook/assets/image%20%2836%29.png)

### Structural Causal Model

The relationships between variables can be represented by a SCM. This model is a DAG with vertices as observables and arrows as direct causation, where each variable is modeled by the influence of its parents plus an unexplained variable \(aka noise\) so that $$X_i := f_i(PA_i, U_i).$$



![Example of a SCM](.gitbook/assets/image%20%2834%29.png)

#### SCMs, Reichenbach and Causal Sufficiency

The noise variables are considered independent on the basis that if they were dependent on a cofounder, the Reichenbach's principle would tell us that the causal graph is incomplete.

#### Entailed Distribution

_So, how do we convert this into a probabilistic distribution?_

Without loops and since everything besides the noise variables is deterministic, we can recursively substitute the noise variables, evaluate the parent \(deterministic\) relationships to get the variable as a function of the noise terms$$X_i := g_i(U_1, .., U_n)$$. Since each $$X_i$$is a RV, we will get a joint distribution of $$X_1, ... X_n$$ called the _observational distribution._

### Markov Conditions

#### Causal Conditional

A.k.a. Causal Markov Kernel. The distribution of a random variable conditioned on the distribution of its parents $$p(X_i | PA_i)$$, corresponding to the structural equation $$X_i := f_i(PA_i, U_i)$$.

This can be used to factorize the join distribution $$p(X_1, .. , X_n) = \Pi_i \ p(X_i | PA_i)$$

### Graphical Causal Inference

> How can we recover the graph $$G$$from a single observational distribution $$p$$? By conditional independence testing, infer a class containing the correct $$G$$

The Markov condition states $$(X \perp \!\!\! \perp Y  | Z)_G  \Rightarrow (X \perp \!\!\! \perp Y  | Z)_p$$, but we need **faithfulness** $$(X \perp \!\!\! \perp Y  | Z)_G  \Leftarrow (X \perp \!\!\! \perp Y  | Z)_p$$.

### Interventions and Shifts

**Definition.** Replacing $$X_i := f_i(PA_i, U_i)$$with another assignment $$(e.g. \ X_i:=const)$$is called an intervention on $$X_i$$.The entailed distribution is now called the interventional distribution.

### Independent Mechanisms

1. The factorization $$p(X_1, .. , X_n) = \Pi_i \ p(X_i | PA_i)$$ exists since noise terms are **independent**.
2. Interventions are feasible if the **mechanisms** \(i.e. causal conditionals\) are **independent**: changing one $$p(X_i| PA_i)$$does not change the conditionals $$p(X_j| PA_j) \text{ for } i \neq j$$, they remain **invariant**. If both 

If both are satisfied, we call this a **disentangled causal factorization**.

### Modelling Taxonomy

<table>
  <thead>
    <tr>
      <th style="text-align:left">Properties, Models</th>
      <th style="text-align:center">Statistical</th>
      <th style="text-align:center">Causal</th>
      <th style="text-align:center">
        <p>Differential</p>
        <p>Equation</p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">i.i.d. prediction, pattern recognition, <em>generalization</em>
      </td>
      <td style="text-align:center">y</td>
      <td style="text-align:center">y</td>
      <td style="text-align:center">y</td>
    </tr>
    <tr>
      <td style="text-align:left">Predict under shift &amp; intervention, <em>horizontal generalization</em>
      </td>
      <td style="text-align:center">n</td>
      <td style="text-align:center">y</td>
      <td style="text-align:center">y</td>
    </tr>
    <tr>
      <td style="text-align:left">Provide physical insight, understand predictions</td>
      <td style="text-align:center">n</td>
      <td style="text-align:center">(y)</td>
      <td style="text-align:center">y</td>
    </tr>
    <tr>
      <td style="text-align:left">Think/Reason, &quot;act as an imagined space&quot; (K. Lorenz)</td>
      <td
      style="text-align:center">n</td>
        <td style="text-align:center">?</td>
        <td style="text-align:center">?</td>
    </tr>
    <tr>
      <td style="text-align:left">Learn from data</td>
      <td style="text-align:center">y</td>
      <td style="text-align:center">(y)</td>
      <td style="text-align:center">n</td>
    </tr>
  </tbody>
</table>



### Pearl's do calculus

The goal of causality is to infer the effect of observations. A distribution of Y given that X is set to x can be computed from $$p$$ and $$G$$ and is given by $$p(Y | do\ X = x) \text{ or } p(Y| do\ x)$$, which **should not be confused with conditional distribution** $$p(Y|X)$$. _Their distinction can be though as the difference between seeing and doing._

* $$p(Y|X)$$: probability an MLSS participant can get a NeurIPS paper accepted
* $$p(Y| do \ x)$$: probability that anyone can get a NeurIPS paper accepted after being made to participate in MLSS

#### Computing join probability under do calculus

To calculate the joint probability conditioned on the do operation$$p(X_1, .. ,X_n | do \ x_i)$$, we can use the previously presented causal factorization and replace the $$p(X_i|PA_i) \text{ with } \delta_{X_ix_i}$$, so that:

$$
p(X_1, .. ,X_n | do \ x_i) := \displaystyle\prod_{j\neq i} p(X_j| PA_j)\delta_{X_ix_i}
$$

and then integrate \(i.e. sum over $$x_i$$\) to get the joint probability so that it no longer depends on $$X_i$$

$$
p(X_1, .. ,X_{i-1}, X_{i+1}, .., X_n | do \ x_i) := \displaystyle\prod_{j\neq I} p(X_j| PA_j(x_i))
$$

and obtain $$p(X_k| do\ x_i)$$by marginalisation. 

### Comparison of Do Probability and Conditional Probability

![Some examples where do- and conditional-probability are the same](.gitbook/assets/image%20%2835%29.png)

![Examples where conditional- and do-probability are NOT the same](.gitbook/assets/image%20%2837%29.png)



### Causal Inference Method

_Assuming a 2 random variables X and Y, how can we determine the causal relationship? Is X causing the values of Y or the other way around?_

By using an **Additive Noise Model** $$Y=f(X) + N$$, we limit the complexity by which the noise acts upon the general functional model $$Y=f(X,N)$$by saying that it acts in an additive manner. So now, our ANM formula is $$Y:=f(X)+N_Y\ \text{, with } X \perp \!\!\! \perp N_Y$$, which is saying that Y is causally modeled by X plus some noise, and the causer X is independent of the noise \(again, if there was some cofounding variable causing X and Ny our graph would be incomplete\). Now we introduce the concept of **Identifiability**: when is there a backward model of the same form?

> Prefer the causal direction that can better be fit with an additive noise model

We can use a regression to fit Y as a function of X, compute the residuals $$N_Y := Y - f(X)$$ and check if they are independent statistically independent with X \(uncorrelation is not sufficient\).

### Independence-based Regression

The classical regression methods assume an underlying distribution for the noise \(e.g. minimize which, if incorrect, may cause the residuals to become dependent. 

> **Independence-based Regression** minimizes the dependence of the residuals instead of maximizing the likelihood of data in the regression objective

Use RKHS distance between kernel mean embeddings/Hilbert-Schmidt-norm of cross-covariance operator between two RKHSes as a dependence measure. 

### Causal and Anticausal Learning

We often use X and Y to talk about the features \(input\) and the label \(output\), where the model learns to predict the labels based on the inputs. On top of that, we can have situations where X causes Y and where Y causes X.

* In **Causal Learning** the prediction flow matches causation. Model is trained based on input features that cause the labels. _Example: predicting gene from mRNA sequence_
* In **Anticausal Learning**, we predict the labels that cause the input features. The prediction and causal relationships are reversed. _Example: MNIST class prediction from handwritten digits._

> When we train a model to classify the class membership of handwritten digits \(i.e. saying if something is a 4 or a 5\), we are predicting the class which causes the handwritten digit to have certain structure. Being an 8 or a 6 determines \(i.e. causes\) the way the digit is handwritten, but when we incur in a recognition task we invert the prediction in relation to the cause.

### Covariate Shift and Semi-Supervised Learning

Goal: learn $$X \rightarrow Y$$, by estimating the properties of the distribution $$p(Y|X)$$.  
In **semi-supervised learning**, we leverage unlabeled data to improve the estimates of $$p(X)$$.   
When facing **covariate shift**, our $$p(X)$$changes between the train and the test \(cf. concept shift\).  
Additionally, the **causal assumption** is that $$p(C)$$and the mechanism $$p(E|C)$$are "independent", i.e. that the distribution of the cause and the distribution of the effect given the cause are independent.

On a **Causal Learning** set, where we predict based on inputs X that cause such labels Y, **semi-supervised learning is impossible.** The distributions of $$p(X)$$and $$p(Y|X)$$are independent \(cf. causal assumption\) and $$p(Y|X)$$is invariant under changes in $$p(X)$$, so improving the estimates of the input features will not affect our estimation for the distribution of the label under such inputs.

On a **Anticausal Learning** scenario, where we predict the cause of our inputs $$Y \rightarrow X$$, **semi-supervised learning is possible.** Given $$p(X)$$and $$p(X|Y)$$are independent \(under causal assumption\), then $$p(X)$$and $$p(Y|X)$$are dependent and we can thus improve our estimation for the distribution of the label $$p(Y|X)$$ with better estimates for the distribution of our inputs $$p(X)$$.

### Semi-Supervised Learning Assumptions

The known SSL assumptions link $$p(X)$$to $$p(Y|X)$$:

* **Cluster assumption:** points in the same cluster $$p(X)$$ should have the ame $$Y$$.
* **Low density separation assumption:** $$p(Y|X)$$should cross 0.5 in area where $$p(X)$$is small. _When in doubt, point to the middle._
* **Semi-supervised smoothness assumption:** $$E(Y|X)$$should be smooth where $$p(X)$$is large 



