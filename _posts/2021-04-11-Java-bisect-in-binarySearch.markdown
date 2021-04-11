---
layout: post
comments: true
title: Doing Bissect in Java collections and arrays
mathjax: false
excerpt: ""
---


I found sometimes it is needed for me to do a bisect instead of binarySearch in Java. For example, in this question
https://leetcode.com/problems/count-of-smaller-numbers-after-self/
The intuitive idea is to scan the original array from the tail and put the element into a growing sorted array .
In order to be precise and quick, I use binary search to find the place to put the element in the growing sorted array.
But regular java binary search does not provide a formal way when there are multiple element you found in the collection.
for example:
in this list: A: [1, 2, 5, 7, 7, 7], the value 7 is started from index 3, to index 5, when you do Java's binarySearch, it is not 
guranteed which index it will return.
I here now provide a solution. 

# Using the comparator.

```java
        Comparator<Integer> bisectComparator = (l, r) -> {
            if (l == r)
                return 1; // note that we have to return a positive number so to let the search go left
            return l - r;
        };
```

In the above comparator, when we found the element, i.e. the case when l == r, we didnt return 0;
Instead we return a positive integer to let the search continue.

```java
int pos = Collections.binarySearch(l, 7, bisectComparator);
```

# Whats the trick

The trick is, you will always not get a positive return, you have to do -(pos + 1) to see the actual index it want to insert.
And there are couple cases when you deal with poss:

1. the element on -(pos + 1) is what you are searching for, then you are on the left most index of that element
2. the element on -(pos + 1) is not what you are looking, then it is also the position you should insert your element
3. the element on -(pos + 1) is actually same as the length of the searching, where you should append (increase the size of the list)



