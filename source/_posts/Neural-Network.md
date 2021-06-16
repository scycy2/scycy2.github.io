---
title: Neural Network
date: 2021-06-16 13:47:38
tags: ['神经网络', '机器学习', '未完待续']
categories: ['自学笔记']
cover:
summary: 神经网络
img:
mathjax: true
---

#### 1. Model Representation

* Input: features $x_1\cdots x_n$

* Output: the result of the hypothesis function.

* <img src="Neural-Network/Neural Network Rep1.png" style="zoom:50%;" />

* $x_0$ input node is called the "bias unit". It is always equal to $1$.

* The same logistic function as in classification, $\frac{1}{1+e^{-\theta^Tx}}$, which is also called sigmoid (logistic) **activation** function.

* "theta" parameters are called "weights".

* A simplistic representation:
  $$
  [x_0x_1x_2]\rightarrow [\ ]\rightarrow h_\theta(x)
  $$

* Inpur nodes (layer 1), known as the "input layer", go into another node (layer 2), which finally outputs the hypothesis function, known as the "output layer".

* The intermediate layers of nodes between the input and output layers called the "hidden layers".

* <img src="Neural-Network/Neural Network Rep2.png" style="zoom:50%;" />

* We label these intermediate or "hidden" layer nodes $a_0^2\cdots a_n^2$ and call them "activation units".

* $$
  \begin{align*}
  & a_i^{(j)} = \text{activation}\ \text{of unit}\ i\ \text{in layer}\ j\\
  & \Theta^{(j)} = \text{matrix of weights controlling function mapping from layer}\ j \text{ to layer}\ j+1
  \end{align*}
  $$

* $$
  [x_0x_1x_2x_3]\rightarrow \left[a_1^{(2)}a_2^{(2)}a_3^{(2)}\right]\rightarrow h_\theta(x)
  $$

* The values for each of the "activation" nodes is obtained as follows:
  $$
  \begin{align*}
  && a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_2 + \Theta_{12}^{(1)}x_1 + \Theta_{13}^{(1)}x_3)\\
  && a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_2 + \Theta_{22}^{(1)}x_1 + \Theta_{23}^{(1)}x_3)\\
  && a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_2 + \Theta_{32}^{(1)}x_1 + \Theta_{33}^{(1)}x_3)\\
  && h_\Theta(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)})
  \end{align*}
  $$

* The dimensions of matrices of weights is determined as follows:

  **If network has $s_j$ units in layer $j$ and $s_{j+1}$ units in layer $j+1$, then $\Theta^{(j)}$ will be of dimension $s_{j+1}\times (s_j+1)$**.

  The $+1$ comes from the addition in $\Theta^{(j)}$ of the "bias nodes", $x_0$ and $\Theta_0^{(j)}$. In other words the output nodes will not include the bias nodes while the inputs will.

#### 2. Vectorized Representation

* Setting $x = a^{(1)}$

  $z^{(j)} = \Theta^{(j-1)}a^{(j-1)}$

* Multiply matrix $\Theta^{(j-1)}$ with dimensions $s_j\times (n+1)$ (where $s_j$ is the number of activation nodes) by vector $a^{(j-1)}$ with height $n+1$. This gives vector $z^{(j)}$ with height $s_j$. Now the vector of activation nodes for layer $j$ as follows:
  $$
  a^{(j)} = g(z^{(j)})
  $$
  where function $g$ can be applied element-wise to vector $z^{(j)}$

* Add a bias unit (equal to $1$) to layer $j$ after we have computed $a^{(j)}$. This will be element $a_0^{(j)}$ and will be equal to $1$.

* Final result $h_\Theta(x) = a^{(j+1)} = g(z^{(j+1)}) = g(\Theta^{(j)}a^{(j)})$

