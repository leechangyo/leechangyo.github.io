---
layout: post
title: 22. Plug in Neural network and racap OPENAI
category: AI
tag: Reinforcement Learning
---

# Plugging in a neural network
- what might happen if we plugged in a neural network instead of a linear model?
- we have seen how we can plug in a tensorflow model into q_learning.py script(Tensorflow_warmup)
- put them together
- try removing RBF layer since presumably(很可能) a deep neural network will automatically learn features

## catastrophic(灾难性的) Forgetting
- lots of attention recently, in relation to "transfer learning"
- train AI on one game, keep some weights, train on another game
- showed the neural net can be trained such that the AI still performs well on the first game
- so it didn't just learn the 2nd game and forget the 1st game
- this is noteworthy(值得注意的) because it's not how neural networks work by default
- more typically we would expect the neural net to forget how to play the 1st game
- Doesn't only apply across different tasks
- Stochastic/ batch gradient descent -> cost can "jump" around
- seems more pronounced on highly nonlinear regression problems
- **we'd like the data in cost function to represent true distribution of data**
- even using all training data simultaneously is only an approximation of the "true" data
- E.g. clinical trial for a drug, we sample 1000 subjects. we hope they are representative of the population as a whole
- So when we use batch/Stochastic GD, the approximation becomes even worse
- in RL, it's even worse because there's a large bias in the ordering of training samples
- we always proceed from start state to end state
- No Randomization, which is recommended in SGD/BGD
- Also, not a true gradient because the target uses the model to make a prediction

## Try dropout
- Dropout has been shown to help
- some weights will not be updated for any particular training iteration
- try other types of regularization and architectures

# Open AI gym & RBF Network summary
- Building Gradually more and more complex RL agents, using what we already know

## OpenAI Gym
- how to load an environment
- inspect its state space, action space
- play an episode
- save a video

## CartPole
- Random search
- Easy to conceptualize, no complicated math
- **choose random weights until we find something good**

## Tabular Q-Learning
- Q-Learning with binned states
- Allows us to use tabular method
- Re-acquaint(再使熟悉) us with q_learning/TD(0)

## RBF Networks
- historically interesting, related to both SVMs and neural networks
- Could use nonlinear features, but still have a linear gradient descent algorithm
- Other Perspective: we're still doing linear regression, just with a different feature expansion

## Mountain Car
- Opportunity to test Q-Learning with RBF networks
- First use of approximation methods
- Choosing RBF exemplars, combining features, other sklearn tools

## Back to CartPole
- then applied the same algorithm to CartPole
- state space extends to infinity
- OpanAI gym samples from state space uniformly, not wrt(with regard to(…에 관해서)) probability of actually being in a state
- Not good exemplar
- Also: **built our own linear function approximator using numpy**

```
Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
