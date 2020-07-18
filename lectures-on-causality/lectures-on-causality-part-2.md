# Lectures on Causality - Part 2

## Total Causal Effect

Given a SCM, there is a total causal effect from X to Y if one of the following equivalent statements is satisfied:

* $$X \not\!\perp\!\!\!\perp Y$$in $$P_{do\ X}:=Ñ_X$$for some random variable $$Ñ_X$$. _i.e. randomize X and see that X and Y depend on each other_
* There are $$x^{\triangle}$$and $$x^{\square}$$, such that $$P^Y_{do\ X:=x^{\triangle}} \neq P^Y_{do\ X:=x^{\square}} $$
* There is $$x^{\triangle}$$, such that$$P^Y_{do\ X:=x^{\triangle}} \neq P^Y$$
* $$X \not\!\perp\!\!\!\perp Y$$ in $$P^{X,Y}_{do\ X:=Ñ_X} \neq P^Y$$for any $$Ñ_X$$whose distribution has full support

 

## Instrumental Variable

In the cases where we have a hidden cofounding influence on cause and effect \(e.g. socioeconomic status H on both smoking  X and lung cancer Y\), we can add an instrumental variable that affects the causer node only \(e.g. tax on tobacco $$I \rightarrow X$$\) such that it does not influence directly either the hidden factors H or the effect Y. 

![I - Tax, X - Smoking, H - Socioeconomic Status, Y - Lung Cancer](../.gitbook/assets/image%20%2839%29.png)

The structural equations for this example are thus:

* $$ Y = \alpha X + \gamma H + N_Y$$
* $$ X = \beta H + \delta I + N_X$$ _meaning that_
* $$ Y = (\alpha \beta + \gamma )H + \alpha \delta I + \alpha N_X + N_Y$$

Then, we do a 2-step:

* Regress X on I. With nothing in between, you'll get an unbiased estimation for $$\delta$$. Find fitted values $$fitted\ values \approx \delta I$$
* Regress Y on fitted values to get a rough estimate of $$\alpha$$, since all the other terms $$(  (\alpha \beta + \gamma)H, \alpha N_X, N_Y )$$are independent of Y.



