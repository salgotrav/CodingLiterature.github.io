---
layout: post
title: TensorFlow - Exercise 2 - Variables in TensorFlow
tags: [ tutorial, machine-learning, tensorflow ]
comments : true
---

 **Exercise** : Generate a random array of size 100 and find mean of z as obtained from below equations:
    1. y = x*x + 5x
    2. z = y*y
    
  Implement both the equations seperately and * means element wise multiplication and not matrix multiplication. Below are some restrictions for this exercise:
   1. y should be of type tf.Variable()
   2. Use tf.random_normal() to get random tensor of size 100. Use seed as 12.

# **Caution : Solution for exercise is below. Please try solving problem by yourself before looking below**


```python
import tensorflow as tf

# get 100 random tensors. 12 is used as seed.
x = tf.random_normal([100], seed=12,name="x")
y = tf.Variable(x*x+5*x,name="y")
z = tf.multiply(y,y,name="z")
zmean = tf.reduce_mean(z)

init_op = tf.global_variables_initializer()

with tf.Session() as sess:
    sess.run(init_op) 
    zout = sess.run(zmean)
    print(zout)
```

    27.7974


**Summary of what each line does in above code:**

__Line 1__ : Import tensorflow library with alias tf.

__Line 4__ : Generate random tensor of size 100.

__Line 5__ : Create a variable y. Initial value of y is given as x*x+5*x.

__Line 6__ : Multiply y and y. 

__Line 7__ : Calculate mean of z.

__Line 9__ : When you launch the graph, variables have to be explicitly initialized before you can run Ops that use their value. You can initialize a variable by running its initializer.

__Line 11__ : Create a new session to run our graph.

__Line 12__ : Run the operation to initialize global variables.

__Line 13__ : Run graph to calculate value of zmean. This will calculate x, y and finally zmean.


For your own unerstanding you can try looking at graph of this code in tensorboard.
