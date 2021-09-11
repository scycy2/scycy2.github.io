---
title: Deep Learning Optimization Methods
date: 2021-09-03 10:50:50
tags: ['Optimization Methods', 'Neural Network']
categories: ['self-study notes']
cover:
summary: Optimizors in Deep Learning (Momentum, RMSprop, Adam)
img:
mathjax: true
---

#### 1. Exponentially Weighted Moving Average (EWMA)

* The Exponentially Weighted Moving Average (EWMA) is a quantitative or statistical measure used to model or describe a time series.

* The moving average is designed as such that older observations are given lower weights. The weights fall exponentially as the data input gets older.

* The only decision a user of the EWMA (denoted by $v$) must take is the parameter **beta**. The parameter decides how important the current observation is in the calculation of the $v$. The higher the value of alpha, the more closely the $v$ racks the original time series.

* Suppose given a series of data $x_1, x_2, \cdots, x_n$，to fit a curve, we use following formula:

  * $$
    v_0 = 0\\
    v_1 = \beta \times v_0 + (1 - \beta) \times x_1\\
    v_2 = \beta \times v_1 + (1 - \beta) \times x_2\\
    \cdots\\
    v_t = \beta \times v_{t-1} + (1 - \beta) \times x_t
    $$

  * <img src="Deep-Learning-Optimization-Methods/Screen Shot 2021-09-11 at 11.31.19 AM.png" style="zoom:50%;" />

    $\beta = 0.9$

* $\beta = 0.98$, green line; $\beta = 0.5$, yellow line

  <img src="Deep-Learning-Optimization-Methods/Screen Shot 2021-09-11 at 11.35.13 AM.png" style="zoom:50%;" />

#### 2. Momentum

* To solve the problem of large oscillating of mini-batch SGD when updating parameters.

* Make the convergence speed faster.

* Implementation details:

  On iteration $t$:

  ​		Compute $dW, db$ on the current mini-batch

  ​		$v_{dW} = \beta v_{dw} + (1 - \beta)dW$

  ​		$v_{db} = \beta v_{db} + (1 - \beta)db$

  ​		$W = W - \alpha v_{dW}$

  ​		$b = b - \alpha v_{db}$

* Momentum takes past gradients into account to smooth out the steps of gradient descent. It can be applied with batch gradient descent, mini-batch gradient descent or stochastic gradient descent.

* You have to tune a momentum hyperparameter $\beta$ and a learning rate $\alpha$.

#### 3. RMSprop

* Blue: Momentum; Green: RMSprop

  <img src="Deep-Learning-Optimization-Methods/Screen Shot 2021-09-11 at 11.50.06 AM.png" style="zoom:50%;" />

* Implementation details:

  On iteration $t$:

  ​		Compute $dW, db$ on the current mini-batch

  ​		$s_{dW} = \beta_2 s_{dw} + (1 - \beta_2)dW^2$

  ​		$s_{db} = \beta_2 s_{db} + (1 - \beta_2)db^2$

  ​		$W = W - \alpha \frac{dW}{\sqrt{s_{dW}} + \epsilon}$

  ​		$b = b - \alpha \frac{db}{\sqrt{s_{db}} + \epsilon}$

* Generally, $\epsilon = 10^{-8}$ 

* Make the oscillating small

#### 4. Adam

* The combination of **Momentum** and **RMSprop**

* Implementation details:

  $v_{dW} = 0, s_{dW} = 0, v_{db} = 0, s_{db} = 0$

  On iteration $t$:

  ​		Compute $dW, db$ on the current mini-batch

  ​		$v_{dW} = \beta_1 v_{dW} + (1 - \beta_1)dW, v_{db} = \beta_1 v_{db} + (1 - \beta_1)db$

  ​		$s_{dW} = \beta_2 s_{dW} + (1 - \beta_2)dW^2, s_{db} = \beta_2 s_{db} + (1 - \beta_2)db^2$

  ​		$v_{dW}^{corected} = \frac{v_{dW}}{1 - \beta_1^t}, v_{db}^{corected} = \frac{v_{db}}{1 - \beta_1^t}$

  ​		$s_{dW}^{corrected} = \frac{s_{dW}}{1 - \beta_2^t}, s_{db}^{corrected} = \frac{s_{db}}{1 - \beta_2^t}$

  ​		$W = W - \alpha \frac{v_{dW}^{corrected}}{\sqrt{s_{dW}^{corrected}} + \epsilon}, b = b - \alpha \frac{v_{db}^{corrected}}{\sqrt{s_{db}^{corrected}} + \epsilon}$

* Since the moving average at the beginning of iteration will lead to a large difference from the initial value, we need to correct the deviation of the values.
* Generally, $\beta_1 = 0.9, \beta_2 = 0.999, \epsilon = 10^{-8}$, $\alpha$ need to be tuned.
