---
layout: post
comments: true
title: A C++ class for Topological sort
mathjax: true
---

This is my first github blog. Maybe let me start with some thing simple.
A Topological sorting class in C++:
{% gist ae6a0bd49ef497bfa4be %}

Essentially, if you want to do it by your own and using your own language.
You need to do a DFS search and when a node on the graph has finished the visiting,
just put the node to the prepared list container and when finished, return the
container.

So what is it so special in my code? Well, nothing big but this one is written in C++
and allows you to use your struct/class directly. I will create internal Graph class and
Node class for my algorithm usage and return the std list for you.

Later on, I will put the test case and the result back to this blog
