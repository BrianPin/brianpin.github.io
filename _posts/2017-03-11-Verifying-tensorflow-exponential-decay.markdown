---
layout: post
comments: true
title: Note on what do to verify tensorflow function exponential decay
mathjax: true
excerpt: "This show a basic structure of a tensorflow program"
---

I guess I am a little weird on the thing about learning. Exponential decay is a very small function of Tensorflow. Nobody will spend time making tensorflow programs just for this function. I did. Even though it is a small program but it also helps me understand a lot of insight into the tensorflow usage.

First let's see the code: (This code is best to run at an ipython notebook)
```python
import tensorflow as tf
import numpy
import matplotlib.pyplot as plt

learning_rates = []
initial_rate = 100.0
step_to_decay = 1
decay_rate = 0.8

with tf.Session() as sess:
  global_step = tf.placeholder(tf.int32, shape=[])
  learning_rate = tf.train.exponential_decay(initial_rate, global_step, step_to_decay, decay_rate)
  tf.global_variables_initalizer().run()

  for step in range(training_steps):
    learning_rate.append(learning_rate.eval(feed_dict={global_step: step}))

fig, ax1 = plt.subplot(1, 1)
fig.set_size_inches(10, 4)
ax1.plot(range(0, training_steps), learning_rates)
plt.show()
```

The result:
<div class="imgcap">
  <img src="/assets/learning_rate_decay.jpg">
  <div class="thecap">The decaying learning rate</div>
</div>

The take aways from this little program for me are:
1. Tensorflow programs can be as small as this is
2. We don't really need `tf.Graph().as_default()`
3. All the batched input use the placeholder
4. If your variable is a pure scalar, just use shape=[]
5. The global_step in the `tf.train.exponential_decay` argument is for keep informing the current progress of training steps
6. The tensors `learning_rate` is running by calling the `eval()` function
7. Plot can be fun
