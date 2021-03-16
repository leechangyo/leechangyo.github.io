---
layout: post
title: Recursive Bayes Filter
category: calculus
tag: calculus
---

- Kalman Filter와 Particle Filter의 프레임 워크이다
- 모션모델과 측정모델을 통해 상태를 추정한다
- SLAM KR에 자세하게 나와있으며, Markov Assumption을 통해(쉽게 관계식들을 정리할 수 있음) 상태에 대한 Probabilistic(likelihood(어느 값이 가우시안 분포도 퍼센테이지 안에 드는 값이 아닌, 어느 값이 가우시안 분포도에서 가장 높은 값)로 predict state)을 구한다



<a href="https://postimg.cc/56sjdzSX"><img src="https://i.postimg.cc/qvTKKXtx/Screen-Shot-2021-03-15-at-11-53-23-AM.png" width="700px" title="source: imgur.com" /><a>


https://en.wikipedia.org/wiki/Recursive_Bayesian_estimation
