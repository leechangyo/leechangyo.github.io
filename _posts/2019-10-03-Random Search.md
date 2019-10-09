---
layout: post
title: 18. Random Search within OpenAI
category: AI
tag: Reinforcement Learning
---

# Random Search
- just add a little code to do random search for a linear model
  - state.dot(params) > 0 -> do action 1
  - state.dot(params) < 0 -> do action 0

```pseudocode
For # of times i want to adjust the weights
  new_weights = random
  For # of episodes i want to play to decide whether to update the weights
    Play episode
  if avg episode length > best so far:
    weithgs = new_weights
Play a final set of episode to see how good my best weights do again
```

> Random search

```python
# https://deeplearningcourses.com/c/deep-reinforcement-learning-in-python
# https://www.udemy.com/deep-reinforcement-learning-in-python
from __future__ import print_function, division
from builtins import range
# Note: you may need to update your version of future
# sudo pip install -U future

import gym
import numpy as np
import matplotlib.pyplot as plt


def get_action(s, w):
  return 1 if s.dot(w) > 0 else 0
# value function


def play_one_episode(env, params):
  observation = env.reset()
  done = False
  t = 0

  while not done and t < 10000:
    env.render() #<- window will pop up and we be able to see a video of the episode as it being played
    t += 1
    action = get_action(observation, params)
    observation, reward, done, info = env.step(action)
    if done:
      break

  return t


def play_multiple_episodes(env, T, params):
  episode_lengths = np.empty(T)

  for i in range(T):
    episode_lengths[i] = play_one_episode(env, params)

  avg_length = episode_lengths.mean()
  print("avg length:", avg_length)
  return avg_length


def random_search(env):
  episode_lengths = []
  best = 0
  params = None
  for t in range(100):
    new_params = np.random.random(4)*2 - 1
    avg_length = play_multiple_episodes(env, 100, new_params)
    episode_lengths.append(avg_length)

    if avg_length > best:
      params = new_params
      best = avg_length
  return episode_lengths, params


if __name__ == '__main__':
  env = gym.make('CartPole-v0')
  episode_lengths, params = random_search(env)
  plt.plot(episode_lengths)
  plt.show()

  # play a final set of episodes
  print("***Final run with final weights***")
  play_multiple_episodes(env, 100, params)

```

```python
import numpy as np
np.empty(100)
```

```
array([ 6.34918891e+151,  5.40618860e+257,  1.16543025e+166,
        2.27438781e+161,  4.11368246e+223,  1.17122224e+166,
        4.82407136e+228,  2.35953965e+232,  1.66880539e-307,
        3.51863529e-315,  4.26638573e-270,  3.71557101e-164,
        4.26638934e-270,  7.89737178e-251,  2.53069372e-212,
        7.72812543e-319,  6.95335581e-309, -2.47148131e-258,
        5.09636100e+173,  2.12611159e+289,  5.40552416e+173,
        4.52018132e+202, -1.71405763e-284,  4.03775671e+202,
        1.74294797e+252,  4.03775703e+202,  4.51999087e+202,
        3.32524435e-188,  3.03195438e-212,  4.03173663e+019,
        4.51999834e+202, -1.79811855e-284,  1.32031518e-019,
        2.85697648e+289,  2.00807226e+289,  2.43400036e+159,
        6.01334412e-154,  5.04344070e+180,  7.29489665e+175,
        1.42244599e+214,  2.45945760e+198,  1.96086579e+243,
        1.69505627e+190,  2.49970340e+262,  6.24527202e-085,
        3.67285416e+194,  1.27919412e-152,  7.20358919e+159,
        2.31634007e-152,  5.82147482e+180,  2.35625381e+251,
        3.81187276e+180,  1.27966001e-152,  4.77092211e+180,
        6.01346953e-154,  9.30350598e+199,  2.05947607e-027,
        1.30389145e-076,  5.66774924e+160,  1.28625507e+248,
        4.47590761e-091,  1.18600496e-259,  1.94861571e-153,
        6.78690190e+199,  6.01347002e-154,  9.82391515e+252,
        1.23875453e-259,  8.90429111e+247,  1.27874063e-152,
        1.94903487e+227,  1.27827553e-152,  1.45729796e-094,
        2.13945866e+161,  6.01347002e-154,  2.31649991e-152,
        2.31463556e-152,  1.68821824e+195,  8.72944715e+183,
        1.14490518e+243,  2.60985693e+180,  2.44015014e-154,
        6.01347002e-154,  9.05293030e+223,  1.69593624e-152,
        5.81816253e+180,  2.64520780e+185,  6.32266889e+180,
        3.17095857e+180,  7.22941924e+159,  5.98150411e-154,
        1.79805224e+044,  2.45943259e+198,  9.75015749e+199,
        3.62483719e+228,  6.97379781e+228,  1.97717050e+161,
        1.46923002e+195,  2.44048419e-152,  2.42766858e-154,
        6.01347002e-154])
```

## How to save a video
- importantm because it allows us to wath the agent play and see what it has learned through human eyes
- states, actions, rewards are abstract -> this is not a bad thing because it gives us a very powerful framework that makes this all possible
- but it leaves some unanswered questions
- has the agent learned to play as a human would?
- Does it play better?
- Does it use unconventional move?
- Main Changes

``` python
import gym
from gym import wrappers
env = gym.make('CartPole-v0')
env = wrappers.Monitor(env, 'my_unique_folder')
observation = env.reset()
while not done:
  action = choose_action()
  observation, reward, done, info = env.step(action)
  if done:
    break

```

> Random search and Save video


```python
# https://deeplearningcourses.com/c/deep-reinforcement-learning-in-python
# https://www.udemy.com/deep-reinforcement-learning-in-python
from __future__ import print_function, division
from builtins import range
# Note: you may need to update your version of future
# sudo pip install -U future

import gym
from gym import wrappers
import numpy as np
import matplotlib.pyplot as plt


def get_action(s, w):
  return 1 if s.dot(w) > 0 else 0


def play_one_episode(env, params):
  observation = env.reset()
  done = False
  t = 0

  while not done and t < 10000:
    t += 1
    action = get_action(observation, params)
    observation, reward, done, info = env.step(action)
    if done:
      break

  return t


def play_multiple_episodes(env, T, params):
  episode_lengths = np.empty(T)

  for i in range(T):
    episode_lengths[i] = play_one_episode(env, params)
    #각 에피소드 1,2,3... 에 에피소드 1에 몇번에 도전끝에 성공이 되었는지 적혀저서 나온다.

  avg_length = episode_lengths.mean()
  print("avg length:", avg_length)
  return avg_length


def random_search(env):
  episode_lengths = []
  best = 0
  params = None
  for t in range(100):
    new_params = np.random.random(4)*2 - 1 # 4 weight
    avg_length = play_multiple_episodes(env, 100, new_params)
    episode_lengths.append(avg_length)

    if avg_length > best:
      params = new_params
      best = avg_length
  return episode_lengths, params


if __name__ == '__main__':
  env = gym.make('CartPole-v0')
  episode_lengths, params = random_search(env)
  plt.plot(episode_lengths)
  plt.show()

  # play a final set of episodes
  env = wrappers.Monitor(env, 'my_awesome_dir')
  print("***Final run with final weights***:", play_one_episode(env, params))

```


Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
