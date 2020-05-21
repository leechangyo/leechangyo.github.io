---
layout: post
title: 6. Build a Intelligence
category: Reinforcement Learning
tag: Reinforcement Learning
---

# 1. Native(本地的) Solution to Tic-Tac-Toe
<a href="https://postimg.cc/nsyfqDpQ"><img src="https://i.postimg.cc/9MMX5dk1/512124123124.png" width="500px" title="source: imgur.com" /></a>
- Small set of rules to ensure we never lose
- we enumerate(列举) the rule of Tic-Tac-Toe. but we won't. still consider what they might be( Ex, if board is empty and it's our turn, place our piece in the center or corner)(Ex2, if opponent has 2 piece in a row, block the 3rd postion so they don't win)(Ex3, if we have 2 piece in a row, add a 3rd so we can win the game)
- Code would like this
```python
if(...):
  {...}
else if (...):
  {...}
else if (...):
  {...}
```
- Goes against the whole idea of ML
- We want one algorithm that can generalize(概括) to many problem(E.g neural network can classify pictures of animals, can also classify music)
- An agent made up of it statements will never be able to do anything other than just play tic-tac-toe(Agent only doing tic-tac-toe)
- Important: we have a model for how the game works. we know everything about tic-tac-toe
- We Know what state yields the highest reward(any state where we have 3 pieces in a row)
- i.e. by doing some action, we know the next state is given the current state(predict)
- Seems Trivial(不重要的), but not always the case

## Model
<a href="https://postimg.cc/qh2qK0V4"><img src="https://i.postimg.cc/xC4zWT6J/512312412312431.png" width="700px" title="source: imgur.com" /></a>
- the idea of having a model of the environment will come into play later.
- some algorithms we'll learn will require us to have a model, like tic-tac-toe
- other algorithms won't require us to know anything, we'll just explore the world and consume information as we go along

# 2. Components of a RL systems
<a href="https://postimg.cc/rd9dKMSW"><img src="https://i.postimg.cc/zfsT2fvd/51212413.png" width="700px" title="source: imgur.com" /></a>

## State
- Note that State involves only what the agent can sense, not everything about the environment(Ex. Vacuum robot in Australia won't be affected by something happening in india)

## Actions and Rewards
- Actions：
  - Things Agent can do that will affect its state. in tic-tac-toe, that's placing a piece on the board(like Player)
  - Preforming an action always brings us to the next state, which also comes with a possible reward.

- Rewards：
  - Rewards tell us "how good" our actions was, not whether it was a correct/incorrect action
  - doesn't tell us the best/Worst action
  - it's just a number
  - rewards we've gotten over the course of our existence doesn't necessarily represent possible rewards we could get in the future（E.g Search a bad part of state space, hit local max of 10 pts, but global max is 1000pts)
  - Agent Doesn't know that.
  - Much like life as an animal/Human Being
  - Rewards are only meaningful relative to each other

## Funny Notation
- S(t),A(t) -> R(t+1),A(t+1)
- sometimes represented as the 4-tuples: (s,a,r,s')
- oddly, the "prime" symbol doesn't strictly mean "at the t+1"
- Instead:
  - s'= state we go to when doing "a" from state "s"
  - r = reward get when we do "a" while in state "s"
{% include advertisements_horizon.html %}
## Episode
- Episode represents one run of the game(Ex. start tic-tac-toe with empty board)
- As soon as one player gets 3 pieces in a row, that's the end of the episode
- our RL agent will learn across many episodes(Ex. after playing 1000,10000 or 100000 episodes, we can possibly have trained an intelligent agent)
- . # episodes we use to train is a hyperparameter
- playing the game tic-tac-toe is an **episode task** because we play it again and again
- Different from a **continuous task** which never ends
- when is the end of an episode?
- Certain states in the state space tell us when the episode is over.
- There are states from which more action can be taken
- They are called **Terminal States**
- For Tic-Tac_toe:
  - one player gets 3 in a row
  - Board is full(draw)

# 3. Other Games(Cart-pole/inverted pendulum/Groundhog Day)
<a href="https://postimg.cc/V0X2s4hk"><img src="https://i.postimg.cc/7hR4VWM0/215321342315234.png" width="500px" title="source: imgur.com" /></a>
- Control systems: Inverted pendulum
- RL : Cart-Pole
- Unstable system
- Episode starts which pole vertical, soon falls
- Agent: Move to Keep the pole within certain angle.
- Terminal : any angle past the point of no return
- Angle requires continuous state space
- infinite number of states
<a href="https://postimg.cc/ftVfvLj4"><img src="https://i.postimg.cc/qRLD46N7/5123214123.png" width="500px" title="source: imgur.com" /></a>
- being able to learn from multiple episode is like the move Groundhog day
- Main Character wakes up and lives through the same day, everyday
- Allows him to get better and better at making the most out of the day
- RL agent may suck at first, but will gradually learn with each passing episode. each episode is a fresh start

# 4. Summary
{% include advertisements_horizon.html %}
- what do we learn so far?
- Agent
- environment
- State, action, reward
- Episodes
- Terminal States

Reference:

[Artificial Intelligence Reinforcement Learning](https://www.udemy.com/course/artificial-intelligence-reinforcement-learning-in-python/)

[Advance AI : Deep-Reinforcement Learning](https://www.udemy.com/deep-reinforcement-learning-in-python/)

[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
