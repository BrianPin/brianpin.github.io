---
layout: post
comments: true
title: Cross Entropy And Stochastic Gradient Descent
use_math: true
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

$H(y_i)= -\sum_i y_ilog(y_i')$
