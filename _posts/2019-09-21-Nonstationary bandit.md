---
layout: post
title: 5. Nonstationary bandit
category: AI
tag: Reinforcement Learning
---

# Nonstationary Bandits
- there equations will show up again and again
- what is stationary?
- A stationary process is one whose statistics don't change over rime(e.g mean)
- weak-sense stationary: mean(1st order statistic) and autocovariance(2nd order statistic) don't change overtime
  - if don't know covariance let's check it in [here](https://ko.wikipedia.org/wiki/%EA%B3%B5%EB%B6%84%EC%82%B0)
- strong-sense stationary: entire PDF doesn't change over time
- SSS -> WSS
- what if our bandits are not stationary? does it make sense to calculate the mean as we have been?
## Mean Update Equation
- from earlier:
<a href="https://postimg.cc/xXmKnx0z"><img src="https://i.postimg.cc/MpPt0NNd/51213.png" width="300px" title="source: imgur.com" /></a>
- some rearranging for convenience:
<a href="https://postimg.cc/Z97Ygm1F"><img src="https://i.postimg.cc/501HXNgG/55151.png" width="400px" title="source: imgur.com" /></a>
- Replace 1/t:
<a href="https://postimg.cc/hJJNDqX0"><img src="https://i.postimg.cc/W4SV7Njx/515.png" width="400px" title="source: imgur.com" /></a>
- Alpha can be anything(even constant). looks kind of like gradient descent
- One adaptive learning rate used 1/t

## Low pass filter
- should look familiar:
<a href="https://postimg.cc/068dnGsK"><img src="https://i.postimg.cc/Xv5HVx1L/51515.png" width="400px" title="source: imgur.com" /></a>
- Low-pass filter studied in ML related to RNN(i will post ML too in future)
- Get rid of recurrence(复发):
 <a href="https://postimg.cc/NLrJ54fQ"><img src="https://i.postimg.cc/XNsT1HRB/521525.png" width="400px" title="source: imgur.com" /></a>
- Q has exponentially decaying dependence on X
- More emphasis on recent value

## Convergence Criteria for Q
- Q will converge if :
 <a href="https://postimg.cc/dLfGrMWX"><img src="https://i.postimg.cc/P5XKGT1t/51511312.png" width="400px" title="source: imgur.com" /></a>
 - think back to calculus 2(두번쨰)
 - constant alpha does not converge
 - 1/t does
 - if problem is Nonstationary, we don't want Convergence(融合)(equal weighting of all samples doesn't make sense) so we will see constant alpha used
 - Does this converge?
  <a href="https://postimg.cc/WdJYJw7G"><img src="https://i.postimg.cc/VkKxp7rV/5123123.png" width="200px" title="source: imgur.com" /></a>
- No. Sum of alphas is not infinity
- Does this convergence?
  <a href="https://postimg.cc/sG71h8vF"><img src="https://i.postimg.cc/Y0yFJB7S/15121312413.png" width="200px" title="source: imgur.com" /></a>
- Nonstationary system은 모델 파라미터 Θ가 시간의 변화에 따라서 상수가 변한다. 광고를 예시를 들면, 광고에 대한 방문자의 선호도는 제품에 대한 소문, 잦은 광고, 노출에 따른 여러가지 이유로 변할 수 있기 때문이다. 이러한 환경의 MAB문제를 Non-stationary 문제라고 한다. 시간에 흐름에 따라 α+β값이 증가함으로 Θ에 대한 추정이 어려워진다.

```python
# From the course: Bayesin Machine Learning in Python: A/B Testing
# https://deeplearningcourses.com/c/bayesian-machine-learning-in-python-ab-testing
# https://www.udemy.com/bayesian-machine-learning-in-python-ab-testing
from __future__ import print_function, division
from builtins import range
# Note: you may need to update your version of future
# sudo pip install -U future


import matplotlib.pyplot as plt
import numpy as np
from bayesian_bandit import Bandit


def run_experiment(p1, p2, p3, N):
  bandits = [Bandit(p1), Bandit(p2), Bandit(p3)]

  data = np.empty(N)

  for i in range(N):
    # thompson sampling
    j = np.argmax([b.sample() for b in bandits])
    x = bandits[j].pull()
    bandits[j].update(x)

    # for the plot
    data[i] = x
  cumulative_average_ctr = np.cumsum(data) / (np.arange(N) + 1)

  # plot moving average ctr
  plt.plot(cumulative_average_ctr)
  plt.plot(np.ones(N)*p1)
  plt.plot(np.ones(N)*p2)
  plt.plot(np.ones(N)*p3)
  plt.ylim((0,1))
  plt.xscale('log')
  plt.show()


run_experiment(0.2, 0.25, 0.3, 100000)
```
