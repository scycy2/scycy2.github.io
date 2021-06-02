---
title: Linear Regression
date: 2021-06-01 22:12:14
tags: ['未完待续', '机器学习', '线性回归']
categories: ['自学笔记']
cover: false
summary: Linear Regression
img:
mathjax: true
---

##### Notation

* $x^{(i)}$ to denote the "input" variables, also called input features.
* $y^{(i)}$ to denote the "output" or target variable that we are trying to predict.
* A pair $(x^{(i)}, y^{(i)})$ is called a training example.
* The dataset that we'll be using to learn - a list of $m$ training examples $(x^{(i)}, y^{(i)}); i = 1, \cdots, m$ - is called a training set.
* Note that the superscript "$(i)$" in the notation is simply an index into the training set, and has nothing to do with exponentiation.
* $X$ to denote the space of input values.
* $Y$ to denote the space of output values.

#### 1. Model Representation

* Given a training set, to learn a function $h: X\rightarrow Y$ so that $h(x)$ is a "good" predictor for the corresponding value of $y$. $h$ is called a hypothesis.
* When the target variable that we're trying to predict is continuous, we call the learning problem a regression problem.

#### 2. Cost Function (Loss Function)

* This takes an average difference (actually a fancier version of an average) of all the results of the hypothesis with inputs from $x$'s and the actual output $y$'s.

* $$
  J(\theta_0, \theta_1) = \frac{1}{2m}\sum_{i=1}^{m}(\hat{y_i} - y_i)^2 = \frac{1}{2m}\sum_{i=1}^{m}(h_{\theta}(x_i) - y_i)^2
  $$

* This function is otherwise called the "Squared error function", or "Mean squared error". The mean is halved $(\frac{1}{2})$ as a convenience for the computation of the gradient descent, as the derivative term of the square function will cancel out the $\frac{1}{2}$ term.

* As a goal, we should try to minimize the cost function.

#### 3. Gradient Descent

* The slope of the tangent is the derivative at that point and it will give us a direction to move towards. We make steps down the cost function in the direction with the steepest descent. The size of each step is determined by the parameter $\alpha$, which is called the learning rate.

* A smaller $\alpha$ would result in a smaller step and a larger $\alpha$ results in a larger step. The direction in which the step is taken is determined by the partial derivative of $J(\theta_0, \theta_1)$.

* The gradient descent algorithm is:

  * repeat until convergence:
    $$
    \theta_j := \theta_j - \alpha\frac{\partial}{\partial \theta_j}J(\theta_0, \theta_1)
    $$
    where

    $j = 0, 1$ represents the feature index number.

* At each iterarion $j$, one should simultaneously update the parameters $\theta_1, \theta_2, \cdots, \theta_n$. Updating a specific parameter prior to calculating another one on the $j^{(th)}$ iteration would yield to a wrong implementation.
* We should adjust our parameter $\alpha$ to ensure that the gradient descent algorithm converges in a reasonable time. Failure to converge or too much time to obtain the minimum value imply that our step size is wrong.
* Gradient descent can converge to a local minimum, even with the learning rate $\alpha$ fixed. As we approach a local optimum, gradient descent will automatically take smaller steps. So, no need to decrese $\alpha$ over time.

#### 4. Multiple Features

* Linear regression with multiple variables is also known as "multivariate linear regression".

* $x^{(i)}_j = $ value of feature $j$ in the $i^{th}$ training example

* $x^{(i)} = $ the input (features) of the $i^{th}$ training example

* $m = $ the number of training examples

* $n = $ the number of features

* The multivariable form of the hypothesis function accommodating these multiple features is as follows:
  $$
  h_{\theta}(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + \theta_3x_3 + \cdots + \theta_nx_n
  $$

* Using the definition of matrix multiplication, our multivariable hypothesis function can be concisely represented as:
  $$
  h_{\theta}(x) = \begin{bmatrix}\theta_0 & \theta_1 & \cdots & \theta_n\end{bmatrix} \begin{bmatrix}x_0\\x_1\\ \vdots\\ x_n\end{bmatrix} = \theta^Tx
  $$

#### 5. Gradient Descent for Multiple Variables

