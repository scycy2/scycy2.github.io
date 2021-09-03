---
title: Advice for applying machine learning
date: 2021-06-29 19:16:12
tags: ['Machine Learning', 'Bias/Variance', 'Cross Validation', 'Learning Curves']
categories: ['self-study notes']
cover:
summary: Advice when applying ML
img:
mathjax: true
---

#### 1. Evaluating a Learning Algorithm

##### 1.1 Evaluating a Hypothesis

* A hypothesis may have a low error for the training examples but still be inaccurate (because of overfitting). Thus, to evaluate a hypothesis, given a dataset of training examples, we can split up the data into two sets: a **training set** and a **test set**. Typically, the training set consists of $70\%$ of data and the test set is the remaining $30\%$.

* The new procedure using these two sets is then:

  * Learn $\Theta$ and minimize $J_{train}(\Theta)$ using the training set
  * Compute the test set error $J_{test}(\Theta)$

* **The test set error**

  * For linear regression: $J_{test}(\Theta) = \frac{1}{2m_{test}}\Sigma_{i=1}^{m_{test}}(h_\Theta(x_{test}^{(i)})-y_{test}^{(i)})^2$

  * For classification ~ Misclassification error (aka $0/1$ misclassification error): 
    $$
    err(h_\Theta(x), y) = \begin{cases}1\quad \text{if } h_\Theta \ge 0.5 \text{ and } y=0 \text{ or } h_\Theta \lt 0.5 \text{ and } y=1\\ 0\quad \text{otherwise}\end{cases}
    $$
    
    $$
    \text{Test Error} = \frac{1}{m_{test}}\Sigma_{i=1}^{m_{test}}err(h_\Theta(x_{test}^{(i)}), y_{test}^{(i)})
    $$

##### 1.2 Model Selection and Train/Validation/Test Sets

* Just because a learning algorithm fits a training set well, that does not mean it is a good hypothesis. It could overfit and as a result the predication on the test set would be poor. The error of the hypothesis as measured on the dataset with which you trained the parameters will be lower than the error on any other data set.

* Given many models with different polynomial degrees, we can use a systematic approach to identify the 'best' function. In order to choose the model of the hypothesis, test each degree of polynomial and look at the error result.
* One way to break down the dataset into the three sets is:
  * Training set: $60\%$
  * Cross validation set: $20\%$
  * Test set: $20\%$
* Calculate three separate error values for the three different sets using the following method:
  1. Optimize the parameters in $\Theta$ using the training set for each polynomial degree.
  2. Find the polynomial degree $d$ with the least error using the cross validation set.
  3. Estimate the generalization error using the test set with $J_{test}(\Theta^{(d)})$, (d = theta from polynomial with lower error);
* This way, the degree of the polynomial $d$ has not been trained using the test set.

#### 2. Bias vs. Variance

##### 2.1 Diagnosing Bias vs. Variance

* Need to distinguish whether **bias** or **variance** is the problem contributing to bad predictions.

* High bias is underfitting and high variance is overfitting. Ideally, we need to find a golden mean between these two.

* The training error will tend to **decrease** as we increase the degree $d$ of the polynomial.

  At the same time, the cross validation error will tend to **decrease** as we increase $d$ up to a point, and then it will **increase** as $d$ is increased, forming a convex curve.

* **High bias (underfitting)**: both $J_{train}(\Theta)$ and $J_{CV}(\Theta)$ will be high. Also, $J_{CV}(\Theta) \approx J_{train}(\Theta)$

* **High variance (overfitting)**: $J_{train}(\Theta)$ will be low and $J_{CV}(\Theta)$ will be much greater than $J_{train}(\Theta)$

* Summurized in the figure below:

  <img src="Advice-for-applying-machine-learning/BiasAndVariance.png" style="zoom:50%;" />

##### 2.2 Regularization and Bias/Variance

* <img src="Advice-for-applying-machine-learning/RegularizationAndBias:Variance.png" style="zoom:50%;" />

* In the figure above, as $\lambda$ increases, the fit becomes more rigid. On the other hand, as $\lambda$ approaches $0$, the model tends to overfit the data.
* In order to choose the model and the regularization term $\lambda$, need to:
  1. Create a list of lambdas (i.e. $\lambda \in \{0, 0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 1.28, 2.56, 5.12, 10.24\}$);
  2. Create a set of models with different degrees or any other variants.
  3. Iterate through the $\lambda$s and for each $\lambda$ go through all the models to learn some $\Theta$.
  4. Compute the cross validation error using the learned $\Theta$ (computed with $\lambda$) on the $J_{CV}(\Theta)$ **without** regularization or $\lambda = 0$.
  5. Select the best combo that produces the lowest error on the cross validation set.
  6. Using the best combo $\Theta$ and $\lambda$, apply in on $J_{test}(\Theta)$ to see if it has a good generalization of the problem.

##### 2.3 Learning Curves

* Training an algorithm on a very few number of data points (such as 1, 2 or 3) will easily have 0 errors because we can always find a quadratic curve that touches exactly those number of points. Hence
  * As the training set gets larger, the error for a quadratic function increases.
  * The error value will plateau out after a certain $m$, or training set size.
* **Experiencing high bias**:
  * **Low training set size**: causes $J_{train}(\Theta)$ to be low and $J_{CV}(\Theta)$ to be high.
  * **Large traning set size**: causes both $J_{train}(\Theta)$ and $J_{CV}(\Theta)$ to be high with $J_{train}(\Theta) \approx J_{CV}(\Theta)$.
  * If a learning algorithm is suffering from **high bias**, getting more training data will not **(by itself)** help much.
* **Experiencing high variance**:
  * **Low training set size**: $J_{train}(\Theta)$ will be low and $J_{CV}(\Theta)$ will be high.
  * **Large training set size**: $J_{train}(\Theta)$ increases with training set size and $J_{CV}(\Theta)$ continues to decrease without leveling off. Also, $J_{train}(\Theta) \lt J_{CV}(\Theta)$ but the difference between them remains significant.
  * If a learning algorithm is suffering from **high variance**, getting more training data is likely to help.

##### 2.4 Deciding what to do next revisited

* The decision process can be broken down as follows:
  * **Getting more training examples**: Fixes high variance
  * **Trying smaller sets of features**: Fixes high variance
  * **Adding features**: Fixes high bias
  * **Adding polynomial features**: Fixes high bias
  * **Decreasing $\lambda$**: Fixes high bias
  * **Increasing $\lambda$**: Fixes high variance
* **Diagnosing Neural Networks**
  * A neural network with fewer parameters is **prone to underfitting**. It is also **Computationally cheaper**.
  * A large neural network with more parameters is **prone to overfitting**. It is also **computationally expensive**. In this case, use regularization (increase $\lambda$) to address overfitting.
  * Using a single hidden layer is a good starting default. You can train your neural network on a number of hidden layers using cross validation set. Then select the one that performs best.
* **Model Complexity Effects**:
  * Lower-order polynomials (low model complexity) have high bias and low variance. In this case, the model fits poorly consistently.
  * Higher-order polynomials (high model complexity) fit the training data extremely well and the test data extremely poorly. These have low bias on the training data, but very high variance.
  * In reality, we would want to choose a model somewhere in between, that can generalize well but also fits the data reasonably well.
