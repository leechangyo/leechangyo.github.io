---
layout: post
title: Least Square에 대한 개념
category: ROBOTICS
tag: ROBOTICS
---

최적화를 할때 많이 사용하는 방법 중 하나 이다.

특히 로봇공학에서는

estimate parameters based on observations

Map a state *x* to a predicted observation

이 정확할 것 같다.

즉 관측값과 예측값을 Least Square를 통해 여려 iterate를 통해 에러를 미니멈라이즈 한다던지, Lesat Square를 최적화 한다

<a href="https://postimg.cc/YhYn9gmr"><img src="https://i.postimg.cc/yYrwLy1Z/Screen-Shot-2021-04-08-at-8-17-05-PM.png" width="700px" title="source: imgur.com" /><a>

위에 와 같이 observation model, 모바일 로봇으로 치면, 바퀴 3개달린, 자동차처럼 바퀴 4개 달린, mechanum wheel인 특성을 고려한 상태전이매트릭스로 간주 할 수 있따.(State Transition Matrix)， 그리고 주어진 상태전이매트릭스에 Angular velocity, Linear Velocity [Twist]가 주어졌을 때 얻어지는 상태 예측 값이다.(Bayese Filter에서 Predict model과 같은 개념이다.)

<a href="https://postimg.cc/vDQf9Vj8"><img src="https://i.postimg.cc/XN58RKk5/Screen-Shot-2021-04-08-at-8-22-17-PM.png" width="700px" title="source: imgur.com" /><a>

그리고 실제 레이더나 카메라 프로세스로 얻어진 관측값을 least Square로 최적화 한다.

<a href="https://postimg.cc/nMG8R60Y"><img src="https://i.postimg.cc/BZdq63Fy/Screen-Shot-2021-04-08-at-8-26-15-PM.png" width="700px" title="source: imgur.com" /><a>

이는 Mapping이나 localization, SLAM, Calibration, BundleAdjustment... 등 많이 사용이 된다.

보통 현실에서는 Non-Linear 한 경우이므로

Least Square을 쓸 때에는 선형화(linearize)를 시켜줘야한다.
- 주로 talyor expanstion(테일러 1차 근사), Jacobian

 즉 로봇 상태를 추정하는데 있어 상태 추정에 대한 1차 테일러 근사를 시킨다면 Jacobian Matrix폼으로 상태를 추정하게 되고(+ Non linear 모델일 경우 uncerntainty matrix도 포함이 된다.)

 Non-linear 형식일 경우 uncerntainty로 완벽한 상태를 추정할 수 없기 때문에 Iterate형식으로 보통 문제를 풀며 이때 Robust kernel 같은 learning rate(?)를 주어주어서 최적화 업데이트를 한다.

 그렇기 때문에 initial guess가 중요하다.
