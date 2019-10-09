---
layout: post
title: 20. RBF Neural Networks
category: AI
tag: Reinforcement Learning
---

# RBF Networks
<a href="https://imgur.com/VWhFjl0"><img src="https://i.imgur.com/VWhFjl0.jpg" width="700px" title="source:imgur.com" /><a>
- RBF = Radial Basis Function
- Useful in RL
- 2 perspectives
  - Linear model with feature extraction, where the feature extraction is RBF kernel
  - 1-hidden layer neural network, with RBF kernel as activation function
- when we first learned about neural networks, we learned these in reverse order
- we first learned that a neural network is a nonlinear function approximator
- later, we saw that hidden units happen to learn features

## RBF Basis Function
- is a non-normalized Gaussian
<a href="https://imgur.com/KIW70Nr"><img src="https://i.imgur.com/KIW70Nr.png" width="400px" title="source:imgur.com" /><a>
- x = input vector
- c = center / exemplar(模范) vector
- Only Repends on distance between x and c, not direction, hence the term **radial**
- Max is 1, when x == c, approaches 0 as x goes further away from c

## How do we choose c?
- how many c's should we choose?
- **number of center / exemplar == number of hidden units in the RBF Network**
- Each unit will have a different center
- A few different ways to choose them

## Support Vecotr Machines
- SVMs also use RBF Kernels
- **# of exemplars == # of training points**
- in fact, with SVMs the exemplars are the training points
- this is why SVMs have fallen out of favour
- Training becomes O(N^2), prediction is O(N), N = # Training samples
- important piece of deep learning history
  - SVMs were once though to be superior
## Another Methods
- Just sample a few points from the state space
- can then choose the # of exemplars
- env.observation_sapce.sample()
- how many exemplars we choose is like how many hidden units in a neural network - it's hyperparameter that must be tuned

## Implementation
- we'll make use of sci-kit learn
- our own difect from-definition implementation would be unnecessarily slow
- RBFsampler uses a Monte Carlo algorithms(MC)

```python
from sklearn.kernel_approximation import RBFsampler
```

- Standard Interface

```python
sampler = RBFSampler()
sampler.fit(radw_data)
features = sampler.transform(raw_data)
```

## The rest we have done before
- now that we know how to transform the raw state, the rest we should be familiar with: Q-learning, linear function approximation with gradient descent
- Unlike a feedforward neural network, the features won't change as we learn
- exemplar we choose at the beginning will remain forever
- May seem restrictive, but works better than feedforward NN

## Old Perspective vs New Perspective
- we used linear functions with polynomial features before
- now we use RBF Kernel for features
- the other Perspective： 1-hidden layer neural network
- remember that in general, this is a nonlinear transformation -> linear model at the final layer
- Recall: dot product is a cosine distance: $a^T$b = IaI IbI cos(angle(a,b))
<a href="https://imgur.com/xdDOaAu"><img src="https://i.imgur.com/xdDOaAu.png" width="700px" title="source:imgur.com" /><a>
<a href="https://imgur.com/rwrtflS"><img src="https://i.imgur.com/rwrtflS.png" width="700px" title="source:imgur.com" /><a>

```
Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
