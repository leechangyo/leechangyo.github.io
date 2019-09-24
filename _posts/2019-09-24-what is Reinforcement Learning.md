---
layout: post
title: 1. Reinforcement Learning Introduction
category: AI
tag: Reinforcement Learning
---

# What is Reinforcement Learning?

*There are 3 big part in AI*

<a href="https://postimg.cc/zLYZjrbb"><img src="https://i.postimg.cc/jjLxRqRX/41232143.png" width="700px" title="source: imgur.com" /></a>
- **Supervised Learning** (i.e. Spam Detection, Image Classification)
<a href="https://postimg.cc/sQyMbq81"><img src="https://i.postimg.cc/bJS1Sj90/4123124123.png" width="700px" title="source: imgur.com" /></a>
- **Unsupervised Learning**(i.e Topic Modeling Web Pages, Clustering Genetic Sequences)
<a href="https://postimg.cc/VSPvb2z4"><img src="https://i.postimg.cc/MGvQg8Nk/413124123.png" width="700px" title="source: imgur.com" /></a>
- **Reinforcement Learning**(i.e Tic-Tac-Toe,Go,Chess, Walking, Super mario, Doom, Starcraft)


## 1. Supervised/Unsupervised Interfaces
- as we know about ML Theory and Function
```python
class SupervisedModel:
  def fit(X,y):
  def predict(x):
```
```python
class UnsupervisedModel:
  def fit(x):
  def transform(x): #(e.g cluster assignment)
```
- common theme is "training data"
- input data: X(N x D Matrix)
- Tragets: Y (N x 1 vector)
- "all data is the same"
- Format doesn't change whether you're in biology, finance, economics, etc.
- fits neatly into one library: scikit-Learn
- "simplistic", but still useful:
  - Face Detection
  - Speech Recognition
{% include advertisements_horizon.html %}
## 2. Reinforcement Learning
<a href="https://postimg.cc/0r96nzLv"><img src="https://i.postimg.cc/7P2gx795/12312413.png" width="700px" title="source: imgur.com" /></a>

- Not just a static table of data
- An agent interacts with the world(environment)
- Can be simulated or real(E.g Vacuum robot)
- Data comes from sensors
  - Cameras, Microphones, GPS, accelerometer
- Continuous stream of data
  - Consider past and future
- RL agent is "thing" with a lifetime
  - At each step, decide what to do
- An (un)supervised model is just a static object -> input -> output

## 3. isn't it still supervised learning?
- X can represent the state i'm in, Y represent the Target (ideal action to perform in that state)
  - state = sensor recording from self-driving car
  - state = Video Game ScreenShot
  - state = Chess Board Positions
- Yes it is, but for example, consider GO: $ N = 8 x 10^100 $
- ImageNet, the image Classification benchmark, has $ N=10^6 $ images
  - Go is 94 orders of magnitude larger
  - Takes ~1 day with good hardware
- 1 order of magnitude Larger -> 10 days
- 2 order of magnitude Larger -> 100 days

## 4. Rewards
<a href="https://postimg.cc/8JD4k1sS"><img src="https://i.postimg.cc/W3qHnzvh/123231.png" width="700px" title="source: imgur.com" /></a>

- sometimes you'll see reference to psychology; RL has been used to model animal behavior
- RL agent's goal is in the future
  - in contrast, a supervised model simply tried to get good accuracy / minimized cost on current input
- Feedback Signals(Rewards) come from the environment (i.e the agent *experiences* them)

## 5. Rewards vs Targets
<a href="https://postimg.cc/1f2p5pzS"><img src="https://i.postimg.cc/x1Tg55DX/14141314123.png" width="700px" title="source: imgur.com" /></a>
- you might think of supervised Targets/labels as something like rewards. but these handmade labels are coded by humans - they do not come from environment
- Supervised inputs/targets are just database tables
- Supervised models **instantly** know if it is wrong/right, because inputs + targets are provided simultaneously
- RL is dynamic - if an agent solves a maze, it only know its decisions were correct if it eventually solves the maze

## 6. On Unusual or Unexpected Strategies of RL
<a href="https://postimg.cc/F1cfg2rM"><img src="https://i.postimg.cc/59Kw1VZ2/125212425.png" width="700px" title="source: imgur.com" /></a>
- Goal of AlphaGO is to win Go, and the goal of a video game agent is high score/live as long as possible
- what is the goal of an animal/human?
- Evolutionary psychologists believe in the "selfish Gene" Theory
  - *Rechard Dawkins - The Selfish Gene*
- Genes simply want to make more of themselves
- We humans(conscious living beings) are totally unaware of this
- we can't ask our genes how they feel
- we are simply a vessel for our genes' proliferation(급증)
- is consciousness just an illusion?
- Disconnect between what we think we want vs "true goal"
{% include advertisements_horizon.html %}
- Like Alphago, we've found roundabout and unlikely ways of achieving our Goal
- The action taken doesn't necessarily have to have an obvious / explicit relationship to the Goal
- we might desire riches/money -but why? Maybe natural selection or leads to better health and social status. there are no laws physics which govern riches and gene replication
- it's a novel solution to the problem
- AI can also find such strange or unusual ways to achieve a goal
<a href="https://postimg.cc/xkw6hWWw"><img src="https://i.postimg.cc/500DYJp2/2152314215234.png" width="700px" title="source: imgur.com" /></a>
- we can replace "getting rich" with any trait(특성) we want
  - being healthy and strong
  - Having strong analytical skills
- That's a sociologist(사회학자)'s job
- Our interest lies in the fact that there are multiple novel strangies of achieving the same goal(gene replication)
- What is considered a good strategy can fluctuate
- Ex. Sugar:
  - our brain runs on sugar, it gives us energy
  - today, it causes disease and death
- Thus, a Strategy that seems good right now may not be globally optimal

## 7. Speed of Learning and Adaption
- Animals gain new traits via evolution/mutation/natural selection
  - this is slow
  - each newborn, even given an advantageous trait, still must learn from scratch
- AI can train via simulation
  - it can spawn new offspring instantly
  - obtain hundreds / thousands of years of experience in the blink of an eye

Reference
[Cutting-Edge Deep-Reinforcement Learning](https://www.udemy.com/cutting-edge-artificial-intelligence/learn/lecture/14650508#overview)
