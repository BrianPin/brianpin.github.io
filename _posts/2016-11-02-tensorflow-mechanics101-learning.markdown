---
layout: post
comments: true
title: Tensorflow Mechanics 101 Learning Notes
mathjax: true
---
### What we are doing

Use Tensorflow to build a 2-layer neural network and training classification models to recognize digits from 1 to 10

### Training Flow Overview

1. find out "tensorflow/examples/tutorials/mnist/fully_connected_feed.py"
2. find out "tensorflow/examples/tutorials/mnist/mnist.py"
3. File in 1. does the overall training flow operation
4. File in 2. implements the function to 'inference', 'training' and 'evaluation'
5. Training is done use 'Stochastic Gradient Descent'
6. The key-point in Tensorflow is: we need to define every tensor operation only then we can construct the graph using the defined tensor operation and running the session.

#### Training One-Iteration In Short

```python
# Part 1. forward tensor
hidden1 = tf.nn.relu(tf.matmul(images, weights1) + biases1)
hidden2 = tf.nn.relu(tf.matmul(hidden1, weights2) + biases2)
logits = tf.matmul(hidden2, weights3) + biases

# Part 2. getting cross entropy
cross_entropy = tf.nn.sparse_softmax_cross_entropy_with_logits(logits, labels)
loss = tf.reduce_mean(cross_entropy)

# Part 3. getting gradient
optimizer = tf.train.GradientDescentOptimizer(learning_rate)
train_op = optimizer.minimize(loss)
```
The above is a unit-step for training. Each training iteration will run through the above 3 parts in order

#### Training Overall Flow In Short

```python
# Part 1. setting tensors operation for the graph
with tf.Graph().as_default():
  logits = mnist.inference(image_input, flags...)
  loss = mnist.loss(logits, labels)
  train_op = mnist.training(loss, flag.learning_rate)
  eval_correct = mnist.evaluation(logits, labels)
  summary_op = tf.merge_all_summaries()
  init = tf.initialize_all_variables()
  saver = tf.train.Saver()
  sess = tf.Session()
  summary_writer = tf.train.SummaryWriter(...)
  sess.run(init)  # only run once!!

  # Part 2. let the tensor flow in the graph
  for step in xrange(max_step):
    feed data ...
    _, loss_value = sess.run([train_op, loss], feed_dict = data_dict)
    summary_writer.add_summary(summary_str, step)
    summary_writer.flush()
```

The above is a smapshot of the whole training flow. I felt it is a little bit confusing as to all these tensor operations. Are they actual multi-dimensional vectors? Or are they actually function objects that can be called during the graph running session?

### API Notes

1. tf.name_scope
2. tf.truncated_normal
 * _meaning_: Generate a normal distribution into 1-D array
 * _usage_ _note_: Parameter shape stores the generated integer values
3. tf.nn.relu
 * _meaning_: Computes rectified linear max(features, 0)
 * _usage_ _note_: 'features' in paramter is of shape 1-D?
4. tf.matmul
5. tf.nn.sparse_softmax_cross_entropy_with_logits
 * _meaning_: Computes softmax cross entropy between logits and labels. Measure probability error
 * _usage_ _note_: MUST not input a softmax-ed value because there is a softmax operation inside this function
6. tf.train.GradientDescentOptimizer
7. tf.merge_all_summaries
8. tf.initialize_all_variables
 * _meaning_: Returns an Op that initializes all variables


### References
[official doc](https://www.tensorflow.org/versions/r0.10/tutorials/mnist/tf/index.html)
