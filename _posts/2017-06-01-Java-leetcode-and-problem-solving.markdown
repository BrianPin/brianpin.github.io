---
layout: post
comments: true
title: Java, Leetcode and Problem solving
mathjax: true
excerpt: "Learning Java and problem solving"
---
# Java
## Algorithm Methods
- Sorting a list or an array can use these two methods:
 - Arrays.sort(array, Comparator<T>)
 - Collections.sort(list, Comparator<T>)
 - The Comparator interface is:
 ```java
    Comparator<T>() {
        @Override
        int compare(T t1, T t2) {

        }
    }
 ```
 - **Q1:** How come we can instantiate an interface like below?
 - `Comparator<T> c = new Comparator<T>(){...}`?

## Patterns and Language Features
- To initialize a list of objects:
 - Use Arrays.asList function: `List<T> A = Arrays.asList(obj1, obj2);`
 - __Or__ `List<T> A = Arrays.asList(value1, value2 ...);`
- To initialize an array of objects:
 - Use '{', '}' literal to surround new'ed objects
 - `T[] A = {new T(...), new T(...), new T(...), new T(...)}`
- When we call `List<T>::add(t)`, we are not creating a new T, we just copy the reference
- So when the reference changed in the new List, the original object will change too!!

# Leetcode and Problem Solving
## LC 57 **Insert Interval**
```java
public class Solution {
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        Collections.sort(intervals, new Comparator<Interval>(){
            @Override
            public int compare(Interval x, Interval y) {
                if (x.start < y.start)
                    return -1;
                else if (x.start == y.start) {
                    if (x.end < y.end)
                        return -1;
                    else if (x.end == y.end)
                        return 0;
                    return 1;
                }
                return 1;
            }
        });
```
- The above has customized the compare method in order to be sorted in the way I wish.
 - I want to sort in way that when an interval has smaller start value, it will be in the front
 - I also want to sort in a way that when two intervals has same start value, the one that has smaller end value will be in the front

```java
        List<Interval> res = new ArrayList<Interval>();
        boolean has_added = false;
```

- General algorithm description
 - Loop invariant: whenever we see the res list, it is always in a merged way!!!
 - We start by looping through the original array, for each interval in the array we do some checks:
  - Call the array element interval as **element_i**
  - if **newInterval** has not overlapped with **element_i**
   - if **element_i** is before the **newInterval**, we insert the **element_i**
   - if **element_i** is after the **newInterval**, we insert both if **newInterval** has not been inserted before
  - if **newInterval** is overlapping with the **element_i**, we do not insert but just update the boundaries of **newInterval**

```java
        for (int i = 0; i < intervals.size(); i++) {
            if (intervals.get(i).end < newInterval.start)
                res.add(new Interval(intervals.get(i).start,intervals.get(i).end));
            else if (intervals.get(i).start > newInterval.end) {
                if (!has_added)
                    res.add(new Interval(newInterval.start, newInterval.end));
                res.add(new Interval(intervals.get(i).start,intervals.get(i).end));
                has_added = true;
            }
            else {
                newInterval.start = Math.min(newInterval.start, intervals.get(i).start);
                newInterval.end = Math.max(newInterval.end, intervals.get(i).end);
            }
        }
        if (!has_added) {
            res.add(new Interval(newInterval.start, newInterval.end));
        }
        return res;
    }
}
```
