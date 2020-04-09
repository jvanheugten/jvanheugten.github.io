---
layout: post
title: "Deeplearning.ai: Improving Deep Neural Networks"
image: "https://lh3.googleusercontent.com/uaM5d3QjZTcbFbcuMVkzi3t9U2Bpq3Y4KW1g1Cb8stpiIjzx4aVT2vSQwNjBcewVUUf8EVom30osMCuWJlRs_4p0epB-SA7cTrb2OB99vbJeC0tvD0oOD7QMX9xbhzcArOC1q1gQjcDCNyOG1YIP3fDTn6BZlxH-SQND7IiN7cFBaVzmFCMj3QkFW9VZtVOyxHPoJ_kV9Q9rbSydpDpfB7Hzj9F9M_S_RgSPUJ3gbTj89pqA7im1RgRIkwfpPbjUopK83KYn_ygE2QD5lEyTnHJ1ybEZKED60eBn8BJPmN4aueVMJrpiSfAM_IDDZN2nmr7vysfr78zD179d8BJ7Fof4iHl-HNdFO6-zLf3_uo2sv9uILz7Gafo1s9W4W1MpwWXKariTlkw-2vh20zFJf_H8o3_Pn_DQ6QH4FtyrHSn457EaOz9M3f8AbNtaXf16OwsaNviRbEX4GumQVpw1Q1zsEEohNezJmfBgPK1ZA2qb4xgZMLo8kqatZQMLdrxNmR66WRO-kUluMTtfZmQcyf8TrhDnQeK08aAxPKX_membnjwspRiLYiMPW9Lp6jrV4zZrnMp21TrOluIexpFDMYnChavyMdaNWGf9qlCyZZwciOgNQ3yJGlG5wQnFUxJ4aMPxvYOZFBZrB0qwg9mt5qGI-c6UcyUbdKlMaQkhQGnQ7O5AWVgyWzrqDzoXBVeCUJjQ0vYFXiTc1IhB5x1TuPaV3ZcOeCIdmlcb2o03HNlHP7QHk8pOVl0=s225-no" 
image-border-radius: 20
date: 2020-04-07 12:00:00 +01:00
categories: [Blog, Deep Learning]
tags: [deeplearning]
---

Some quick notes made during [Deeplearning.ai's Improving Deep Neural Networks Course](https://www.coursera.org/learn/deep-neural-network/home/welcome) on Coursera.

## Week 1
- train/dev/test sets: 60/20/20 if small; 70/20/10 if large dataset
- make sure dev+test come from same distribution
### Bias/Variance
- high bias: underfitting (if high error on train set)
- high variance: overfitting (if large discrepancy in error between train and validation)
- Optimal (Bayes) error: often assumed to be 0% error
- Linear classifier: high bias (underfits)
- Can be high bias + high variance at same time depending on regions of space

If high bias (better train performance):
- bigger network
- larger learning rate
- NN architecture search
- Try to fit training set well

If high variance (better val set performance):
- get more data
- regularization
- NN architecture search

With DL there is less of a bias-variance trade-off; can be done independently if regularized well

### Regularization
Weight decay:
- L2 regularization: $\frac{\lambda}{2m}\lVert W\rVert_2^2$ (or Frobenius norm for matrices)
- L1 regularization: makes weights more sparse than L2
- Larger lambda: will reduce capacity of network, by reducing effect of parameters
- If weights stay small then layers in linear range -> less expressive

Debugging full gradient descent: should reduce monotonically

### Dropout
- probabilistically remove units
-  Same as assumption of gaussian prior over weights in Bayesian framework (see Hinton course); $\lambda$ can be related to ratio of standard deviations of the assumed weight prior and the squared error
- smaller networks are being trained
- Inverted dropout: multiply activations by boolean array and scale activations by keep probability (to keep expected value of activations the same, so no scale difference)
- At training time: no dropout and scaling ('effective' averaging)
- Dropout can be shown to be adaptive form of L2 regularization
- Only use if algorithm is overfitting
- Cost function less well-defined as cost function changes at each iteration; harder to debug (so switch off dropout)

### Other Regularization
- data augmentation: artificially enlarge dataset
- early stopping: stop training when val set starts to increase
  - prevents weights to not grow to large
  - downside: mixes optimization of cost function and reducing overfitting
  - If you have compute, better to use L2 regularization and train longer

### Normalize inputs
- z-score the dataset: $\frac{x-\mu}{\sigma}$
- gradient descent performs better if features on same scales since learning rate is same in all directions

### Vanishing/Exploding gradients & Weight Initialization
- simple to understand in linear case without bias (just multiply scale of weight matrix)
- weight initialization: set variance of weight init to $1/n$ (or $2/n$ for ReLU) because pre-activation consists of summing $n$ weights

### Gradient Checking for debugging backprop
- Numerical approximation of gradients: $\frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2\epsilon}\approx f'(\theta)$
1. Reshape weights and bias to single vector $\theta$
2. Then calculate numerical approximation of derivative of cost
3. Then check $\frac{\lVert d\theta_{approx}-d\theta\rVert_2}{\lVert d\theta_{\approx}\rVert_2 + \lVert d\theta\rVert_2}$
4. If issues, then look at components to see where backprop goes wrong

Othogonalization
First optimize cost function, then reduce overfitting

### Interview Yoshua Bengio
- read Hinton, Rummelhart

Discoveries:
- curse of dimensionality; joint distribution of many variables from nets
- long-term dependencies
- stack of AE, RBM
- denoising, attention, 
- attention: allows creation of many types of data structures

- Current research: combining ideas unsupervised and reinforcement learning
- work on reasoning
- sequential processing of information

Advice:
- Try out on toy problems
- Find out more why something works
- Try to derive from first principles
- Try understand each part; why you are doing things
- look at other people's code
- Read Proceedings of ICLR for overview of field
- learn about optimization, calculus, probability

## Week 2

### Mini-batch gradient descent
- Batch gradient descent: full data at each iteration
- Mini-batch gradient descent: random sample of full data at each iteration
- typically take mini-batch sizes so that it fits in CPU/GPU memory in powers of two
- Also good to use as hyperparameter
- Take exponentially weighted average of losses, etc: $x_t = \beta x_{t-1} + (1-\beta) \theta_t$, averages over approximately $1/(1-\beta)$; computationally efficient
- Bias correction in EMWA: write $x_t/(1-\beta^t)$ to get better estimates at low $t$
- Stochastic gradient descent: $m=1$ only a single example per mini batch

Procedure:
- shuffle the data
- partition the data

### Optimization
#### Gradient descent with momentum:
- Take EMWA of gradient $V_{dW}=\beta V_{dW} + (1-\beta)dW$ and change weight to $W=W-\alpha V_{dW}$

#### RMSProp:
- EMWA of square of derivatives $S_{dW}=\beta S_{dW} + (1-\beta)dW^2$ and change weight to $W=W-\alpha \frac{dW}{\sqrt{S_{dW}+ \epsilon}}$
- Dampens oscillations; allows larger learning rate due to stability

#### Adam optimization algorithm:
- Combination of RMSProp and Momentum
- $W=W-\alpha \frac{V_{dW}}{\sqrt{S_{dW}+\epsilon}}$, where both parameters are corrected using the bias correction $S_{dW}=S_{dW}/(1-\beta^t)$
- Good values: $\epsilon=10^{-8}$, $\beta_V=0.9$, $\beta_S=0.999$

#### Learning rate decay
Several ways to do:
- $\alpha=\frac{\alpha_0}{1+\textrm{decay\_rate}*\textrm{epoch\_number}}$
- $\alpha = 0.95^{\textrm{epoch\_number}}\alpha_0$ or $\frac{k}{\sqrt{t}}\alpha_0$
- low on list to use, but helps in final accuracy

#### Local optima
- used to be big worry for neural networks, however, most minima are saddle points because of the large dimensionality

### Interview Yuangqing Lin
- Physics background
- Liza 
- Head of China's National Lab
  - Build largest DL infrastructure
  - Provide reproducible code
  - Combine infrastructure

Advice:
- try out the frameworks
- compare yourself to public benchmarks
- learn basic machine learning: PCA, LDA, etc


## Week 3

### Hyperparameter optimization
- Try not to optimize too many at same time
- Grid used to be used, but doesn't take into account importance of hyperparameters
- Random is better: many different value combinations sampled
- Zoom in after a course sample
- use log scaling for learning rate, etc.
- Train many models in parallel

### Normalization of activations
- more stable and faster learning
- Batch normalization, apply before activation (more often done), still a debate
  - Calculate $\mu$ and std $\sigma$ of the pre-activations then normalize $Z_\textrm{norm}=\frac{Z-\mu}{\sqrt(\sigma^2 + \epsilon)}$, and write $\tilde{Z}=\gamma Z_\textrm{norm} +\beta$ with $\gamma$ and $\beta$ learnable parameters
  - remove bias from Z calculation, as the mean is already included in batch norm
  - at test time use EMWA of $\mu$ and $\sigma$ calculated during training
- Batch norm works because:
  - same to input distribution might change from train to test: covariate shift
  - previous layer is constantly changing due to training, batch norm keeps mean and std in beginning the same
  - regularization effect: mean and variance are calculated on the mini batch, thus some noise exists. Adding some noise forces hidden units not to rely too much on a single unit (depends on mini batch size)

### Softmax
- multi-class classification: $\textrm{softmax}(z^{[i]})=\frac{\exp{z^{[i]}}}{\sum_i \exp{z^{[i]}}}$
- softmax cross-entropy loss: $\mathscr{L}(y,\hat{y})=-\sum_j y_j\log{\hat{y}_j}$

### Criteria for DL frameworks
- ease of development
- ease of deployment
- running speed/scaling
- truly open source

