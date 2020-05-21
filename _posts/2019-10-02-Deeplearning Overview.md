---
layout: post
title: 16. DeepLearning Overview
category: Reinforcement Learning
tag: Reinforcement Learning
---

# Deep Learning
<a href="https://postimg.cc/gn4x7Fc6"><img src="https://i.postimg.cc/mgW7tB0V/413131312.png" width="700px" title="source:imgur.com" /><a>
- Deep Learning is a name for **Neural network**
- in its simplest form, it's just a bunch of logistic regressions stacked together
- layers in between input and output are called **hidden leyers**
- we call this network a "feedforward neural network"
- Nonlinear activation functions **(f)** make it a nonlinear function approximator
<a href="https://postimg.cc/RNfLj06b"><img src="https://i.postimg.cc/X711BGV3/4131313.png" width="500px" title="source:imgur.com" /><a>

## Training
- Despite the complexity, training hasn't changed since logistic regression, we still just do gradient descent
<a href="https://postimg.cc/fkF3Q463"><img src="https://i.postimg.cc/Yq2QgMRx/4124123131.png" width="300px" title="source:imgur.com" /><a>
- problem: not as robust with deep networks. sensitive to hyperparameters:
  - Learning rate, # hidden units, # hidden layers, activation fcn, optimizer(AdaGrad, RMSprop, Adam, etc) which have their own hyperparameters
  - we won't know what works until we try

## Feature Engineering
- As with all ML models, input is a feature vector x
- **Neural networks are nice because they save us from having to do lots of manual feature engineering**
- Nonlinear characteristics of NNs have been shown to learn features automatically and hierarchically between layers
- Ex
  - layer 1 : edges
  - layer 2 : groups of edges
  - layer 3 : eye, nose, lips, ears
  - layer 4 : entire faces

## Working with images
- as a human, one of our main sensory(感觉的) input
- as a robot navigating the real-world, images are also one of our main sensory input
- images also make up states in video games
- thus we'll need to know how to work with images to play Atary environments in Open Gym
- Luckily we have tools for this: **Convolutional neural network**
- Does 2-D Convolutions before the fully-connected layers
- (All layers in a feedforward network are fully-connected)
- Convolutional Layers have filter which are mush smaller than the image
- Also, instead of working with 1-D feature vectors, we work with the 2-D image directly(3-D if color)
<a href="https://postimg.cc/MnQhTBP0"><img src="https://i.postimg.cc/y8QBLX0G/412412313.jpg" width="700px" title="source:imgur.com" /><a>
- idea: slide kernal/filter across the image and multiply by a patch to get output
- Aside from the sliding, it works exactly like a fully-connected layer
- Multiplication and nonlinear activation
- Concept of **shared weights**
- smaller # of parameters, takes up less space and trains faster
<a href="https://postimg.cc/VJ4zqpPs"><img src="https://i.postimg.cc/Gpn9CnMD/412312312412.png" width="700px" title="source:imgur.com" /><a>

## Working with sequences
- In RL, not only are we interested in images, but sequences too
- **Main tool: recurrent neural networks**
- Episode is made of sequence of states, actions and rewards
- Any Network where a node loops back to an earlier node is recurrent
<a href="https://postimg.cc/0M1ffWsj"><img src="https://i.postimg.cc/vBcNGk2r/12312.jpg" width="700px" title="source:imgur.com" /><a>
- Typically don't think of individual recurrent connections, but recurrent layers or units, e.g **LSTM or GRU**
- we think of them as black boxs: input -> box -> outpu
- since output depends on previous inputs, it means the black box has "memory"
- useful because we can make decisions based on previous frames/ states
<a href="https://postimg.cc/18HnTvw4"><img src="https://i.postimg.cc/XvMfrhvK/12.png" width="700px" title="source:imgur.com" /><a>

## Classification vs Regression
- we mostly focused on multi-class classification
- **softmax on multiple output nodes + cross entropy**
- in RL we want to predict a real-value scalar(the value function)
- **one output node + squared error**

Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
