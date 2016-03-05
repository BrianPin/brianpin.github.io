---
layout: post
comments: true
title: Docker Installation For Deep Learning Course
---
# 1. Introduction
I have always been wondering to learn the Course of Deep Learning. So I started
to learn the Udacity course ud730. And the first significant hump I met is the
install of docker. From my understanding docker is just a Linux process. But I
found the whole docker community is way more complex and interesting. Here in
this note, I just want to write down knowledges about Docker when I was installing
tensorflow assignment.

## 1.1 Docker Container From Google Cloud Repository
I was following the instructions from this page:

> https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/udacity/README.md

To be able to work on the assignments, the tensorflow has to be installed. There
are three ways to install. I choose to install a pre-built docker container image.
After installing docker on my Linux box, this is the first command I executed:

> docker run -p 8888:8888 -it --rm b.gcr.io/tensorflow-udacity/assignments
