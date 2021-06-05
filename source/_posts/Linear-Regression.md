---
title: Linear Regression
date: 2021-06-01 22:12:14
tags: ['未完待续', '机器学习', '线性回归']
categories: ['自学笔记']
cover: true
summary: Linear Regression
img: /medias/featureimages/26.jpg
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

* Linear regression with multiple variables is also known as **"multivariate linear regression"**.

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

* repeat until convergence:
  $$
  \theta_j := \theta_j - \alpha\frac{1}{m}\sum_{i=1}^{m}(h_{\theta}(x^{(i)}) - y^{(i)})\times x_j^{(i)}\quad for\ j := 0\cdots n
  $$

#### 6. Gradient Descent in Practice - Feature Scaling

* We can speed up gradient descent by having each of our input values in roughly the same range. This is because $\theta$ will descend quickly on small ranges and slowly on large ranges, and so will oscillate inefficiently down to the optimum when the variables are very uneven.

* The way to prevent this is to modify the ranges of our input variables so that they are all roughly the same. Ideally: $-1 \le x_{(i)} \le 1$ or $-0.5 \le x_{(i)} \le 0.5$. These aren't exact requirements; we are only trying to speed things up. The goal is to get all input variables into roughly one of these ranges, give or take a few.

* Two techniques to help with this are **feature scaling** and **mean normalization**. Feature scaling involves dividing the input values by the range (i.e. the maximum value minus the minimum value) of the input variable, resulting in a new range of just 1. Mean normalization involves subtracting the average value for an input variable from the values for that input variable resulting in a new average value for the input variable of just zero. To implement both of these techniques, adjust your input values as shown this formula:
  $$
  x_i := \frac{x_i - \mu_i}{s_i}
  $$
  Where $\mu_i$ is the **average** of all the values for feature $(i)$ and $s_i$ is the range of values (max - min), or $s_i$ is the standard deviation.

#### 7. Gradient Descent in Practice - Learning Rate

* **Debugging gradient descent.** Make a plot with *number of iterations* on the x-axis. Now plot the cost function, $J(\theta)$ over the number of iterations of gradient descent. If $J(\theta)$ ever increases, then you probably need to decrease $\alpha$.
* **Automatic convergence test.** Declare convergence if $J(\theta)$ decreases by less than $E$ in one iteration, where $E$ is some small value such as $10^{-3}$. However in practice it's difficult to choose this threshold value.
* It has been proven that if learning rate $\alpha$ is sufficiently small, then $J(\theta)$ will decrease on every iteration.
* To summarize:
  * If $\alpha$ is too small: slow convergence.
  * If $\alpha$ is too large: may not decrease on every iteration and thus may not converge.

#### 8. Features and Polynomial Regression

* We can improve our features and the form of our hypothesis function in a couple different ways.
* We can **combine** multiple features into one.
* **Polynomial Regression**
  * Our hypothesis function need not be linear (a straight line) if that does not fit the data well.
  * We can **change the behavior or curve** of our hypothesis function by making it a quadratic, cubic or square root function (or any other form).
  * For example, if our hypothesis function is $h_{\theta}(x) = \theta_0 + \theta_1 x_1$ then we can create additional features based on $x_1$, to get the quadratic function $h_{\theta}(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_1^2$.
  * One important thing to keep in mind is, if you choose your features this way then feature scaling becomes very important.

#### 9. Normal Equation

* A second way of minimizing $J$.

* In the "Normal Equation" method, we will minimize $J$ by explicitly taking its derivatives with respect to the $\theta_j$'s, and setting them to zero. This allows us to find the optimum theta without iteration. The normal equation formula is given below:
  $$
  \theta = (X^TX)^{-1}X^Ty
  $$

* There is **no need** to do feature scaling with the normal equation.

* The comparison of gradient descent and the normal equation:

  <img src="Linear-Regression/Screen Shot 2021-06-04 at 10.02.03 PM.png" style="zoom:50%;" />

* With the normal equation, computing the inversion has complexity $O(n^3)$. So if we have a very large number of features, the normal equation will be slow. In practice, when $n$ exceeds $10,000$ it might be a good time to go from a normal solution to an iterative process.

* **Noninvertibility**

  * If $X^TX$ is **noninvertible**, the common causes might be having:
    * Redundant features, where two features are very closely related (i.e. they are linearly dependent)
    * Too many features (e.g. $m\le n$). In this case, delete some features or use "regularization".
  * Solutions to the above problems include deleting a feature that is linearly dependent with another or deleting one or more features when there are too many features.
