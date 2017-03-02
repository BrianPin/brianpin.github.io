---
layout: post
comments: true
title: Cross Entropy And Stochastic Gradient Descent
mathjax: true
excerpt: "Cross entropy plays a similar role in deep learning as well as square root error loss function"
---
# Introduction
Today I am going to write down the note about cross entropy and Stochastic Gradient Descent. Why? Because I feel they are kind of a bridge between a generic software engineer to begin a machine learning software engineer. Also it is because it just so happened today that I learned the concept of these two and I want to write down to help me remember in the future and share it with people who are not familiar but want to know it.

# Prerequisites
First, you need to have a little bit of math background in the area of geometry which you need to know what "gradient" means. Not just the formulas but the meaning of it in space. Second, you need to know what entropy means. Since most relevant area we are talking about is machine learning, so I assume people knows about similarity. This can be described in few different ways:
1. Assuming your data is y, and your model is W, the model generated out data is called y', which is a synthesized data. How do you describe and quantify the distance between real y and various generated model output data y'?
2. If we just talking about entropy, it is a value that represent the degree of randomness only with y. If we talk about cross-entropy, it is about how random y' is if y' is considered to compose y. That means, when y' needs large value of entropy to be y, then we say y' is not very close to y if there is a y" that needs smaller values of entropy that closer to y.
__#entropy__, __#cross-entropy__, __#gradient__

# Cross Entropy
* It is mainly used in the loss calculation between models and expected model output
* Cross entropy formula:

$$ cross-entropy = \sum_i \mathbf{y}log(\mathbf{y'}) $$

* y is the real observed data, y' is the model generated data. Both y and y' are in vectors form

# Stochastic Gradient Descent
. Gradient Descent: Aggregate all training samples output and then calculate the gradient

. Stochastic Gradient Descent: Update the gradient during each training samples iteration

# TensorFlow Side Notes
. Interactive sessions: Normally, when you focus the build the graph, or you don't have to check how the graph functions, you have to build the complete graph, before you can run the session. But, if you use interactive sessions, you can run the session without building the complete graph.

. Computational Graph: Although Numpy has optimized mathmatical calculations of TensorFlow, but it is still not enough because it will have too many context switches. So, in order to solve this problem, TensorFlow will read the whole computation and then delicate the one that needs heavy computation to the outside world of Python
