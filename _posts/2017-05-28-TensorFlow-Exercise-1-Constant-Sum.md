---
layout: post
title: TensorFlow - Exercise 1 - Sum of two numbers
tags: [ tutorial, machine-learning, tensorflow ] 
comments : true
---

**Exercise : Sum of two numbers**
    
Given two number say 5 and 15 find the sum of these two numbers. You are given the numbers to add in advance
and the numbers won't change.

**Excepted output**
20

# Caution : Solution for exercise is below. Please try solving problem by yourself before looking below


```python
# Code starts here
import tensorflow as tf

# As we are told that numbers won't change 
# so it's safe to assume it as tensorflow constant
a = tf.constant(5,name="a")
b = tf.constant(15,name="b")

# Now we have to add a and b.
# tenorflow provides add function for same.
# https://www.tensorflow.org/api_docs/python/tf/add
c = tf.add(a,b,name="c")

# Tensor flow creates graph and doesn't run the graph till
# you run the graph in a session. 
# So if we try printing value of c at this point
# we will get the output as a tensor and not actual value of c
# This is because value of c is not computed till we demand 
# value of c, (and this is done by sess.run() )
print("Value of c before running tensor:",c)
```

    Value of c before running tensor: Tensor("c_5:0", shape=(), dtype=int32)



```python
# A new session is created using tf.Session() call.
sess = tf.Session()

# now we need to run graph we created
# c is passed as input to run as we need 
# to run graph till value of c is obtained.
output = sess.run(c)
print("Value of c after running graph:",output)

# Once we are done we need to close the session.
sess.close()

# code ends here
```

    Value of c after running graph: 20


While learning tensorflow I kept hearing that tensorflow creates graph, but it was hard to visualize. I used to wonder if I can see the graph which tensorflow creates. And the answer is **YES**. We can actually see the graph created by tensorflow.

Tensorflow provides a tool called Tensorboard which can be used to see the graph created by tensorflow. Please google and learn about tensorboard in detail. Refer: [Tensorboard](https://www.tensorflow.org/get_started/summaries_and_tensorboard) for details on same.

For now, Let's check the graph created for above program by tensorboard. The graph created for above program is:

![Graph]({{site.url}}/images/P1.png)

As it can be seen in the graph that c is dependent on a and b. So when we execute **sess.run(c)** tensorflow evalutes c based on a and b. Tensor a and b are evaluted first and then c is computed based on value of a and b. 

Let's try last thing for this exercise. What happens if we define d and don't use value of d at all. Consider code below


```python
import tensorflow as tf

a = tf.constant(5,name="a")
b = tf.constant(15,name="b")
c = tf.add(a,b,name="c")
d = tf.sub(a,b,name="d")

sess = tf.Session()
output = sess.run(c);
print(output)
sess.close()
```

    20


In this case we are running session on c, so only c and all nodes on which c is dependent (a and b in our
case) are evaluated. d tensor (which is subtraction of a and b) is not computed at all. To evaluate d we need
either compute d in same or a new session like sess.run(d).

Hope you enjoyed this!. Please help me in improving this by sending feedback at mjain8@binghamton.edu
