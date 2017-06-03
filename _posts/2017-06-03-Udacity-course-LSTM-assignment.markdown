---
layout: post
comments: true
title: Learning Notes on LSTM Using Tensorflow
mathjax: true
excerpt: ""
---

### Preparation

- Data source
  - Data is text8.zip downloaded from Mattmahoney.net
  - Data is a 100M characters, the first 1000 is prepared for validation, the remaining is for training
  - Since char is not able to do numerical calculation, each char will be finding a mapping id to represent itself.
  - Char's id is its numerical value substracting the first ascii char's numerical value
  - Batch generation
    - A batch is a group of char feed into Tensorflow graph at the same time
    - The batch size is defined as a constant number (64) no matter it is training text or a validation text
    - The way the tutorial extracts a batch is a technique of uniformly extracting: e.g:
    - In the following text, text length is 130, batch size is 5, segment is 130/5 => 26
    - Cursor content in the beginning is [0, 26, 52, 78, 104]
    - Cursor content in the next batch is [1, 27, 53, 78, 105]
```
abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz
a<-cursor                 a<-cursor                 a<-cursor                 a<-cursor                 a<-cursor
```
    - The uniformity is achieved by segregating the whole long text into segments
    - Each time when there is batch generation request, we use the cursor list to find the char and then generate the batch
    - In my example, the first batch is **[aaaaa]**, the next batch is **[bbbbb]** ...etc
    - The whole data batch generation can be forever even though the cursor moves beyond the segment or text boundary
    - Because we will modulo the index so when boundary is passed, the index will go back from the beginning
    - **But** the real batch is a **2D** tensor just like this:
```
batch index 0 [0, 0, 0, 1, 0, 0, 0, 0, 0]
batch index 1 [0, 0, 0, 0, 1, 0, 0, 0, 0]
batch index 2 [0, 0, 1, 0, 0, 0, 0, 0, 0]
batch index 3 [0, 0, 0, 0, 1, 0, 0, 0, 0]
batch index 4 [0, 0, 0, 1, 0, 0, 0, 0, 0]
```
    - The batch is a tensor with shape to be (batch_size, vocabulary_size) i.e (64, 26+1)


### Training Procedure
- Aribtrarily defined training steps to be 7001 by course teacher
- For each step, the training code fetch a number of unrolling batches to the graph. This is because LSTM's RNN nature. It needs to keep previous chars for calculation
- For example, in one of the training steps, we fed the graph with a list of length **num_unrolling+1**
  - `[batches[0], batches[1], batches[2], batches[3] .......... batches[num__unrolling]]`
    - Each batches[i] contains **64** 1-hot vectors converted from characters extracted from source uniformly distributed
    - `*a*  b  c  d  __first step__`
    - `          *d* e  f  g __second step__`

- Feeding the graph
  - `feed_dict[train_data[i]] = batches[i]`  This is in the session training loop
  - `train_data.append(tf.placeholder(...blah...))`   This is in the graph
  - All I think is that code is a bit of magic. All we can imagine is using this high level interpretation:
  - `train_data[i] = batches[i]`



