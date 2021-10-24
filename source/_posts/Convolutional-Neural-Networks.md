---
title: Convolutional Neural Networks
date: 2021-10-24 18:07:02
tags: ['Machine Learning', 'Computer Vision']
categories: ['self-study notes']
cover: true
summary: CNN
img: /medias/featureimages/33.jpg
mathjax: true
---

#### 1. Edge Detection

* Vertical Edge Detection

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 4.36.38 PM.png" style="zoom:50%;" />

* Vertical and Horizontal Edge Detection

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 4.38.54 PM.png" style="zoom:50%;" />

  * By using different filter kernel.

#### 2. Padding

* If there is no padding:
  * shrink output
  * throw away info from edge
* **Zero-Padding**

<img src="/Users/apple/Desktop/selfstudy/CNN/pics/Screen Shot 2021-10-24 at 4.41.20 PM.png" style="zoom:50%;" />

* Suppose input image is $n\times n$, filter is $f\times f$, padding is $p$, then the output is $(n+2p-f+1)\times (n+2p-f+1)$
* **Valid and Same convolutions**
  * **Valid**: no padding, output $(n-f+1)*(n-f+1)$
  * **Same**: Pad so that output size is the same as the input size
    * $n+2p-f+1=n \Rightarrow p=\frac{f-1}{2}$
    * $f$ is usually odd

#### 3. Strided convolutions

<img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 4.48.50 PM.png" style="zoom:50%;" />

* $n\times n$ * $f\times f$

  padding $p$, stride $s$

  Output size: $\lfloor\frac{n+2p-f}{s}+1\rfloor \times \lfloor\frac{n+2p-f}{s}+1\rfloor$

#### 4. Convolutions over volumes

* Convolutions on RGB images

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 4.54.49 PM.png" style="zoom:50%;" />

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 4.57.05 PM.png" style="zoom:50%;" />

* Summary:

  $n\times n \times n_c$ * $f\times f \times n_c$ $\Rightarrow$ $(n-f+1) \times (n-f+1) \times n_c'$

  where $n_c'$ is the number of filters

#### 5. One layer of a convolutional network

* one example:

  input image $n\times n \times C$ $\rightarrow$ 10 filters $\rightarrow$ Relu(convolutional output $\lfloor\frac{n+2p-f}{s}+1\rfloor \times \lfloor\frac{n+2p-f}{s}+1\rfloor \times n_c'$ + bias $b$)

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 5.20.02 PM.png" style="zoom:50%;" />

* Number of parameters in one layer

  If you have 10 filters that are 3 x 3 x 3 in one layer of a neural network, how many parameters does that layer have?

  <img src="Convolutional-Neural-Networks/Screen Shot 2021-10-24 at 5.21.12 PM.png" style="zoom:50%;" />

* Summary of notation

  If layer $l$ is a convolution layer:
  $$
  \begin{align*}
  & f^{[l]} = \text{filter size}\\
  & p^{[l]} = \text{padding}\\
  & s^{[l]} = \text{stride}\\
  & n_c^{[l]} = \text{number of filters}\\
  & \text{Each filter is: } f^{[l]} \times f^{[l]} \times n_c^{[l-1]}\\
  & \text{Activations: } a^{[l]} \rightarrow n_H^{[l]} \times n_W^{[l]} \times n_c^{[l]}\\
  & \text{Weights: } f^{[l]} \times f^{[l]} \times n_c^{[l-1]} \times n_c^{[l]}\\
  & \text{bias: } (1,1,1,n_c^{[l]})\\
  & \text{Input: } n_H^{[l-1]} \times n_W^{[l-1]}\times n_c^{[l-1]}\\
  & \text{Output: } n_H^{[l-1]} \times n_W^{[l-1]}\times n_c^{[l]}\\
  \end{align*}
  $$

