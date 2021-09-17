---
title: Neural Network
date: 2021-06-16 13:47:38
tags: ['Neural Network', 'Machine Learning']
categories: ['self-study notes']
cover:
summary: Neural Network
img: /medias/featureimages/23.jpg
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

#### 3. Cost Function

* First define a few variables need to use:

  * $L = \text{total number of layers in the network}$
  * $s_l = \text{number of units (not counting bias unit) in layer } l$
  * $K = \text{number of output units/classes}$

* In neural networks, we may have many output nodes. Denote $h_\theta(x)_k$ as being a hypothesis that results in the $k^{th}$ output.

* The cost function for neural networks is going to be generalization of the one used for logistic regression.
  $$
  J(\Theta) = -\frac{1}{m} \sum_{i=1}^{m} \sum_{k-1}^{K} \left[y_{k}^{(i)} \log((h_\Theta(x^{(i)}))_k) + (1-y_k^{(i)})\log(1-(h_\Theta(x^{(i)}))_k)\right] + \frac{\lambda}{2m} \sum_{l=1}^{L-1} \sum_{i=1}^{s_l} \sum_{j=1}^{s_l+1}(\Theta_{j, i}^{(i)})^2
  $$
  We have added a few nested summations to account for our multiple output nodes. In the first part of the equation, before the square brackets, we have an additional nested summation that loops through the number of output nodes.

* In the regularization part, after the square brackets, we must account for multiple theta matrices. The number of columns in our current theta matrix is equal to the number of nodes in our current layer (including the bias unit). The number of rows in our current theta matrix is equal to the number of nodes in the next layer (excluding the bias unit). As before with logistic regression, we square every term.

* Note:

  * the double sum simply adds up the logistic regression costs calculated for each cell in the output layer.
  * the triple sum simply adds up the squares of all the individual $\Theta$s in the entire network.
  * the $i$ in the triple sum does **not** refer to training example $i$.

#### 4. Backpropagation Algorithm

* "Backpropagation" is neural-network terminology for minimizing the cost function.

* Compute the partial derivative of $J(\Theta)$:
  $$
  \frac{\partial}{\partial \Theta_{i, j}^{(l)}}J(\Theta)
  $$

* Procedure:

  * Given training examples set $\{(x^{(1)}, y^{(1)})\cdots(x^{(m)}, y^{(m)})\}$
  * Set $\Delta_{ij}^{(l)} = 0$ for all $(l, i, j)$
  * For $i = 1$ to $m$:
    * Set $a^{(1)} = x^{(i)}$
    * Perform forward propagation to compute $a^{(l)}$ for $l = 2,3,\cdots, L$
    * Using $y^{(i)}$, compute $\delta^{(L)} = a^{(L)} - y^{(i)}$
    * Compute $\delta^{(L-1)}, \delta^{(L-2)}, \cdots, \delta^{(2)}$ using $\delta^{(l)} = ((\Theta^{(l)})^T\delta^{(l+1)})\ .*\ a^{(l)}\ .*\ (1-a^{(l)})$
      * The delta values of layer $l$ are calculated by multiplying the delta values in the next layer with the theta matrix of layer $l$. We then element-wise multiply that with a function called $g^{\prime}$, or g-prime, which is the derivative of the activation function $g$ evaluated with the input values given by $z^{(l)}$.
    * $\Delta_{ij}^{(l)} := \Delta_{ij}^{(l)} + a_j^{(l)}\delta_i^{(l+1)}$
  * $D_{ij}^{(l)} := \Delta_{ij}^{(l)} + \lambda \Theta_{ij}^{(l)} \text{ if } j \ne 0$
  * $D_{ij}^{(l)} := \Delta_{ij}^{(l)} \text{ if } j = 0$

* Intuitively, $\delta_j^{(l)}$ is the "error" for $a_j^{(l)}$ (unit $j$ in layer $l$). More formally, the delta values are actually the derivative of the cost function: $\delta_j^{(l)} = \frac{\partial}{\partial z_j^{(l)}}cost(t)$

