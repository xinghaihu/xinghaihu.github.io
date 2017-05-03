# Causal inference

Supervised learning often comes to mind when talking about machine learning: Naive Bayes, Logisic regression, and the hottest deep neural network. The essence of supervised learning is solving generalization - model designers need balance model complexity, interpretability, solvability and practical demands, design loss function and regularization, and finally build an effective optimization algorithm. In order word, supervise learning summarize the data. 

Except for summary, we also want machine to do inference, understand causality and make reasonable decisions. This is extremely valuable for business decision with abundant data available. It answers questions like, whether a market campaign is effective, whether a product optimization increases revenue, what if a different strategy is applied, etc.

In this essay, we will go through the question causal inference is going to answer, logics underlying the solution and finally the math.

## Causal questions
We start from 4 causal problems :  
* __Is a new medicine effective?__ A patient takes a new medicine and gets cured. Can we conclude the effectiveness of the pill? 
* __Is a different strategy better?__ Can we apply a different trading strategy based on historical data and prove it improves earnings?
* __What other news to recommend?__ News website recommends articles similar to what customers have read. For example, Trump supporters are recommended with Trump campaign news and anti-Hillary news. Are these customers only interested in these news? What if Hillary compaign news are recommended?
* __What bandit policy should take?__ There could be Simpsons paradox in bandit. For example, in non-contextual bandit, performance of Arm A is better than Arm B on Friday, so that more allocation is assigned to Arm A on Saturday. Since rewards are averagely higher on weekends, the reward uplift of Arm A compared with Arm B is even higher, making the arm selection converge extremely fast.

All these three problems involving a key question: how to evalute an action. The answer to the first question is clearly NO, as comparison with no-medicine situation is needed, and so do all questions related to action/policy/strategy evaluation - reward given action is not enough and only comparison with alternatives indicate effectiveness. 

But how, and what data and method can be used?

## Regression, IPS, doubly robust
There are two principal ideas to handle the problem. 

### Regression
The idea is to find a function 
