---
layout: post
title: 1.Introduction to Optimization
category: Optimization method
tag: Optimization method
---

# 1. Introduction

## what is optimization?

- An important tool in daily life, business and engineer
  - Running a business: to maximize profit, minimize loss, maximize efficiency, or minimize risk.
  - Design: minimize the weight of a bridge, and maximize the strength, within the design constrains; airplane engineering such as shape design and material selection, packing transistors in a computer chip in a functional way.
  - Planning: select a flight route to minimize time or fuel consumption of an airplane
  - Supermarket pricing and logistic
  - Big data/AI
- **Formal definition**: to minimize (or maximize) a real function by deciding the values of free variables from within an allowed set.

## Classification of optimization problems

- Continuous vs Discrete
- Constrained vs Unconstrained
- Global vs Local
- Stochastic vs Deterministic
- **Convex vs Nonconvex**
  - Stochastic : random variable(DSPG)
- Convex has a lot of problem come in
- Convex form in range
  - λ ∈ {0,1} (0~1)

## Mathematical optimization
<a href="https://postimg.cc/06JgJkBk"><img src="https://i.postimg.cc/1zvy240q/Capture.png" width="700px" title="source: imgur.com" /><a>
- **optimal solution $x^*$** has smallest value of $f_0$ among all vectors that satisfy the constraints

## Examples
- portfolio optimization
  - variables: amounts invested in different assets
  - constraints: budget, max./min. investment per asset, minimum return
    - max/min : minimum could be zero, so we're not allowed to actually purchase
    - minimum return: we **expect** from this collection from this portfolio
  - objective: overall risk or return variance
    - ex) 500 stocks in can invest in, in my mean return absolutely muse excess 11% period
- device sizing in electronic circuits
  - variables: device widths and lengths
  - constraints: manufacturing limits, timing requirements, maximum area
  - objective: power consumption
- data fitting
  - variables: model parameters
  - constraints: prior information, parameter limits
  - objective: measure of misfit or prediction error

## Examples
<a href="https://postimg.cc/PL2w92FL"><img src="https://i.postimg.cc/ZRhx9Q8L/Capture.png" width="700px" title="source: imgur.com" /><a>

## Examples
- Find *two nonnegative numbers* whose sum is up to 6 so **that their product is a maximum.**
- It is equivalent to find the largest area of a rectangular region provided that its perimeter is no greater than 12.
- Problem: let the two numbers be x, y, solve
<a href="https://postimg.cc/K47BXPpD"><img src="https://i.postimg.cc/xjhRzgKh/Capture.png" width="700px" title="source: imgur.com" /><a>

## Examples
- Find a line that "best" fit three given points (x1, y1) = (2, 1), (x2, y2) = (3, 6) and (x3, y3) = (5, 4).
- Problem: let the **line equation be y = ax + b**, in the following least square sense:

<a href="https://postimg.cc/wy37PNPg"><img src="https://i.postimg.cc/9MYZDtYR/Capture.png" width="700px" title="source: imgur.com" /><a>

## Traveling sales man problem (TSMP)

<a href="https://postimg.cc/3kxwcpzN"><img src="https://i.postimg.cc/T3rWhJ9V/Capture.png" width="700px" title="source: imgur.com" /><a>

- A salesman needs to visit a number (n) of cities, denoted by city 1, 2, · · · , n.
- The distances between cities are known, i.e. d_ij denotes the distance between city i and city j.
- The salesman wants to travel each city once in turn and in the meantime minimize the total travel distance.
- What order should be used?
- Problem: denote the order of travelling by ($i_1$, · · · , $i_n$), model
- this is polynomial time

<a href="https://postimg.cc/Y4yWp7J6"><img src="https://i.postimg.cc/50fqSNgR/Capture.png" width="700px" title="source: imgur.com" /><a>

## Solving optimization problems
- general optimization problem (find a point: global optimization)
  - very difficult to solve
  - methods involve some compromise, e.g., very long computation time, or not always finding the solution
- exceptions : certain problem classes can be solved efficiently and reliably **(Global solution)**
  - least-squares problems (we get the answer in period)
  - linear programming problems (we get the answer in period)
  - convex optimization problems **(Global solution)**
- most problem can not be optimized, but we use convex optimization solve the problem
  - ex) singular values, principal component analysis

## Convex optimization problem
<a href="https://postimg.cc/r095tpF7"><img src="https://i.postimg.cc/FRMp8dtF/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/wyZQhQtZ"><img src="https://i.postimg.cc/J04YCTRr/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/4KZ4BYH3"><img src="https://i.postimg.cc/HL8JW5pb/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/4Hhnjrf4"><img src="https://i.postimg.cc/SRg8YhH9/Capture.png" width="700px" title="source: imgur.com" /><a>
- objective and constraint functions are convex:
<a href="https://postimg.cc/pmnQH6dS"><img src="https://i.postimg.cc/FHTTjtXh/Capture.png" width="700px" title="source: imgur.com" /><a>
- includes least-squares problems and linear programs as special cases
- linear function are right on the boundary they have zero curvature(弯曲)
- so one way to say convex is just positive curvature(can solve problem)
<a href="https://postimg.cc/MM22ZNxr"><img src="https://i.postimg.cc/DZm263NF/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/q6CTxK0g"><img src="https://i.postimg.cc/5ys215R5/convex-upward-function.png" width="700px" title="source: imgur.com" /><a>

## Convex optimization problem
<a href="https://postimg.cc/LhHRqJr9"><img src="https://i.postimg.cc/zDhfcW7g/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/ygCfJSx7"><img src="https://i.postimg.cc/FR7wByQk/Capture.png" width="700px" title="source: imgur.com" /><a>
- linear square problems
<a href="https://postimg.cc/zyXYB6Mx"><img src="https://i.postimg.cc/x1LfsSPV/Capture.png" width="700px" title="source: imgur.com" /><a>

## Convex optimization problem
- solving convex optimization problems
  - no analytical solution
  - reliable and efficient algorithms
  - computation time (roughly) proportional to max{n3, n2m, F}, where F is cost of evaluating $f_i$’s and their first and second derivatives(this is basically square)
  - almost a technology
- using convex optimization
  - often difficult to recognize
  - surprisingly many problems can be solved via convex optimization
<a href="https://postimg.cc/ZBryRWvt"><img src="https://i.postimg.cc/B6z5mDq6/Capture.png" width="700px" title="source: imgur.com" /><a>

## Convex optimization example
- I have surface here with patches and I have some lamps here and we are going to choose the illumination powers in the lamps. that's our variable. we'll say the illumination on a patch here is going to be a sum of illumination from the lamps and it is going to be following proportional to the lamp power. and proportionality constant can be an inverse square law. R is the distance between patch and lamp, and there will be something which is a shading effect because obviously if we are coming in straight on. we get full power if this lamp for examples, put little illumination on here and if this were below it's horizon if there were a lamp here, it would be not illuminate this patch at all


## Least squares problems
<a href="https://postimg.cc/R625PTMK"><img src="https://i.postimg.cc/Vvkfk7pG/Capture.png" width="700px" title="source: imgur.com" /><a>
- A is structured like sparse(稀少的)(image processing like transformation)
<a href="https://postimg.cc/R625PTMK"><img src="https://i.postimg.cc/Vvkfk7pG/Capture.png" width="700px" title="source: imgur.com" /><a>


## Variants of Least squares
<a href="https://postimg.cc/9wzPfGT8"><img src="https://i.postimg.cc/prJsqCdH/Capture.png" width="700px" title="source: imgur.com" /><a>

## Linear Programming
- huge amount of qualitative intelligent things about linear program what we can not do is we can not write down formula for solution
<a href="https://postimg.cc/XB8vthXt"><img src="https://i.postimg.cc/xqrbNVQj/Capture.png" width="700px" title="source: imgur.com" /><a>
- solving linear programs
  - no analytical formula for solution ( there is no formula, there is no a transpose A inverse a transpose B)
  - reliable and efficient algorithms and software
  - computation time proportional to $n^2$m if m >= n; less with structure (same as least square)
    - m : number of contain
  - a mature technology
  - a few standard tricks used to convert problems into linear programs
    - e.g., problems involving $l_1$- or $l_∞$- norms, piecewise(分段)-linear functions

## Graphic optimization in 2D
<a href="https://postimg.cc/kBWxjXnp"><img src="https://i.postimg.cc/L6bN8nG2/Capture.png" width="700px" title="source: imgur.com" /><a>

## Example: The transportation problem
<a href="https://postimg.cc/WqgX6GW9"><img src="https://i.postimg.cc/Bbwk3NxG/Capture.png" width="700px" title="source: imgur.com" /><a>

## Optimal transport
<a href="https://postimg.cc/dkMCRzbY"><img src="https://i.postimg.cc/mDkS44Vt/Capture.png" width="700px" title="source: imgur.com" /><a>

## Optimal transport: LP
<a href="https://postimg.cc/qNhSzK05"><img src="https://i.postimg.cc/W1XvYmyT/Capture.png" width="700px" title="source: imgur.com" /><a>

## Global vs local solution
<a href="https://postimg.cc/YvQcdFVm"><img src="https://i.postimg.cc/Njzgf7kb/Capture.png" width="700px" title="source: imgur.com" /><a>

## Global vs local solution

<a href="https://postimg.cc/9rnQP7Sh"><img src="https://i.postimg.cc/Njg2c8k5/Capture.png" width="700px" title="source: imgur.com" /><a>

## Graph

<a href="https://postimg.cc/4YpnJ3LR"><img src="https://i.postimg.cc/nhPmGjqr/Capture.png" width="700px" title="source: imgur.com" /><a>

# Reference

[ Optimization method - Standford University ](https://web.stanford.edu/class/ee364a/lectures.html)
