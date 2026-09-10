+++
date = '2026-02-25T12:17:50+03:00'
draft = false
title = 'Perceptron'
+++

This project explores the fundamentals of neural networks by implementing 
a perceptron from scratch to classify sentences as positive or negative. 
The primary objective was to evaluate the performance of three optimization 
methods: Gradient Descent (GD), Stochastic Gradient Descent (SGD), and Adam.
The dataset consists of 400 LLM-generated sentences (200 positive, 200 negative), 
split into an 80% training set and a 20% testing set. The core engine is 
implemented in pure C. Python is utilized for data preprocessing and visualization. 
Check the [source code](https://gitlab.com/den.ege.der/perceptron-v2).

**Example Sentences:**

```
[neg] The echo of a closing door resonated with the finality of an ended chapter.
[neg] The hollow echo of footsteps in an empty hallway underscored the solitude.
[neg] The once-lively room now felt like a cavern of echoes, devoid of life.

[pos] The shared excitement of a surprise event brought smiles and expressions of awe.
[pos] A successful surprise party left the celebrant beaming with happiness and gratitude.
[pos] The sound of rain tapping on the roof created a cozy atmosphere for an evening indoors.
```

## Pipeline

![Pipeline overview](/img/perceptron/pipeline.png)

## Model

The model is defined as: **`y = tanh(WX)`**. **W** is the parameter vector and **X** is the 
one-hot-vector of a sample. The output is rounded to -1 or +1 to produce a binary prediction.
Training supports three optimization methods: GD, SGD, and Adam. Charts are generated using 
Python and `matplotlib`. Trainings are capped at 1000 iterations with a step size (EPS) of 0.1.

## Training

The model's convergence was tested across various initial parameter.

**Random Initialization**

![Random init](/img/perceptron/random.png)

**Initial Parameters: 0**

![Init 0](/img/perceptron/init0.png)

**Initial Parameters: 0.001**

![Init 0.001](/img/perceptron/init0.001.png)

**Initial Parameters: 0.1**

![Init 0.1](/img/perceptron/init0.1.png)

**Initial Parameters: 0.5**

![Init 0.5](/img/perceptron/init0.5.png)

Adam proved the most efficient in terms of both iterations and runtime. While GD frequently stalled 
in local minima, SGD and Adam successfully bypassed them. SGD through stochastic sampling and Adam 
through momentum. At 0 initialization, GD iterations were approximately 16x slower than Adam. 
At 0.5 initialization, models experienced vanishing gradients. No meaningful updates occurred, and 
all methods converged prematurely on the first step. 

## Evaluation and Visualization

Models were evaluated on the held-out test set using random initialization, a 1,000-iteration 
limit, and `EPS = 0.05`.

![Test results](/img/perceptron/results.png)

To visualize the high-dimensional parameter space (1,000+ dimensions), the data was first reduced 
to 50 dimensions via PCA, and then to 2 dimensions using t-SNE. The plot below shows SGD training 
from 5 different starting points (1500 iterations, EPS = 0.01), generated with `plotly`, 
`scikit-learn`, and `numpy`.

![t-SNE plot](/img/perceptron/tsne.png)

| ID | Initial Value | Color |
|----|---------------|-------|
| 0  | 0.01          | Navy  |
| 1  | 0.001         | Purple|
| 2  | -0.03         | Pink  |
| 3  | 0             | Orange|
| 4  | -0.0009       | Yellow|

The visualization confirms that convergence is sensitive to starting points: runs 1, 3, and 4 converged 
to the same local minimum, while runs 0 and 2 diverged.

## Conclusion

Adam consistently outperformed GD and SGD in both convergence speed and computational efficiency. The 
results highlight the susceptibility of standard GD to local minima and the critical role of weight 
initialization in preventing vanishing gradients.
