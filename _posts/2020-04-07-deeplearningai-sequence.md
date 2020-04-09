---
layout: post
title: "Deeplearning.ai: Sequence Models Coursera course"
image: "https://lh3.googleusercontent.com/uaM5d3QjZTcbFbcuMVkzi3t9U2Bpq3Y4KW1g1Cb8stpiIjzx4aVT2vSQwNjBcewVUUf8EVom30osMCuWJlRs_4p0epB-SA7cTrb2OB99vbJeC0tvD0oOD7QMX9xbhzcArOC1q1gQjcDCNyOG1YIP3fDTn6BZlxH-SQND7IiN7cFBaVzmFCMj3QkFW9VZtVOyxHPoJ_kV9Q9rbSydpDpfB7Hzj9F9M_S_RgSPUJ3gbTj89pqA7im1RgRIkwfpPbjUopK83KYn_ygE2QD5lEyTnHJ1ybEZKED60eBn8BJPmN4aueVMJrpiSfAM_IDDZN2nmr7vysfr78zD179d8BJ7Fof4iHl-HNdFO6-zLf3_uo2sv9uILz7Gafo1s9W4W1MpwWXKariTlkw-2vh20zFJf_H8o3_Pn_DQ6QH4FtyrHSn457EaOz9M3f8AbNtaXf16OwsaNviRbEX4GumQVpw1Q1zsEEohNezJmfBgPK1ZA2qb4xgZMLo8kqatZQMLdrxNmR66WRO-kUluMTtfZmQcyf8TrhDnQeK08aAxPKX_membnjwspRiLYiMPW9Lp6jrV4zZrnMp21TrOluIexpFDMYnChavyMdaNWGf9qlCyZZwciOgNQ3yJGlG5wQnFUxJ4aMPxvYOZFBZrB0qwg9mt5qGI-c6UcyUbdKlMaQkhQGnQ7O5AWVgyWzrqDzoXBVeCUJjQ0vYFXiTc1IhB5x1TuPaV3ZcOeCIdmlcb2o03HNlHP7QHk8pOVl0=s225-no" 
image-border-radius: 20
date: 2020-04-07 16:00:00 +01:00
categories: [Deep Learning]
tags: [deeplearning]
---
Some quick notes made during [Deeplearning.ai's Sequence Models](https://www.coursera.org/learn/nlp-sequence-models/home/welcome) on Coursera.

## Week 1

### Examples of sequence data
Sequence data: varying length data
- speech recognition
- music generation
- sentiment classification
- DNA sequence analysis
- Machine translation
- Video activity recognition
- Name entity recognition

### RNN
- $x^{(i) \langle n \rangle}$ : element $n$ of sequence $i$
- $T_x^{(i)}$: length of sequence $i$
- Representing words by index into dictionary; and convert index to one-hot
  - UNK: unknown word token in dictionary
  - EOS: stop of sentence token
- (normal) FCNN can't easily represent varying length sequences, and doesn't share features across several positions of text
- (Uni-directional) RNN passes on activation from previous timestep
  - initial activation is initialized randomly
  - parameters shared between timesteps

Forward propagation
- activation at time $t$: $a^{\langle t \rangle}=g_a\left(W_a\begin{bmatrix}a^{\langle t-1 \rangle} \\ x^{\langle t\rangle}\end{bmatrix}+b_a\right)$ 
- output at time $t$: $\hat{y}^{\langle 1\rangle}=g_y(W_{ya}a^{\langle 1 \rangle}+b_y)$
 
Backpropagation through time
- loss over time: mean individual losses per time step
- Recursive backpropagation

### RNN architecture types
- input and output doesn't have to be same ($T_x \neq T_y$),
- one-to-one: standard FCNN
- many-to-one: $T_x > T_y=1$, eg sentiment classification
- one-to-many: $T_x=1< T_y$, eg music generation, where outputs are often used as inputs for next time step
- many-to-many architecture: eg machine translation

Sequence generation: 
- input at timestep $t$ is output of timestep $t-1$, such that network outputs conditional probability over previous outputs
- During test time: Random sampling from the output softmax at each timestep
- sample until EOS token, or until max number of outputs, or resample till EOS
- character or word level model; word-level model more common

### Vanishing gradient problems with RNN
- standard RNN not capturing long-term dependencies well because of vanishing gradients, since it is effectively a very deep FCNN and error is hard to propagate all the way back to beginning
- exploding gradients less of a problem with RNN, eg by using gradient clipping

### GRU: Gated Recurrent Unit
- better captures long term dependencies
- 2014 Cho On the properties of neural machine translation: Encoder-decoder approaches
- 2014 Chung Empirical evaluation of gated recurrent networks on sequence modelling
- GRU uses memory cell $c$, an update gate $\Gamma_u$, and a relevance gate: 
  - Memory cell: $c^{\langle t\rangle}=\tanh(W_c[\Gamma_r*c^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_c)$
  - Update gate: $\Gamma_u=\sigma(W_u[c^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_u)$
  - Relevance gate: $\Gamma_r=\sigma(W_r[c^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_r)$
  - Memory cell update rule: $c^{\langle t\rangle }=\Gamma_u*c^{\langle t\rangle }+(1-\Gamma_u)*c^{\langle t-1\rangle }$
  - The update gate tells whether to "store" the new memory or keep the previous value
  - The relevance gate tells how relevant $c^{\langle t-1\rangle }$ is for calculating the next candidate of $c^{\langle t\rangle }$
  - GRU simplified version of LSTM

### LSTM: Long-Short Term Memory
- 1997 Hochreiter Long short-term memory
  - Memory cell: $c^{\langle t\rangle }=\tanh(W_c[a^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_c)$
  - Update gate: $\Gamma_u=\sigma(W_u[a^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_u)$
  - Forget gate: $\Gamma_f=\sigma(W_f[a^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_f)$
  - Output gate: $\Gamma_o=\sigma(W_o[a^{\langle t-1\rangle }, x^{\langle t\rangle }]^T+b_o)$
  - Memory cell update rule: $c^{\langle t\rangle }=\Gamma_u * c^{\langle t\rangle }+\Gamma_f * c^{\langle t-1\rangle }$
  - Activation: $a^{\langle t\rangle }=\Gamma_o*\tanh c^{\langle t\rangle }$
  - Good at passing on $c$
  - $\Gamma$ sometimes also includes $c^{\langle t-1\rangle }$ which is then called a peephole connection (peeks at the previous memory cell value rather than only previous activation and current input)
- No clear winner between LSTM and GRU; LSTM seems work well

### Bi-directional RNN
- adds a backwards recurrent layer
- acyclic graph
- $\hat{y}^{\langle t\rangle }=g(W_g[\overrightarrow{a}^{\langle t\rangle }, \overleftarrow{a}^{\langle t\rangle }])$
- Often used in NLP
- disadvantage: needs full sequence, so not useful in real-time

### Deep RNN
- stacking RNN layers
- $a^{[l]\langle t\rangle }=g(W_a^{[l]}[a^{[l]\langle t-1\rangle },a^{[l-1]\langle t\rangle }]^T+b_a^{[l]})$
- Usually not many deep recurrent layers as it is computationally expensive

## Week 2

### Word embeddings
- often models use pre-trained word embeddings instead of one-hot encodings of words
- unsupervised model learns the word embeddings that convey correlations from a large text corpus
- useful for transfer learning, and can be fine-tuned given a large enough corpus (not done often)
- To visualize word embeddings: dimensionality reduction with t-SNE (non-linear mapping): 2008 van der Maaten Visualizing data using t-SNE
- word embeddings allow for reasoning by analogy: 2013 Mikolov Linguistic regularities in continuous space word representations
- similarity measures: cosine of angle or squared distance between vectors

### Learning word embeddings
2003 Bengio A neural probabilistic language model
- learn an embedding matrix: $[E]=n_\textrm{emb space}\times n_\textrm{vocab size}$
- Input: one-hot encoding, Learned parameters: embedding matrix $E$, Output: softmax to predict a word close to context
- Input a context window, eg:
  - $n$ words on left and/or right to predict middle/next word 
  - Nearby 1 word: skip-gram model
  
Word2Vec: 2013 Mikolov Efficient estimation of word representations in vector space
- context $c$ to predict nearby word
- $o_c\rightarrow E \rightarrow \textrm{softmax}$
- Softmax is computationally expensive, need to sum over 10k entries (vocab)
- Solution: Hierarchical softmax (tree speeds up computation); most common words on top of tree
- Need heuristic to sample context, otherwise too many common words show up in context

Negative sampling: 2013 Mikolov Distributed representation of words and phrases and their compositionality
- generate positive and negative pairs of context and target word
- randomly sample target word for negative example
- Input: context and word, Output: predict if positive or negative example (logistic regression) 
- Use $k$ negative examples for each positive example
- How to choose negative examples:
  - sample according to frequency of vocab (too many simple words) or uniformly at random (not close to language)
  - sample according to $f^{3/4} works well empirically (between the two extremes)

GloVe: 2014 Pennington GloVe: Global Vectors for word representation 
- $X_{ij}$: number of times word $j$ appears close to word $i$
- minimize $\sum_{i=1}^{10k}\sum_{j=1}^{10k}f(X_{ij})(\theta_i^T e_j+b_i+b_{j'}-\log X_{ij})^2$, and a weighting term $f(X_{ij})=0$ if $X_{ij}=0$ to suppress divergence of log
- Learns how related are $\theta$ (context) and $e$ (target) according to $X_{ij}$
- Initialize $\theta$ and $e$ randomly; take average at end since $\theta$ and $e$ learn same embedding (symmetric in cost)
- Cannot learn fixed interpretable axes using the above algorithm, since can always multiply the vectors by some invertible matrix to keep cost the same

### Sentiment classification
Several models to do sentiment classification
- average word embeddings of sentence as input and predict eg star rating (ignores word order)
- use RNN of word embeddings (takes sequence into account)

### Debiasing word embeddings: 2016 Bolukbasi Man is to computer programmer as woman is to homemaker: Debiasing word embeddings
- Word embeddings reflect biases in written text
- Identify bias direction using singular value decomposition
- Neutralize: for every word that is not definitional (like grandmother/grandfather), project to get rid of bias
- Equalize pairs: gender should be only difference between grandmother/grandfather or girl/boy
- How to decide words to neutralize: authors trained a linear classifier which words should be consider definitional
- Handpick which pairs to equalize

## Week 3

### Sequence-to-sequence models
Image captioning
- 2014 Mao Deep captioning with multimodel recurrent neural networks
- 2014 Vinyals Show and tell: Neural image caption generator
- 2015 Karpathy Deep visual-semantic alignments for generating image descriptions
- input: image, output: caption of image

Machine Translation
- 2014 Sutskever Sequence to sequence learning with neural networks
- 2014 Cho Learning phrase representations using RNN encoder-decoder for statistical machine translation
- encoder RNN: inputs french sentence; decoder RNN: outputs english sentence
- Machine translation: a conditional language model $\textrm{argmax}_{y} P(y^{\langle 1\rangle },\ldots,y^{\langle T_y\rangle }\lvert x^{\langle 1\rangle },\ldots,x^{T_x})$
- How to get the most likely translation?
  - Greedy search: taking the max probability of $y$ at each step. This will not give the best sentence, since some individual words are much more likely given a previous context
  - Beam Search

### Beam Search
- Pick beam width, eg B=3: how many possibilities to consider (B=1 greedy search)
- Pick the top three word from $P(y^{\langle 1\rangle }\lvert x)$
- We want to know $P(y^{\langle 1\rangle },y^{\langle 2\rangle }\lvert x)=P(y^{\langle 1\rangle }\lvert x)P(y^{\langle 2\rangle }\lvert x,y^{\langle 1\rangle })$
- Pick next word given one of the three initial words $P(y^{\langle 2\rangle }\lvert x,y^{\langle 1\rangle })$
- repeat until EOS
- Instead of maximizing the product $\mathrm{argmax} \prod_{t=1}^{T_y}P(y^{\langle t\rangle }\lvert x,y^{\langle 1\rangle },\ldots,y^{\langle t-1\rangle })$ (requires many multiplies of small numbers), maximize the normalized (by $T_y^\alpha$) log probability
- The normalization makes the log probability on the same scale for short vs long sequences; empirically a softer normalization works best ($\alpha < 1$)
- How to choose B: production system ~10, research ~1k
- Beam search is faster than Breadth First and Depth First Search, but is not guaranteed to find exact maximum

### Error analysis on Beam Search
- was the issue because of RNN or Beam Search
- RNN computes $P(y\lvert x)$, so compare it for produced $\hat{y}$ and best/ground-truth $y^{*}$
  - if $P(y^{*}\lvert x)\rangle P(\hat{y}\lvert x)$: Beam search is likely at fault as it picked $\hat{y}$
  - conversely: RNN is at fault as it predicted $P(y^{*}\lvert x)\langle P(\hat{y}\lvert x)$
  - compute fraction of RNN/Beam search errors over examples, to decide what to improve

### Machine Translation metric: Bleu Score (Bi-Lingual Evaluation Understudy Score)
- Might have two equally good reference translation of a sentence
- 2002 Papineni A method for automatic evaluation of machine translation
  - count n-grams
  - clip count to maximum amount of times n-gram occurs in reference sentences
  - Blue score on n-grams: $p_n=\frac{\sum_{\textrm{n-grams}\in \hat{y}}\mathrm{Count}_ \mathrm{clip} (\textrm{n-gram})}{\sum_{\textrm{n-grams}\in \hat{y}}\textrm{Count} (\textrm{unigram})}$ measures a sense of overlap between sentences
  - Combined Blue Score: $\textrm{BP}\exp\left(\frac{1}{4}\sum_{n=1}^{4}p_n\right)$
  - Breyity penalty $\textrm{BP}$: short translations have higher precision; 1 if longer than reference, 0 if shorter than reference (see paper for exact conditions)

### Attention
- 2014 Bhadanau Neural machine translation by jointly learning to align and translate
- 2015 Xu Show attention and tell: neural image caption generation and visual attention
- Context vector: $c^{\langle t\rangle }=\sum_{t'}\alpha^{\langle t,t'\rangle }a^{\langle t'\rangle }$, reweights the activation with attention weights $\alpha$
- $\alpha^{\langle t,t'\rangle }=\textrm{softmax}(e^{\langle t,t'\rangle })$, where $e^{\langle t,t'\rangle }$ is determined by a single layer NN that takes previous state and the current activation as input
- quadratic cost ($T_x*T_y$) is you take previous state and current activation as input

### Speech Recognition
- 2006 Graves Connectionist temporal classification
- input timesteps (audio) usually much bigger than output (text)
- create character output that adds blank and space characters
- CTC cost collapses repeated characters not separated by a blank character