---
layout: post
title: "Deeplearning.ai: Neural Networks and Deep Learning"
image: "https://lh3.googleusercontent.com/uaM5d3QjZTcbFbcuMVkzi3t9U2Bpq3Y4KW1g1Cb8stpiIjzx4aVT2vSQwNjBcewVUUf8EVom30osMCuWJlRs_4p0epB-SA7cTrb2OB99vbJeC0tvD0oOD7QMX9xbhzcArOC1q1gQjcDCNyOG1YIP3fDTn6BZlxH-SQND7IiN7cFBaVzmFCMj3QkFW9VZtVOyxHPoJ_kV9Q9rbSydpDpfB7Hzj9F9M_S_RgSPUJ3gbTj89pqA7im1RgRIkwfpPbjUopK83KYn_ygE2QD5lEyTnHJ1ybEZKED60eBn8BJPmN4aueVMJrpiSfAM_IDDZN2nmr7vysfr78zD179d8BJ7Fof4iHl-HNdFO6-zLf3_uo2sv9uILz7Gafo1s9W4W1MpwWXKariTlkw-2vh20zFJf_H8o3_Pn_DQ6QH4FtyrHSn457EaOz9M3f8AbNtaXf16OwsaNviRbEX4GumQVpw1Q1zsEEohNezJmfBgPK1ZA2qb4xgZMLo8kqatZQMLdrxNmR66WRO-kUluMTtfZmQcyf8TrhDnQeK08aAxPKX_membnjwspRiLYiMPW9Lp6jrV4zZrnMp21TrOluIexpFDMYnChavyMdaNWGf9qlCyZZwciOgNQ3yJGlG5wQnFUxJ4aMPxvYOZFBZrB0qwg9mt5qGI-c6UcyUbdKlMaQkhQGnQ7O5AWVgyWzrqDzoXBVeCUJjQ0vYFXiTc1IhB5x1TuPaV3ZcOeCIdmlcb2o03HNlHP7QHk8pOVl0=s225-no" 
image-border-radius: 20
date: 2020-04-07 11:00:00 +01:00
categories: [Deep Learning]
tags: [deeplearning]
---

Some quick notes made during [Deeplearning.ai's Neural Networks and Deep Learning Course](https://www.coursera.org/learn/neural-networks-deep-learning?specialization=deep-learning) on Coursera.

## Week 1

### Introduction
- ReLU: Rectified Linear Unit
- Dense layer: set of neurons

### Supervised Learning with Neural Networks
Supervised learning: input x -> output y
Examples:
- Home features -> Housing price
- Ad, user info -> Click on ad
- Image -> object position
- Audio -> Text transcript
- English -> Chinese

Structured data: Fully connected NN
Image data: CNN
Sequence data: RNN

Structured data: database data
Unstructured data: Audio, Images, Text

### Why is deep learning taking off?
- Amount of labelled data vs Performance graph
- Only in large data regime DL is often better than traditional ML
Reasons for progress:
- Data scale has driven DL progress
- Computation, since need to run experiments iteratively
- Algorithm improvements, allows larger and stable networks

### Hinton interview
Boltzman machines: Wake/Sleep algorithm
Restricted Boltzmann machines
Sigmoid Belief net
Variational Bayes
Paper on ReLU to show how it approximates logistic units
Initialize ReLU with identity: can train very good
Residual networks
Backpropagation in brain? -> recirculation idea
Spike-timing plasticity
Stack of RBM: reconstruction error derivative fo discriminative performance (might be how brain does it?)
Fast weights for recursion (multi-time scale)
Capsules: 
- how to represent multi-dimensional entities
- groups of units to different subsets, instance of features, all properties of feature
- routine by agreement: vote for face is based on capsule on mouth and capsule of eyes
- different way to train than simple backprop
Wegstein algorithm
Unsupervised learning crucial, not working yet
- GAN
- Variational Autoencoder
Mostly supervised learning has worked

Read literature but not too much
Trust intuition
Never stop programming
Find advisor/mentor with same beliefs

## Week 2

### Binary classification
- $m$: number of examples
- $n_x$: number of features 
- input matrix: $X \in \mathbb{R}^{n_x \times m}$
- output vector: $y \in \mathbb{R}^{1 \times m}$

### Logistic Regression
- Given x, want probability $\hat{y}=P(y=1\vert x)$
- Parameters: $w\in\mathbb{R}^{n_x}, b\in\mathbb{R}$
- Sigmoid: $\sigma(z)=\frac{1}{1+\exp(-z)}$
- $\hat{y}=\sigma(w^Tx+b)$

### Logistic Regression Cost Function
- Squared error makes gradient descent not work well (non-convex)
- Logistic or log loss: $\mathscr{L}(\hat{y},y)=-(y\log{\hat{y}}+(1-y)\log{(1-\hat{y})})$, convex
- Loss function: for single example
- Cost function: $J(w,b)=\frac{1}{m}\sum_{i=1}^m \mathscr{L}(\hat{y}^{(i)},y^{(i)})$
- Cost function: averaged loss over all examples

### Gradient Descent
Update parameters according to gradient to minimize J: 
- $w \leftarrow w-\alpha \frac{\partial J}{\partial w}$
- $b \leftarrow b-\alpha \frac{\partial J}{\partial b}$

### Computation graph & further
- Chain rule examples
- use shorthand: $\partial \mathrm{var}=\frac{\partial J}{\partial\mathrm{var}}$

### Vectorization
- vectorization to speed up forward/backprop
- avoid explicit for loops
- use broadcasting in numpy 
  - Can lead to subtle bugs
  - Take care with rank 1 arrays; better to use explicit rank 2
  - add asserts to test shape of arrays

### Explanation of logistic regression cost function
If $y=1$: $P(y\vert x)=\hat{y}$  
If $y=0$: $P(y\vert x)=1-\hat{y}$  
This function satisfies the above: $P(y\vert x)= \hat{y}^y (1-\hat{y})^{(1-y)}$  
Log function is strictly monotonically increasing function, so maximizing $\log{P(y\vert x)}$ is same as maximizing $P(y\vert x)$.
The log probability $\log{P(y\vert x)}$ then gives the logistic loss.
If the examples are i.i.d. (identically independently distributed), then the cost can be written as $\log{P(y\vert x)}=\log{\prod_{i=1}^{m}P(y^{(i)}\vert x^{(i)})}=\sum_{i=1}^{m}\log{P(y^{(i)}\vert x^{(i)})}$.  
This is called Maximum Likelihood Estimation.

### Pieter Abbeel interview
- reinforcement learning
- worked with Durant

Open questions:
- exploration problem
- credit assignment
- safety
- representation (can be with eep network)
- long time horizons

Can we learn reinforcement learning program? Meta-learning

Inspirations:
- John McCarthy
- Daphne Koller
- Andrew Ng

Advice:
- self-study
- Andrej Karpathy course
- Try things yourself
- Get mentor

Believes RL will use expert imitation as bootstrap, to avoid learning from scratch

## Week 3

### Notation
- $X$, $Z$, $A$: $[n_{feat}, n_{ex}]$
- $W$: $[n_{out}, n_{in}]$
- $a_4^{[1] (2)}$: fourth component of the activation of first layer for second example
- $Z^{[l]}=W^{[l]}A^{[l-1]}+b^{[l]}$

### Lecture 3.1-5
- hidden layer: values not in training set
- activation: the result of a node computation
- input layer is often not counted as a layer

### Lecture 3.6: Activation functions
- Use Tanh for bounded activation function, since mean is 0. 
- Sigmoid has mean 0.5, and mostly useful to output probabilities.
- ReLU most popular; no slope issues like with Tanh and Sigmoid
- LeakyReLU: $\max(0.01,z)$; often works better than ReLU, but not popular
- Without non-linearity: NN always linear function

### Lecture 3.11: Random Initialization
Important to randomly initialize with small numbers, since:
- hidden units will be identical, and thus will compute the same (even after backprop)
- If using Tanh/Sigmoid, then small number will keep activation function in linear range initially

### Interview: Ian Goodfellow
- built own GPU infrastructure
- Boltzmann machines; sparse coding; deep belief networks
- GAN: generative models
- working on stabilizing GANs
- works on adversarial examples; machine learning security

Advice:
- post on github
- work on a project
- write paper for arxiv


## Week 4

### Notation
- $l$: number of layers
- $n^{[l]}$: number of units in layer $l$
- Parameters: weights and biases
- Hyperparameters: learning rate, momentum, etc.

### Consistency check
- Check dimensions of matrices on paper

### Why _Deep_ Neural networks?
- feature composition
- circuit theory: shallow network require exponentially more hidden units to compute some functions
  - For example, XOR of all features can be done in O(log n) depth for deep network; single layer need $2^{n-1}$ features