---
title: Logistic Regression -- Classification
date: 2021-06-08 15:26:22
tags: ['机器学习','逻辑回归','分类问题', '未完待续']
categories: ['自学笔记']
cover: true
summary: Logistic Regression
img: /medias/featureimages/21.jpg
mathjax: true
---

#### 1. Classification

* The classification problem is just like the regression problem, except that the values we now want to predict take on only a small number of discrete values.
* **Binary classification problem**
  * $y$ can take on only two values, $0$ and $1$. (Multi-class later)
  * $0$ is called the negative class, and $1$ the positive class, and they are sometimes also denoted by the symbols "-" and "+".
  * Given $x^{(i)}$, the corresponding $y^{(i)}$ is also called the **label** for the training example.

#### 2. Hypothesis Representation

* By using the "Sigmoid Function", also called the "Logistic Function":
  $$
  \begin{align*}
  & h_{\theta}(x) = g(\theta^Tx)\\
  & z = \theta^Tx\\
  & g(z) = \frac{1}{1+e^{-z}}
  \end{align*}
  $$

* The function $g(z)$, maps any real number to the $(0,1)$ interval, making it useful for transforming an arbitrary-valued function into a function suited for classification.

* $h_{\theta}(x)$ will give us the **probability** that our output is $1$. The probability that the prediction is $0$ is just the complement of the probability that it is $1$.
  $$
  \begin{align*}
  & h_{\theta}(x) = P(y=1\mid x;\theta) = 1 - P(y=0\mid x;\theta)\\
  & P(y=0\mid x;\theta) + P(y=1\mid x;\theta) = 1
  \end{align*}
  $$

#### 3. Decision Boundary

* $$
  \theta^Tx\ge 0 \Rightarrow y=1\\
  \theta^Tx\lt 0 \Rightarrow y=0
  $$

* The **decision boundary** is the line that separates the area where $y = 1$ and where $y = 0$. It is created by the hypothesis function.

* The input to the sigmoid function $g(z)$ (e.g. $\theta^TX$) doesn't need to be linear, and could be a function that describes a circle or any shape to fit our data.

#### 4. Cost Function

* We cannot use the same cost function that we use for linear regression because the Logistic Function will cause the output to be wavy, causing many local optima. In other words, it will not be a convex function.

* Instead, our cost function for logistic regression looks like:
  $$
  \begin{align*}
  & J(\theta) = \frac{1}{m}\sum^m_{i=1}Cost(h_\theta(x^{(i)}), y^{(i)})\\
  & Cost(h_\theta(x), y) = -\log(h_\theta(x))\quad\quad if\ y = 1\\
  & Cost(h_\theta(x), y) = -\log(1-h_\theta(x))\ if\ y = 0
  \end{align*}
  $$

* $$
  \begin{align*}
  & Cost(h_{\theta}(x), y) = 0\ if\ h_\theta(x) = y\\
  & Cost(h_\theta(x), y) \rightarrow \infin\ if\ y = 0\ and\ h_\theta(x)\rightarrow 1\\
  & Cost(h_\theta(x), y) \rightarrow \infin\ if\ y = 1\ and\ h_\theta(x) \rightarrow 0
  \end{align*}
  $$

* If the correct answer '$y$' is $0$, then the cost function will be $0$ if our hypothesis function also ouputs $0$. If the hypothesis approaches $1$, then the cost function will approach infinity. If the correct answer '$y$' is $1$, the case is reverse.

* Not that writing the cost function in this way guarantees that $J(\theta)$ is convex for logistic regression.

#### 5. Simplified Cost Function

* Compress the cost function's two conditional cses into one case:
  $$
  Cost(h_\theta(x), y) = -y\log(h_\theta(x)) - (1-y)\log(1-h_\theta(x))
  $$

* Cost function
  $$
  J(\theta) = -\frac{1}{m}\sum_{i=1}^m[y^{(i)}\log(h_\theta(x^{(i)})) + (1-y^{(i)})\log(1 - h_\theta(x^{(i)}))]
  $$

* A vectorized implementation:
  $$
  h = g(X\theta)\\
  J(\theta) = \frac{1}{m}(-y^T\log(h) - (1-y)^T\log(1-h))
  $$

#### 6. Gradient Descent

* $$
  \begin{align*}
  & Repeat\ \{ \\
  & \theta_j := \theta_j - \frac{\alpha}{m}\sum_{i=1}^m(h_\theta(x^{(i)}) - y^{(i)})x_j^{(i)}\\
  & \}
  \end{align*}
  $$

* A vectorized implementation:
  $$
  \theta := \theta - \frac{\alpha}{m}X^T(g(X\theta) - \vec{y})
  $$
  

#### 7. Advanced Optimization

* "Conjugate gradient", "BFGS", and "L-BFGS" are more sophisticated, faster ways to optimize $\theta$ that can be used instead of gradient descent.

#### 8. Multiclass Classification: One-vs-all

* Instead of $y = \{0, 1\}$, we will expand the definition so that $y = \{0, 1\cdots n\}$.

* Since $y = \{0, 1\cdots n\}$, we divide the problem into $n+1$ binary classification problems; in each one, predict the probability that '$y$' is a member of one of our classes.

* 
  $$
  \begin{align*}
  & y\in {0, 1\cdots n}\\
  & h_\theta^{(0)}(x) = P(y = 0\mid x;\theta)\\
  & h_\theta^{(1)}(x) = P(y = 1\mid x;\theta)\\
  & \cdots\\
  & h_\theta^{(n)}(x) = P(y = n\mid x;\theta)\\
  & prediction = \max_{i}(h_\theta^{(i)}(x))
  \end{align*}
  $$

* We are basically choosing one class and then lumping all the others into a single second class. Do this repeatedly, applying binary logistic regression to each case, and then use the hypothesis that returned the highest value as our prediction.
