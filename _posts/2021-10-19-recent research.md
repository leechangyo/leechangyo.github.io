---
layout: post
title: VISLAM Research
category: SLAM
tag: SLAM
---

Lidar - Robust but cost

VO
  - Weak at Homogenous coordinate(because of scale factor, can be have scale error)
  - Weat at Rotation(because of feature-based(intensity-based drain a lot resource of cpu)

VIO
  - improved Rotation Problem
  - structure ambiguity problem(if similiar scene a lot, might have problem to loop closure
  - typical work VIO is "Okvis(stereo + IMU), Vins-mono(mono+imu), BASALT)

STATE PROBLEM
  - Loop Closure Detection is weak at similar scene
  - it occurs global localization and map

Related work
  - Cube SLAM
  - QuadricSLAM

Resent Research
  - offers Semantic VI SLAM is to solve ambiguity problem

Keyword
about brief propogation : https://eehoeskrap.tistory.com/182
Factor graph.
