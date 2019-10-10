---
layout: post
title: 2. TensorFlow warmup
category: Deep Learning
tag: Deep Learning
---

# TensorFlow warmup

```python
# https://deeplearningcourses.com/c/deep-reinforcement-learning-in-python
# https://www.udemy.com/deep-reinforcement-learning-in-python
from __future__ import print_function, division
from builtins import range

import numpy as np
import tensorflow as tf
import q_learning


class SGDRegressor:
  def __init__(self, D):
    print("Hello TensorFlow!")
    lr = 0.1

    # create inputs, targets, params
    # matmul doesn't like when w is 1-D
    # so we make it 2-D and then flatten the prediction
    self.w = tf.Variable(tf.random_normal(shape=(D, 1)), name='w')
    self.X = tf.placeholder(tf.float32, shape=(None, D), name='X')
    self.Y = tf.placeholder(tf.float32, shape=(None,), name='Y')

    # make prediction and cost
    Y_hat = tf.reshape( tf.matmul(self.X, self.w), [-1] )
    delta = self.Y - Y_hat
    cost = tf.reduce_sum(delta * delta)

    # ops we want to call later
    self.train_op = tf.train.GradientDescentOptimizer(lr).minimize(cost)
    self.predict_op = Y_hat

    # start the session and initialize params
    init = tf.global_variables_initializer()
    self.session = tf.InteractiveSession()
    self.session.run(init)

  def partial_fit(self, X, Y):
    self.session.run(self.train_op, feed_dict={self.X: X, self.Y: Y})

  def predict(self, X):
    return self.session.run(self.predict_op, feed_dict={self.X: X})


if __name__ == '__main__':
  q_learning.SGDRegressor = SGDRegressor
  q_learning.main()
```

> result

```
Hello TensorFlow!
Hello TensorFlow!
episode:
0 total reward: 34.0 eps: 1.0 avg reward (last 100): 34.0
episode: 100 total reward: 199.0 eps: 0.09950371902099892 avg reward (last 100): 105.13861386138613
episode: 200 total reward: 176.0 eps: 0.07053456158585983 avg reward (last 100): 179.85148514851485
episode: 300 total reward: 178.0 eps: 0.0576390417704235 avg reward (last 100): 170.88118811881188
episode: 400 total reward: 199.0 eps: 0.04993761694389223 avg reward (last 100): 191.97029702970298
avg reward for last 100 episodes: 198.79
```
