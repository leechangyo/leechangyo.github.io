---
layout: post
title: 6. path planning
category: ROBOTICS
tag: ROBOTICS
---

# path planning
- Geometrical description of the robot's movement in space
- smooth transition between two or more target points
- A unique geometrical path can be described by different time dependent trajectories
- Nomenclature(命名法)
  - PTP
  - Line
  - Circle
  - Splines

## 1. Point to Point(PTP)
- Linear interpolation of joint axes
<a href="https://postimg.cc/7bmYk5SY"><img src="https://i.postimg.cc/m2Gc7MDM/Capture.png" width="700px" title="source: imgur.com" /><a>
- t is just parameter describing the curve from zero to one only geometrically
- no direct control over TCP position and orientation
- they have no control over the actual path of the end effector of the robot
- cyclic call of the kinematic
<a href="https://postimg.cc/4m6PyTNd"><img src="https://i.postimg.cc/L67byHcz/Capture.png" width="700px" title="source: imgur.com" /><a>
- time-optimal movement(move as fast as joint axes can)
- only movement that can modify joint configuration

## 2. path-interpolated movements

<a href="https://postimg.cc/KRjLSSkw"><img src="https://i.postimg.cc/FsgxSh9r/Capture.png" width="700px" title="source: imgur.com" /><a>

- orientation cannot be interpolated linearly when using Euler angle

<a href="https://postimg.cc/s1hdn1KN"><img src="https://i.postimg.cc/DfCvwXGn/Capture.png" width="700px" title="source: imgur.com" /><a>

## 3. Quaternions
- convenient way to express an orientation
<a href="https://postimg.cc/D4ZcMW3Y"><img src="https://i.postimg.cc/Rh1bqfs9/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/nXkCC3DB"><img src="https://i.postimg.cc/JhwJYg76/Capture.png" width="700px" title="source: imgur.com" /><a>
- commutative means that (교환법칙이 성립할 때를 의미함)
- Vs **Euler angles**
  - unique representation of rotation
  - can be easily interpolated
- Vs **Rotation Matrix**
  - more compact representation
  - easy to interpolate
  - do not suffer from numerical approximation
- slerp(Spherical Linear interpolation:구면보간법)
  - Optimal(shortest) path between $q_1$ and $q_2$

<a href="https://postimg.cc/dhjcFXyP"><img src="https://i.postimg.cc/kgkJQPM4/Capture.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/7C47GChJ"><img src="https://i.postimg.cc/1znKdwZB/Capture.png" width="700px" title="source: imgur.com" /><a>

## 4. Line and Circle
- Interpolate position linearly

<a href="https://postimg.cc/gwL5JTw2"><img src="https://i.postimg.cc/VNKc7ymn/Capture.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/5Y1ZRm09"><img src="https://i.postimg.cc/Hx7djZ1M/Capture.png" width="700px" title="source: imgur.com" /><a>

- Circle detail
  - point must be non collinear(동일선상)
  - N = $v_1$ x $v_2$
- direction N stays the same, only magnitude changed
- normalize N after the product

<a href="https://postimg.cc/hhJFvsYw"><img src="https://i.postimg.cc/tRk9rmNX/Capture.png" width="700px" title="source: imgur.com" /><a>

## 5. spline(매끄러운 곡선)
- The operator needs a "smooth" path between points

<a href="https://postimg.cc/6T73Kdf6"><img src="https://i.postimg.cc/pVCnDC9j/Capture.png" width="700px" title="source: imgur.com" /><a>

## 5.1 Practice
- place control point along targets

<a href="https://postimg.cc/p5v1sQ2Q"><img src="https://i.postimg.cc/qq6VMQSF/Capture.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/phWs8Wkk"><img src="https://i.postimg.cc/JnJvftXw/Capture.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/jwSh28dF"><img src="https://i.postimg.cc/tThvrw0p/Capture.png" width="700px" title="source: imgur.com" /><a>

## 6 transition(변화)
- round edge
- sharp edges are a problem
  - smooth geometrical path
  - increase path speed
  - reduces mechanical wear

<a href="https://postimg.cc/PCLWgLVB"><img src="https://i.postimg.cc/2SG0BhQ8/Capture.png" width="700px" title="source: imgur.com" /><a>


<a href="https://postimg.cc/PCL7S5Wq"><img src="https://i.postimg.cc/Vk4cLS5X/Capture.png" width="700px" title="source: imgur.com" /><a>

## 7 path length and corrections
- analytical calculation for lines and circles

<a href="https://postimg.cc/Z0dCz8JP"><img src="https://i.postimg.cc/59PvvgGr/Capture.png" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/bGw9mCc9"><img src="https://i.postimg.cc/nLvScg3g/Capture.png" width="700px" title="source: imgur.com" /><a>

- numerically calculation for PTP and splines

<a href="https://postimg.cc/G9CJNmCp"><img src="https://i.postimg.cc/rydnDDZ5/Capture.png" width="700px" title="source: imgur.com" /><a>

- external path corrections

<a href="https://postimg.cc/k26bG0mZ"><img src="https://i.postimg.cc/RZGQsmbM/Capture.png" width="700px" title="source: imgur.com" /><a>

- happen at runtime, not planned, not predictable
- typical case : conveyor tracking(increased productivity)
- position limits must be monitored at runtime
- path length changes, so dynamic limits must also be monitored at runtime
