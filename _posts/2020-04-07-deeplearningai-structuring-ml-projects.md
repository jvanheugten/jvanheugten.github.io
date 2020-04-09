---
layout: post
title: "Deeplearning.ai: Structuring Machine Learning Projects"
image: "https://lh3.googleusercontent.com/uaM5d3QjZTcbFbcuMVkzi3t9U2Bpq3Y4KW1g1Cb8stpiIjzx4aVT2vSQwNjBcewVUUf8EVom30osMCuWJlRs_4p0epB-SA7cTrb2OB99vbJeC0tvD0oOD7QMX9xbhzcArOC1q1gQjcDCNyOG1YIP3fDTn6BZlxH-SQND7IiN7cFBaVzmFCMj3QkFW9VZtVOyxHPoJ_kV9Q9rbSydpDpfB7Hzj9F9M_S_RgSPUJ3gbTj89pqA7im1RgRIkwfpPbjUopK83KYn_ygE2QD5lEyTnHJ1ybEZKED60eBn8BJPmN4aueVMJrpiSfAM_IDDZN2nmr7vysfr78zD179d8BJ7Fof4iHl-HNdFO6-zLf3_uo2sv9uILz7Gafo1s9W4W1MpwWXKariTlkw-2vh20zFJf_H8o3_Pn_DQ6QH4FtyrHSn457EaOz9M3f8AbNtaXf16OwsaNviRbEX4GumQVpw1Q1zsEEohNezJmfBgPK1ZA2qb4xgZMLo8kqatZQMLdrxNmR66WRO-kUluMTtfZmQcyf8TrhDnQeK08aAxPKX_membnjwspRiLYiMPW9Lp6jrV4zZrnMp21TrOluIexpFDMYnChavyMdaNWGf9qlCyZZwciOgNQ3yJGlG5wQnFUxJ4aMPxvYOZFBZrB0qwg9mt5qGI-c6UcyUbdKlMaQkhQGnQ7O5AWVgyWzrqDzoXBVeCUJjQ0vYFXiTc1IhB5x1TuPaV3ZcOeCIdmlcb2o03HNlHP7QHk8pOVl0=s225-no" 
image-border-radius: 20
date: 2020-04-07 12:30:00 +01:00
categories: [Deep Learning]
tags: [deeplearning]
---
Some quick notes made during [Deeplearning.ai's Structuring Machine Learning Projects Course](https://www.coursera.org/learn/machine-learning-projects/home/welcome) on Coursera.

## Week 1

### ML Strategy: Orthogonalization
- try to rune hyperparameters in a more-or-less independent way
- try to use single real number evaluation metric
- satisficing/optimizing metric: eg accuracy is optimizing metric, and run time or false positive rate is satisficing metric (only needs to be smaller than some threshold)
- development set or held out cross validation set

ML project:
- create fixed development set with clear metric
- 60/20/20 split is good for small datasets (<10k) or 98/1/1 if (>>10k)
- make test set large enough to give high confidence in overall performance
- in some cases test set can be ignored, if an unbiased estimate is not necessary (accuracy not that important)
- change dev/test if becomes clear if training set is not representative
- change metric if results are not alinged with goal: eg reweight importance based on business case

Training (mitigate Avoidable Bias):
- bigger networks
- opimization algorithm
- architecture search

Validation (mitigate variance):
- regularization
- bigger training set

Test:
- bigger validation set

Real world:
- change validation set
- change cost function

### Comparing to human-level performance
- Bayes optimal error: Best possible error of mapping X to Y
- get human feedback on errors and bias/variance
- human: a good baseline; if humans has much lower error than train: try reduce bias
- Avoidable bias: gap between human and training in error
- Variance: gap between training and validation
- Focus on where the biggest gap is
- DL often better at structured data

### Interview Andrej Karpathy
- works at OpenAI
- build your own human baseline, eg simple interface
- surprised about NNs as general feature extractor; transfer learning
- unsupervised learning is big challenge

Goal:
- Incorrect approach to try to do each tasks seperately and then combine; try to learn learning

Advice:
- understand low-level details

## Week 2

### Error analysis
- look at statistics of mislabeled/worst-performant examples and their prevalence
- find out how much performance gain can be achieved
- also count/clean up incorrectly labelled data
- Prioritize: eg If 10% overall dev set error, and incorrect labels is 6% of 10%, only 0.6%, so not worth too much
- perform analysis also on all train/dev/test sets
- also randomly check examples that are correct

Advice:
- start simple/quickly on a train/dev/test set and metric, iterate often
- use error analysis to prioritize next steps
- use bias/variance analysis

### Mismatched train and dev/test set
- make sure that dev/test represent what you aim for, which might require putting most data in dev/test and training to consist of some other data with a few examples of what you aim for
- balance sources of data across dev/test 
- if unbalanced data sources across train/dev/test: create a training-dev set you don't train on but has same distribution as training set to understand variance (gap train and train-dev) and data mismatch (gap train and dev)
- try to generate more training data similar to the dev/test (the goal)
  - artificial data (synthesized) - be careful of overfitting
  - data augmentation

### Learning from multiple tasks
- Transfer learning: use part of trained network to a new task
  - Improves data efficiency when going from a large data problem to a small data problem
- Multi-task learning: learning multiple tasks at same time
  - can learn on impartially labelled data
  - leverages shared features from multiple tasks
  - best if amount of labelled data is similar

### End-to-end learning
Learn a task from raw data directly to final task (eg audio -> transcript)
- less hand-designing of features
- works well because there is lots of data (audio, transcript), but not the intermediate traditional subtasks

When it doesn't work:
- subtasks can each have a lot of data, but not for the whole end-to-end task
- needs large amount of data
- excludes potentially useful hand-designed components

### Ruslan Salakhutdinov interview
- Director of research at Apple and prof at Carnegie Mellon University
- Master in Toronto; PhD with Hinton
- co-author RBMs
- RBM: how to use convolution; better ways to train; earlier had to pretrain, but after compute allowed training the whole RBM
- GAN, VAE, deep energy models: not fully figured out

Advice:
- try new things, try to innovate
- understand low-level; calculate backprop for convnets
- industry: more resources, academic: more long-term

Challenges:
- deep RL
- reasoning and NLP
- one-shot or transfer learning; low data regime