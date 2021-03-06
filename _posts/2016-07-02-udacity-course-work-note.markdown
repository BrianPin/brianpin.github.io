---
layout: post
comments: true
title: Numpy functions used in the course
mathjax: true
excerpt: "Note down functions used in Tensorflow that I first met in the udacity course"
---

# Machine Learning Libraries and Functions Used in Assignment

## 1 Numpy and some python

  * scipy reading an image from file and return a numpy ndarray(N-dimensional array)

```Python
ndimage.imread(image_file)
```

  * Convert the data type in the numpy ndarray to desired type

```python
ndimage.imread(image_file).astype(float)
```

  * Mathematic operation to every element in the vector or matrix

```python
ndimage.imread(image_file).astype(float) - pixel_depth/2
```

  * Numpy index slicing, since there are 3 dims, the following is selecting the first dim from 0 to num_images-1

```python
dataset = dataset[0:num_images, :, :]
```

  * Python pickle dump/serialize data to file

```python
#Serializing to a file
pickle.dump(dataset, f, pickle.HIGHEST_PROTOCOL)
#De-serializing from a file
with open(pickle_file, 'rb') as f:
  letter_set = pickle.load(f)
```

  * Making numpy N-dimensional arrarys

```python
dataset = np.ndarray((nb_rows, img_size, img_size), dtype=np.float32
labels = np.ndarray(nb_rows, dtype=np.int32)
```

  * Two methods random shuffle of the data

```python
#Method 1 letter_set is numpy ndarray
np.random.shuffle(letter_set)
#Method 2 Using index slicing to randomize the data
permutation = np.random.permutation(labels.shape[0])
shuffled_dataset = dataset[permutation,:,:]
```
