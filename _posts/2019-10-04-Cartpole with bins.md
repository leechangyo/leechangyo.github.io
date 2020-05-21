---
layout: post
title: 19. Cartpole with bins
category: Reinforcement Learning
tag: Reinforcement Learning
---

# Cartpole with bins
<a href="https://imgur.com/7l1pWjn"><img src="https://i.imgur.com/7l1pWjn.png" width="400px" title="source:imgur.com" /><a>
- Add some complexity: Q-Learning using the tabular method
- why?
  - it's not necessary to do the most complicated thing possible
  - simple adjustments allow us to use what we know, and it works well
- CartPole state space is continuous / infinite
- But some states are more likely than others
- From the documentation, we can infer that some are impossible, since if we go past a certain angle/position, we lose
- very high velocity is likely impossible as well
- Cut up relevant part of the state space into boxes
- this image is a 3-D box (a 4-D box would look weird)
- now we have a discrete state space
- we just name these 0,1 etc
- now we can use the tabular method

## Hidden Complexities
- how do we choose upper and lower limits of the box?
  - Naïve approach: just try different numbers until it works
  - More complex: play some episode, plot histogram to see what limit are
- what if we land in a state outside the box?
  - Extend the edge boxes to infinity
    - ex) box0 - box1 - box2 - box3

## implementation
- Convert state into a bin
- Must be a unique number, so we can index a dictionary or array
- Not easy, so take our time to test out a few ideas

## Overwriting rewards
- Default reward is +1 for every step
- Doesn't work too well ( but makes perfect sense)
- Better: give a large negative reward (like -300) if the pole falls
- incentivizes the agent to not reach that point
- is modifying the default rewards a good idea or not?
- in the real world, if we are building an agent to solve a novel task, we would be defining the rewards anyway
- the programmer must define an intelligent reward structure

```python
# https://deeplearningcourses.com/c/deep-reinforcement-learning-in-python
# https://www.udemy.com/deep-reinforcement-learning-in-python
from __future__ import print_function, division
from builtins import range
# Note: you may need to update your version of future
#       if builtins is not defined
# sudo pip install -U future

import gym
import os
import sys
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from gym import wrappers
from datetime import datetime


# turns list of integers into an int
# Ex.
# build_state([1,2,3,4,5]) -> 12345
def build_state(features):
  #print(int("".join(map(lambda feature: str(int(feature)), features))))

  return int("".join(map(lambda feature: str(int(feature)), features)))

def to_bin(value, bins):
  return np.digitize(x=[value], bins=bins)[0]
# it takese in a value and then array of possible bins and figures out which bin the value belongs in


class FeatureTransformer:
  def __init__(self):
    # Note: to make this better you could look at how often each bin was
    # actually used while running the script.
    # It's not clear from the high/low values nor sample() what values
    # we really expect to get.
    # nine size because it given 10 bins
    self.cart_position_bins = np.linspace(-2.4, 2.4, 9)
    self.cart_velocity_bins = np.linspace(-2, 2, 9) # (-inf, inf) (I did not check that these were good values)
    self.pole_angle_bins = np.linspace(-0.4, 0.4, 9)
    self.pole_velocity_bins = np.linspace(-3.5, 3.5, 9) # (-inf, inf) (I did not check that these were good values)

  def transform(self, observation):
    # returns an int
    cart_pos, cart_vel, pole_angle, pole_vel = observation
    return build_state([
      to_bin(cart_pos, self.cart_position_bins),
      to_bin(cart_vel, self.cart_velocity_bins),
      to_bin(pole_angle, self.pole_angle_bins),
      to_bin(pole_vel, self.pole_velocity_bins),
    ])


class Model:
  def __init__(self, env, feature_transformer):
    self.env = env
    self.feature_transformer = feature_transformer

    num_states = 10**env.observation_space.shape[0]
    # 10000 num states
    num_actions = env.action_space.n
    # 2
    self.Q = np.random.uniform(low=-1, high=1, size=(num_states, num_actions))
    #2개 인덱스의 10000개 난수 생성

  def predict(self, s):
    x = self.feature_transformer.transform(s)
    return self.Q[x]

  def update(self, s, a, G):
    x = self.feature_transformer.transform(s)
    self.Q[x,a] += 1e-2*(G - self.Q[x,a])
    # this takens in as state action and target return

  def sample_action(self, s, eps):
    if np.random.random() < eps:
      return self.env.action_space.sample()
    else:
      p = self.predict(s)
      return np.argmax(p)


def play_one(model, eps, gamma):
  observation = env.reset()
  done = False
  totalreward = 0
  iters = 0
  while not done and iters < 10000:
    action = model.sample_action(observation, eps)
    prev_observation = observation
    observation, reward, done, info = env.step(action)

    totalreward += reward

    if done and iters < 199:
      reward = -300

    # update the model
    G = reward + gamma*np.max(model.predict(observation))
    model.update(prev_observation, action, G)

    iters += 1

  return totalreward


def plot_running_avg(totalrewards):
  N = len(totalrewards)
  running_avg = np.empty(N)
  for t in range(N):
    running_avg[t] = totalrewards[max(0, t-100):(t+1)].mean()
  plt.plot(running_avg)
  plt.title("Running Average")
  plt.show()


if __name__ == '__main__':
  env = gym.make('CartPole-v0')
  ft = FeatureTransformer()
  model = Model(env, ft)
  gamma = 0.9

  if 'monitor' in sys.argv:
    filename = os.path.basename(__file__).split('.')[0]
    monitor_dir = './' + filename + '_' + str(datetime.now())
    env = wrappers.Monitor(env, monitor_dir)

  N = 10000
  totalrewards = np.empty(N)
  for n in range(N):
    eps = 1.0/np.sqrt(n+1)
    totalreward = play_one(model, eps, gamma)
    totalrewards[n] = totalreward
    if n % 100 == 0:
      print("episode:", n, "total reward:", totalreward, "eps:", eps)
  print("avg reward for last 100 episodes:", totalrewards[-100:].mean())
  print("total steps:", totalrewards.sum())

  plt.plot(totalrewards)
  plt.title("Rewards")
  plt.show()

  plot_running_avg(totalrewards)
```

```
Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
