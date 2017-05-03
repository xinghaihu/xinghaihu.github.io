# Causal inference

Supervised learning often comes to mind when talking about machine learning: Naive Bayes, Logisic regression, and the hottest deep neural network. The essence of supervised learning is solve generalization - model builder needs to design the model framework, loss function and regularization, and finally an effective optimization algorithm. In order word, supervise learning summarize the data. 

Except for summary, we also want machine to do inference, understand causality and make reasonable decisions. This is extremely valuable for business decision with abundant data available. 

In this essay, we will go through the question causal inference is going to answer, logics underlying the solution and finally the math.

## Causal questions
We start from 3 causal questions:  
* __Is a new medicine effective?__ A patient takes a new medicine and gets cured. Can we conclude the effectiveness of the pill? 
* 
