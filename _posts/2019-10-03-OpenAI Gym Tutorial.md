---
layout: post
title: 17. OpenAI Gym Tutorial
category: AI
tag: Reinforcement Learning
---

# OpenAI Gym Tutorial
- Tutorial on the basics of Open AI Gym
- install gym : pip install openai
- what we'll do:
  - Connect to an environment
  - Play an episode with purely random actions
- Purpose: Familiarize ourselves with the API

## Import Gym
- First things :
```python
import gym
```

## Get our environment

```python
env = gym.make('CartPole-v0')
```
- Full list: https://gym.openai.com/envs
- has short blurbs(简介)
  - what the task is
  - whether it's been solved
  - leaderboard

## Start an episode
```python
# put ourselves in the start state
# it also return the state
env.reset()
# out: array([-0.04533, -0.032314, -0.0146921, 0.04151])
```

## What is the state?
- what do these numbers mean?
- https://github.com/openai/gym/wiki/CartPole-v0
- Documentation is somewhat sparse
<a href="https://imgur.com/PCxa56M"><img src="https://i.imgur.com/PCxa56M.png" width="500px" title="source:imgur.com" /><a>

## In the console

```python
Box = env.observation_space
# In : Box
# out : box(4,)
# in box.
# box.contains box.high, box.simple, box.to_jsonable, box.from_jasonable, box.low , box.shape
```
- The observation_space defines the structure of the observations our environment will be returning. Learning agents usually need to know this before they start running, in order to set up the policy function. some general-purpose learning agents can bandle a wide range of overvation types: Discrete, box, or pixels(which is usually a Box(0,255,[height,width,3])for RGB pixels)

## Action Space

```python
env.action_space
# in : env.action_space
# out : Discrete(2)
# in : env.action_space
# env.action_space.contains, env.action_space.n, env.action_space.to_jsonable, env.action_space.form_jsonable, env.action_space.sample
```

## play an episode

```python
observation, reward, done, info = env.step(action)
```
- Typically ignore info, since it can't be used in submission(屈服)(although it's possible it can help training)

## Finish an episode

```python
done = False
while not done:
  observation, reward, done, _ = env.step(env.action_space.sample())
```

- will end quickly since random actions can't keep the pole up for long

## Exercise
- determine how many steps, on average, are taken when actions are randomly sampled
- can be a benchmark to compare later algorithms

```python
# https://deeplearningcourses.com/c/deep-reinforcement-learning-in-python
# https://www.udemy.com/deep-reinforcement-learning-in-python
import gym
# Wiki:
# https://github.com/openai/gym/wiki/CartPole-v0
# Environment page:
# https://gym.openai.com/envs/CartPole-v0

# get the environment
env = gym.make('CartPole-v0')

# put yourself in the start state
# it also returns the state
env.reset()
# Out[50]: array([-0.04533731, -0.03231478, -0.01469216,  0.04151   ])

# what do the state variables mean?
# Num Observation Min Max
# 0 Cart Position -2.4  2.4
# 1 Cart Velocity -Inf  Inf
# 2 Pole Angle  ~ -41.8°  ~ 41.8°
# 3 Pole Velocity At Tip  -Inf  Inf

box = env.observation_space

# In [53]: box
# Out[53]: Box(4,)

# In [54]: box.
# box.contains       box.high           box.sample         box.to_jsonable
# box.from_jsonable  box.low            box.shape

env.action_space

# In [71]: env.action_space
# Out[71]: Discrete(2)

# In [72]: env.action_space.
# env.action_space.contains       env.action_space.n              env.action_space.to_jsonable
# env.action_space.from_jsonable  env.action_space.sample

# pick an action
action = env.action_space.sample()

# do an action
observation, reward, done, info = env.step(action)


# run through an episode
done = False
while not done:
  observation, reward, done, _ = env.step(env.action_space.sample())
```

```python
print(box.contains) # <bound method Box.contains of Box(4,)>
num_states=10**env.observation_space.shape[0]
num_actions= env.action_space.n
import numpy as np
a=np.random.uniform(low=-1, high=1, size=(num_states, num_actions))
len(a) #10000
observation_examples = np.array([env.observation_space.sample() for x in range(10000)])
observation_examples.shape
#(10000, 4)
```

```
observation_examples

array([[-9.7157693e-01, -7.4095410e+37, -4.1871965e-01,  6.7507521e+37],
       [-3.7798457e+00, -2.5458868e+38,  2.8346911e-01,  3.4012554e+38],
       [ 2.6570508e+00, -3.3872902e+38, -4.0225080e-01,  4.4559389e+37],
       ...,
       [-2.4882526e+00,  2.3085303e+38, -4.1779616e-01, -1.0087459e+38],
       [-3.6637935e-01,  8.6948986e+37, -2.4446332e-01, -3.9084766e+37],
       [ 2.8921950e+00,  3.1869669e+38, -2.1086818e-02, -8.7358295e+37]],
      dtype=float32)
```

```python
np.random.random((20000, 4))*2 - 1
a =np.random.random((20000, 4))*2 - 1
a.shape #(20000, 4)
```

```
array([[-0.24646925, -0.71345292, -0.13534508,  0.56783749],
       [ 0.94458905,  0.51363389,  0.16503288, -0.42810724],
       [ 0.09917716, -0.44244101, -0.46836587, -0.04689816],
       ...,
       [-0.59025553, -0.39560999,  0.35734313,  0.43326763],
       [ 0.03911455, -0.6548753 , -0.11969308,  0.91571025],
       [-0.10430864,  0.15491529, -0.73753709, -0.05311987]])
```

Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
