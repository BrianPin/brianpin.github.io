---
layout: post
comments: true
title: Notes on Artificial Intelligence course of Udacity
use_math: true
---

### A.I Terminologies: Fully Observable, Partial Observable
1. A fully observable thing is when AI can determine the optimal decision when all the context needed to make that optimal decision is ready and readable
2. I think, the real world, is mostly not fully observable. But we humans can use our memory in our brain to construct the unobservable, plus the observable. This is how we get the decision
3. During the class teacher briefly mentioned about Hidden Markov Model. This makes me wonder, does AI use HMM to construct/predict unobservable data to make the optimal decision?

### Working On Deep Learning Assignment 5 (Udacity)
#### Python
1. list class can accept an iterator to construct a list object
2. Meet `class Counter` and `class deque`, Why the `Counter` class is in Uppercase?
3. `class Counter` is a subclass of `dict`
4. `class deque` is a optimized version of `list`
5. `class zipfile` is a standard library for python
6. package `six` is a feature that combines py2 in py3
7. package `six.moves` is a consistent method to load either py2 or py3 modules
8. Basic Python data types accept iterator as constructor parameter. (Like C++, so this is a pattern!)
9. Sometimes Python really confusing when does it return iterators and when does it return actual lists!

#### Text Processing
1. Using to `Counter(...)` to easily have statistics of all words
2. Using the `Counter.most_common` to easily get the most popular words
3. Use a `dict()` to map word to its ranking (order in popularity)
4. Add a list of rankings in the order of original word list
5. Add a `dict()` so we can map from ranking to actual word
6. Generate batch for Tensorflow

#### WordVec Using SkipGram with Tensorflow (Course Provided Example)
1. The generate_batch function

  * Word vector is built  for each `prominent` word
  * Each word vector is fixed length denoted by `num_skips`
  * For example: Word w, is represented by vector `[d1, d2, d3, d4]`, d1, d2, d3 and d4 are randomly chosen from the nearest 4 words surround w.
  * Udacity example is set the vocab size to 50,000 words. This will expand to (50,000 x 4) int vectors if the `num_skips` is set to 4

2. The training Model

  * Goal: giving a word, will know the highest probability of context surrounded words

#### WordVec Using CBOW
Since all the training procedures are similar, I just reversed the label and batch variable references in the original generate_batch function and call it 'generate_cbow_batch'

#### About tf.truncated_normal and tf.random_normal
1. In truncated_normal, we provide a standard deviation
2. All the values generated from truncated_normal are within the standard deviation
3. tf.random_normal does not have this feature
4. Using the truncated_normal is to prevent the model parameters overflow from training
5. [Andrej Karpathy's blog about backpropagation](https://medium.com/@karpathy/yes-you-should-understand-backprop-e2f06eab496b#.pzsn13f2q)

### Recurrent Neural Network (RNN) and Long Short-Term Memory (LSTM)
* In CNN the parameters are shared because near by space, this is somewhat a tying. In recurrent NN, we are going to use this tying again.
* To optimize sequence parameters we need to back propagate the derivative through time. Or in practice as many steps as we can afford.
* A lot of correlated updates. This is bad for stochastic gradient descent. Math is very unstable.
* Exploding gradient and vanishing gradient problems

 * The gradients grows exponentially or the gradients demimishes to zero

 * The gradient clipping (Only works for grow cases). This is kind of a simple hack.

 * For gradient vanishing problems, it is kind of memory loss. (This is where LSTM comes in)![alt text](lstm.png "LSTM")

 * There is a `memory cell` in the center, and `write`, `read` and `forget`

 * There are gates to control these above actions, and gate control is a continuous parameter

 * Means we can take derivative and back propagate

 * The gating values for each gate get controlled by a tiny logistic regression of each parameters
