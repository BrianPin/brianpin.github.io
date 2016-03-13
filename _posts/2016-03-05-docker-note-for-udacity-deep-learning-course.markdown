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

Here is what the command means:

+ docker run [doc](https://docs.docker.com/engine/reference/commandline/run/)
+ -p 8888:8888 [port mapping]
+ -it The -it instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container. [stackoverflow](http://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty)
+ --rm [remove when exit]
+ b.gcr.io [google cloud repository](https://cloud.google.com/container-registry/docs/)
- [*docker stop*](https://www.ctl.io/developers/blog/post/gracefully-stopping-docker-containers/)
   * SIGKILL: It will let Kernel kill the process without talking to process
   * SIGTERM: It will ask Process: Would you mind to stop?
   * docker kill ----signal=SIGINT foo
   * docker stop ----time=30 foo **Use SIGTERM then SIGKILL**
- [*docker ps*](http://www.liquidweb.com/kb/how-to-list-and-attach-to-docker-containers/)
   * docker ps
   * docker ps -a
   * docker ps -l
+ /tensorflow-udacity/assignments

This directory contains a Dockerfile. This file has the following content:


```
FROM b.gcr.io/tensorflow/tensorflow:latest
MAINTAINER Vincent Vanhoucke <vanhoucke@google.com>
RUN pip install scikit-learn
ADD *.ipynb /notebooks/
WORKDIR /notebooks
CMD ["/run_jupyter.sh"]
```

Imagine docker container is just a process. The "./run_jupyter.sh" is just a process that we asked to run. This will run jupyter ipython command and brings up the ipython process. Which listening on port 8888 is the default port of a notebook server [jupyter doc](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html)

+ The assignment container mount from local directory
> docker run -p 8888:8888 -v "/home/spin/z/tensorflow/tensorflow/examples/udacity:/notebooks" -it --rm b.gcr.io/tensorflow-udacity/assignments

## 1.2 Hotkey when running iPython notebook

+ [ipython hotkey](https://www.webucator.com/blog/wp-content/uploads/2015/07/IPython-Notebook-Shortcuts.pdf)
+ [re-reading stdout,stderr for the existing container](https://docs.docker.com/engine/reference/commandline/logs/)

# 2. Summary

+ Figure out what composes a container? Is it a process command? Is it a binary or shell script?
