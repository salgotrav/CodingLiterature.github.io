---
layout: post
title: TensorFlow - Exercise 3 - Counting
tags: [ tutorial, machine-learning, tensorflow ]
comments : true
---

**Exercise : Counting**
    
Count upto 5 in tensorflow

**Expected Output**
1
2
3
4
5

# Caution : Solution for exercise is below. Please try solving problem by yourself before looking below

```python
import tensorflow as tf

# Initialize count with value 0
count = tf.Variable(0)

newVal = tf.add(count,1)
# Assign operation updates the value of ref
# https://www.tensorflow.org/api_docs/python/tf/assign
assignment = tf.assign(count,newVal)

with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    
    # Now update the count 5 times
    for count in range(5):
        print(sess.run(assignment))
```
    1
    2
    3
    4
    5
