# Causal inference

```python
def method():
print "just a test"
```

Supervised learning often comes to mind when talking about machine learning: Naive Bayes, Logisic regression, and the hottest deep neural network. The essence of supervised learning is solving generalization - model designers need balance model complexity, interpretability, solvability and practical demands, design loss function and regularization, and finally build an effective optimization algorithm. In order word, supervise learning summarize the data. 

Except for summary, we also want machine to do inference, understand causality and make reasonable decisions. This is extremely valuable for business decision with abundant data available. It answers questions like, whether a market campaign is effective, whether a product optimization increases revenue, what if a different strategy is applied, etc.

In this essay, we will go through the question causal inference to answer, logics underlying the solution and finally the math.

## Causal questions
We start from 4 causal problems :  
* __Is a new medicine effective?__ A patient takes a new medicine and gets cured. Can we conclude the effectiveness of the pill? 
* __Is a different strategy better?__ Can we apply a different trading strategy based on historical data and prove it improves earnings?
* __What other news to recommend?__ News website recommends articles similar to what customers have read. For example, Trump supporters are recommended with Trump campaign or anti-Clinton articles. Are these customers only interested in these news? What if articles supporting Clinton are recommended?
* __What bandit policy should take?__ There could be Simpson's paradox in bandit. For example, in non-contextual bandit, performance of Arm A is better than Arm B on Friday, so that more allocation is assigned to Arm A on Saturday. Since rewards are averagely higher on weekends, the reward uplift of Arm A compared with Arm B is even higher, making the arm selection converge extremely fast.

All these three problems involving a key question: how to evalute an action. The answer to the first question is clearly NO, as comparison with no-medicine situation is needed, and so do all questions related to action/policy/strategy evaluation - reward given action is not enough and only comparison with alternatives indicate effectiveness. 

But how, and what data and method can be used?

## Regression, IPS, doubly robust
There are two principal ideas to handle the problem: regression and IPS.

### Regression
The idea is to find a function that uses contexts and actions to predict reward. With these function, the optimal action given a context can be found, which is the one with highest reward. Similarly, other listed problems can be solved as well. 

It is a good strategy when the estimator is good. However, the estimator can be biased because the applied dataset has a different distribution from the modeled dataset.

When there are confounding factors, the estimator could also be biased and Simpson's paradox can happen. Mathematically, some confounding factor Z is correlated with action, and different actions may end up with different Z, making predicted reward biased. The bandit question (question 4) is an example. 

### IPS
IPS is short for inverse propensity score. The idea is to reweight the data using importance weighting, so that the distribution of context will be the same for differnt actions and a fair comparison can be made. 

This estimator is unbiased, however prone to high variance, especially when dividing a small propensity score (probability to assignment). Similar to regression method, IPS will also suffer when there is confounding factors.

Usually, a separate model is needed to compute the probability of assignment in the dataset - either it is a Logistic regression model that uses context to predict assignment or a density estimation model.

### Doubly robust
[Doubly robust](https://arxiv.org/pdf/1103.4601.pdf) is a statistical method that combines the previous two methods and can balance bias and variance. It is claimed that, when either regression model or IPS performs well, doubly robust estimator also win. 

This estimator uses regression model as a baseline, and use IPS to adjust the difference of every observation reward from the baseline reward.

## Instrumental variable
As we discussed above, confounding factors could be a killer to causal inference, as it is correlated with action. To alleviate the problem, instrumental variable (IV) is introduced - a variable that is correlated with action but uncorrelated with all contexts. 

Here is an example. In a study about relation between opium treatment duration and post-cure opium addition rate, patients' personal condition can be a confounder - ex. spritually weaker patient are prone to worse health condition and also becoming more likely to get drug addicted. An IV in this case is the distance between the patients' residence and the hospital, as patience living closer to the hospital has an averagely longer duration to be treated, whereas the distance is independent from patients' conditions. Hence it is a fair comparison between people living closer and farther from hospitals.

In observational study, samples are usually randomly split into intent-to-treat (IIT) group and control group. The group index can be used as IV, as it is independent from any context variables. 

To use IV in causal modeling, it is usually used as a feature in the reward model or assignment model mentioned in previous section. 

If no IV is available, a method called _two stage least squares_ (2SLS) can be used. In this method, treatment is projected to the subspace spanned by all context variables, and the residue is used to for the second stage regression. The idea is that, the residue is correlated to action but not context, and thus can be viewed as IV. 

## Summary
Causal inference answers the question: will the result differ when an action change? It is the key to policy evaluation, domain adaptation and counterfactual analysis. The main method includes regression adaptation and IPS adaptation, and doubly robust combines the two method and balance the bias and variation. Confounder can be a headache in causal inference, and IV/2SLS can help alleviate the problem.
