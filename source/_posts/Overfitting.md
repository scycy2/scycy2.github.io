---
title: Overfitting
date: 2021-06-09 16:14:59
tags: ['Machine Learning']
categories: ['self-study notes']
cover:
summary: Overfitting (Regularization)
img: /medias/featureimages/16.jpg
mathjax: true
---

#### 1. The Problem of Overfitting

* **Underfitting**, or high bias, is when the form of our hypothesis function $h$ maps poorly to the trend of the data. It is usually caused by a function that is too simple or uses too few features.
* At the other extreme, **overfitting**, or high variance, is caused by a hypothesis function that fits the available data but does not generalize well to predict new data. It is usually caused by a complicated function that creates a lot of unnecessary curves and angles unrelated to the data.
* Two main options to address the issue of overfitting:
  * Reduce the number of features:
    * Manually select which features to keep.
    * Use a model selection algorithm.
  * Regularization
    * Keep all the features, but reduce the magnitude of parameters $\theta_j$.
    * Regularization works well when we have a lot of slightly useful features.

#### 2. Cost Function

* If we have overfitting from our hypothesis function, we can reduce the weight that some of the terms in our function carry by increasing their cost.

* 
  $$
  \min_\theta\frac{1}{2m}\Sigma_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})^2 + \lambda \Sigma_{j=1}^n \theta_j^2
  $$

* The $\lambda$, or lambda, is the **regularization parameter**. It determines how much the costs of our theta parameters are inflated.

* Using the above cost function with extra summation, we can smooth the output of our hypothesis function to reduce overfitting.

* If lambda is chosen to be too large, it may smooth out the function too much and cause underfitting.

#### 3. Regularized Linear Regression

* **Gradient Descent**
  $$
  \begin{align*}
  & Repeat\ \{\\
  & \quad\theta_0 := \theta_0 - \alpha\frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_0^{(i)}\\
  & \quad\theta_j := \theta_j - \alpha\left[ \left( \frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}\right) + \frac{\lambda}{m}\theta_j\right] \quad\quad j\in\{1, 2\cdots n\}\\
  & \}
  \end{align*}
  $$

* The term $\frac{\lambda}{m}\theta_j$ performs regularization. With some manipulation, the update rule can also be represented as:
  $$
  \theta_j := \theta_j(1-\alpha\frac{\lambda}{m}) - \alpha\frac{1}{m}\Sigma_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}
  $$
  The first term in the above equation, $1 - \alpha\frac{\lambda}{m}$ will always be less than $1$. Intuitively reduce the value of $\theta_j$ by some amount on every update.

* **Normal Equation**
  $$
  \theta = (X^TX+\lambda L)^{-1}X^Ty\quad
  where\ L = \begin{bmatrix} 0 \\ \ & 1 \\ \ &\ & 1 \\ \ & \ & \ & \ddots \\ \ & \ & \ &\ & 1\end{bmatrix}
  $$
  $L$ is a matrix with $0$ at the top left and $1$'s down the diagonal, with $0$'s everywhere else. It should have dimension $(n+1)\times (n+1)$. Intuitively, this is the identity matrix (though we are not including $x_0$), multiplied with a single real number $\lambda$.

* When we add the term $\lambda L$, then $X^TX + \lambda L$ becomes invertible.

#### 4. Regularized Logistic Regression

* Cost function
  $$
  J(\theta) = -\frac{1}{m}\Sigma_{i=1}^m[y^{(i)}\log(h_\theta(x^{(i)})) + (1-y^{(i)})\log(1-h_\theta(x^{(i )}))] + \frac{\lambda}{m}\Sigma_{j=1}^n\theta_j^2
  $$
  The second sum, $\Sigma_{j=1}^n\theta_j^2$ **means to explicitly exclude** the bias term $\theta_0$.

* **Gradient Descent**
  $$
  \begin{align*}
  & Repeat\ \{\\
  & \quad\theta_0 := \theta_0 - \alpha\frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_0^{(i)}\\
  & \quad\theta_j := \theta_j - \alpha\left[ \left( \frac{1}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}\right) + \frac{\lambda}{m}\theta_j\right] \quad\quad j\in\{1, 2\cdots n\}\\
  & \}
  \end{align*}
  $$
