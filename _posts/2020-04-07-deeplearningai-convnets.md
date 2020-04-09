---
layout: post
title: "Deeplearning.ai: Convolutional Neural Networks"
image: "https://lh3.googleusercontent.com/uaM5d3QjZTcbFbcuMVkzi3t9U2Bpq3Y4KW1g1Cb8stpiIjzx4aVT2vSQwNjBcewVUUf8EVom30osMCuWJlRs_4p0epB-SA7cTrb2OB99vbJeC0tvD0oOD7QMX9xbhzcArOC1q1gQjcDCNyOG1YIP3fDTn6BZlxH-SQND7IiN7cFBaVzmFCMj3QkFW9VZtVOyxHPoJ_kV9Q9rbSydpDpfB7Hzj9F9M_S_RgSPUJ3gbTj89pqA7im1RgRIkwfpPbjUopK83KYn_ygE2QD5lEyTnHJ1ybEZKED60eBn8BJPmN4aueVMJrpiSfAM_IDDZN2nmr7vysfr78zD179d8BJ7Fof4iHl-HNdFO6-zLf3_uo2sv9uILz7Gafo1s9W4W1MpwWXKariTlkw-2vh20zFJf_H8o3_Pn_DQ6QH4FtyrHSn457EaOz9M3f8AbNtaXf16OwsaNviRbEX4GumQVpw1Q1zsEEohNezJmfBgPK1ZA2qb4xgZMLo8kqatZQMLdrxNmR66WRO-kUluMTtfZmQcyf8TrhDnQeK08aAxPKX_membnjwspRiLYiMPW9Lp6jrV4zZrnMp21TrOluIexpFDMYnChavyMdaNWGf9qlCyZZwciOgNQ3yJGlG5wQnFUxJ4aMPxvYOZFBZrB0qwg9mt5qGI-c6UcyUbdKlMaQkhQGnQ7O5AWVgyWzrqDzoXBVeCUJjQ0vYFXiTc1IhB5x1TuPaV3ZcOeCIdmlcb2o03HNlHP7QHk8pOVl0=s225-no"
image-border-radius: 20
date: 2020-04-07 15:00:00 +01:00
categories: [Deep Learning]
tags: [deeplearning]
---

Some quick notes made during [Deeplearning.ai's Convolutional Neural Networks Course](https://www.coursera.org/learn/convolutional-neural-networks/home/welcome) on Coursera.

## Week 1

### Computer vision
- image classification
- object detection
- neural style transfer

Convolution:
- fully connected not feasible for large images: too many parameters
- Old CV: Sobel/Scharr filter for edge detection, new CB: learn filter/kernel
- Convolution of $n_h\times n_w \times n_c$ input with $f\times f \times n_c\times n_f$ filters gives $\lfloor\frac{n_h+2p-f}{s}+1\rfloor \times \lfloor\frac{n_w+2p-f}{s}+1\rfloor\times n_f$ output with padding $p$ and stride $s$
  - "valid" padding: no padding $p=0$
  - "same" padding: pad to keep output size as input $p=(f-1)/2$
  - common to use odd $f$ in CV; easily divisible and has center point
  - parameter sharing: feature detector is useful in many positions in the image
  - sparsity of connections: each output value depends only a small number of inputs
  - translational equivariance
- technically the convolution in DL is called cross-correlation; since mathematical convolution requires flipping matrix so that associativity holds

Pooling:
- output size: $\lfloor\frac{n_H - f}{s} + 1\rfloor\times\lfloor\frac{n_W - f}{s} + 1\rfloor\times n_C$
- to reduce dimensionality, more robust features
- several types: max/min/average pooling
- max pooling is more often used
- average pooling sometimes used to collapse representation deep in the network
- no learnable parameters; fixed function

### Interview: Yann LeCun
- studied electrical engineering; chip design
- Hired by Bell Labs
- inspired by Piaget vs Chomsky: nurture vs nature
  - Fukushima: neocognitron (hierarchical architecture without backprop)
  - Optimal Perceptual Inference: Hinton - hidden units
  - Rumelhart, Hinton paper on backprop
- Bell Labs: LeNet on hand drawn digits
- worked with engineers Chris Dodgers, Krieg Nolan on char recognition
- created djvu format
- works at FAIR (facebook research), NYU
- Schoepfer CTO Facebook: background in open source

- contribute to open source; you'll get noticed and enter community

## Week 2

### Classic Networks
- LeNet5: 1998 LeCun Gradient-based learning applied to document recognition
  - conv avgpool conv avgpool fc fc output
  - nonlinearity after pooling (not common)
  - complicated way to deal with channels because of memory constraints
  - height and width go down, but channels increase (still common)
- AlexNet: 2012 Krizhevsky ImageNet classification with deep convolutional networks
  - conv maxpool conv maxpool conv conv conv conv maxpool fc fc fc 
  - 60M parameters
  - split across 2GPU because of memory constraints
  - local response normalization (not common anymore, doesn't help much)
- VGG16: 2015 Simonyan Very deep convolutional networks for large-scale image recognition
  -  always simple convolutions: 3x2 s=1 same and maxpool 2x2 s=2
  -  2x conv maxpool 2x conv maxpool 3x conv pool 3x conv pool 3x conv pool 2x fc
  -  138M parameters
- ResNets: 2015 He Deep residual networks for image recognition 
  - Add activations to a deeper layer: $a^{[l+2]}=g(z^{[l+2]}+ W_s a^{[l]})$, where $W_s$ is a learned/fixed matrix to match the matrix sizes
  - skip connections to avoid exploding/vanishing gradients, though some evidence that the mapping of the identity function is more important for performance
  - Allows to train much deeper networks
  - Identity function is easy to learn ($W^{[l+2]}=b^{[l+2]}=0$), while in general network would take many steps to learn the identity
- 1x1 convolutions or "Network in network": 2013 Lin Network in network
  - Adds over the channels, ie, fully connected neural network only over channel direction
  - Useful for reduction over the number of channels
  - Used in Inception
- Inception: 2014 Szegedy Going deeper with convolutions
  - Inception layer: 
    - Do all types of layers (1x1 convolution, a few fxf convolutions, maxpool, ..)
    - Reshape each output to match dimensions
  - Large computational cost for fxf convolutions can be reduced by using 1x1 convolutions first as a bottleneck
  - For maxpool, you can add 1x1 conv after pooling to reduce channels
  - Inception has side branches for prediction, seems to help for regularization

### Practical advice for using ConvNets
- look online/github and run to get a quick idea
- transfer learning using public pre-trained models
  - freeze all layers except last layer and retrain
  - or use as feature extractor and train a new model using those features
  - or use model as initialization to retrain
- Data Augmentation:
  - random cropping
  - color shifting; rgb shifts drawn from tight distribution
    - PCA color augmentation (see AlexNet paper) to keep color tint the same
  - rotation/shearing/local warping (used less)
  - use CPU threads to do augmentation to run in parallel to GPU

### Doing well on benchmarks
- ensembling outputs
- multi-crop at test time: run on multiple versions of test images and average results
- pretrained models and fine-tune

## Week 3

### Object Localization
Bounding boxes
- class and bounding box prediction
- $[p_c, b_x, b_y, b_h, b_w, c_1, .., c_n]$, where $p_c$ is probability of object present
- squared error for bounding box, logistic loss for probability, cross-entropy loss for the classes

Landmark detection
- probability of face, and find particular points, eg, points around the eyes
- used in snapchat, or for pose detection, etc.

Object detection
- Sliding window detection: feed resized crops of images and classify
  - disadvantage: computational cost; size of box is fixed
- Convolutional sliding window detection: 2014 Sermanet Overfeat: Integrated recognition, localization and detection using convolutional networks
  -  convert fc to conv layer (and use 1x1 convolutions for subsequent fc layers): $[n_\textrm{neurons}] \rightarrow [1,1,n_\textrm{neurons}]$
  -  runs several crops at same time and shares computation for each crop
  -  output will be $[n_{crops}, n_{crops}, n_{outputs}]$
- YOLO: 2015 Redmon You Only Look Once: Unified real-time object detection
  - split image into eg 9 grid cells (usually finer grid cell)
  - apply training to each grid cell with bounding boxes $[p_c, b_x, b_y, b_h, b_w, c_1, .., c_n]$, to output $[3,3,8]$
  - use midpoints to associate prediction with grid cell
  - outputs precise bounding boxes
  - problem: dealing with multiple objects in grid cell
  - fast algorithm (real-time)
  - some dimensions of bounding box ($h>1$ or $w>1$) can be outside cell (height and width specified relative to cell) 
- Intersection over Union (IoU)
  - used to calculate overlap of bounding boxes
  - size of overlap/size of union
- Non-max suppression
  - resolves multiple identical detections of same object
  - multiple grid cells might indicate same object
  - high IoU overlap detections are discarded based on probability of detection
  - Discard boxes with $p_c<0.6$, pick box with largest $p_c$, discard remaining boxes
- Anchor boxes
  - used to detect two objects per cell
  - have two different shaped default (anchor) boxes to annotate your dataset
  - output for each type of anchor box: $[h_\textrm{grid},w_\textrm{grid},n_\textrm{anchors}, n_\textrm{bbparam}+n_\textrm{classes}]$
  - problem: doesn't work with >2 objects; same anchor box shape causes problem
  - can use k-means to choose anchor boxes on dataset
- R-CNN (Region proposal): 2015 Girshik Rich feature hierarchies for accurate detection and semantic segmentation
  - run segmentation algorithm, then run object detection on separate blobs
  - rather slow algorithm, but several faster versions made
    - Fast R-CNN: 2015 Girshik Fast R-CNN
    - Faster R-CNN: 2016 Faster R-CNN: Towards real-time object detection with region proposal networks


## Week 4

### Face Recognition
- Face verification (easier: $1:1$ problem)
- Face recognition (harder: $n_\textrm{persons}:1$ problem)
- liveness detection also possible

One-shot learning
- recognize using only one example
- learn similarity function $d(\textrm{img}_1, \textrm{img}_2)$: degree of difference between images
- Siamese network: 2014 Taigman DeepFace closing the gap to human level performance
  - train network so that encodings have small norm: $\lVert f(x^{(i)})-f(x^{(j)})\rVert$

Triplet loss: 2015 Schroff FaceNet: A unified embedding for face recognition and clustering
- three images: anchor, positive, negative example
- train such that $\frac{\lVert f(A)-f(P)\rVert}{d(A,P)} + \alpha \leq \frac{\lVert(A)-f(N)\rVert}{d(A,N)}$
- $\alpha$: a margin, like in SVM, to avoid zero or the same embedding $f(A)=f(P)=f(N)$
- $\mathscr{L}(A,P,N)=\max\left(\lVert f(A)-f(P)\rVert^2 - \lVert(A)-f(N)\rVert^2 + \alpha,0\right)$
- In training set, you need multiple pictures for each person; but not for test time
- Can't pick A,P,N randomly during training as it would be too easy; Need to pick triplets that are hard to train on; pick according to $d(A,P)\approx d(A,N)$

Face Verification using Binary Classification (see DeepFace paper)
- train two branches of NN with tied weights for each image and a logistic output for if face is similar or not
- cache the embeddings for test time speed

### Neural Style Transfer
Neuron visualization: 2013 Zeiler Visualizing and understanding convolutional networks
- find image patch (receptive field) maximizes each unit's activation and visualize patches

Neural style transfer: 2015 Gatys A neural algorithm of artistic style
- Use pretrained NN
- Initialize G randomly, Minimize J(G)
- cost function: $J(G) = J_\textrm{content}(C,G)+\beta J_\textrm{style}(S,G)$, G: generated image, C: content image, S: style image
- The content cost forces the generated activations to be similar to the content activations: $J_\textrm{content}(C,G)=\frac{1}{2}\lVert a^{[l] (C)} - a^{[l] (G)}\rVert^2$
- Define style as correlation between activations between channels
- Style (or Gram) $n_c^{[l]}\times n_c^{[l]}$ matrix: $G_{kk'}^{[l]}=\sum_{i=1}^{n_H}\sum_{j=1}^{n_W}a_{ijk}^{[l]}a_{ijk'}^{[l]}$ measures correlation (actually un-normalized cross-covariance) between channel/filters
- The style cost over multiple layers: $J_\textrm{style}(S,G)=\sum_l\lambda^{[l]}\lVert G^{[l] (S)}-G^{[l] (G)}\rVert_F^2$ forces the activations of the generated image and style image to be correlated in the same way

### 1D and 3D generalization
- 1D convolutions useful on time series
- 3D convolutions useful for volumetric data

