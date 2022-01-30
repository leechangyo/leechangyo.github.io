---
layout: post
title: Graph SLAM
category: SLAM
tag: SLAM
---

 Graph SLAM은 Pose graph optimization의 방식으로 odometery(휠엔코더)로 생성된 로봇 포즈에 대한 값을 Node로 저장을 한다.

 주행중 생긴 여러 노드들간의 Rotation이나 Traslation의 상대적인 값(odometery measurement)들은 edge로 여겨지고 이는 관찰값이 된다.

 그리고 만약 현재 주행하고 있는 포즈에 대한 추정 값이 이전에 방문한 노드에 존재한다면, 루프크로져를 통해 그래프 최적화가 일어나게 되는데

 이때, 이전에 생성된 edge들에 대한 상대적 Odometer값들은 예측값으로 변화하게 되고, 카메라 센서다 라이다 센서를 통해 얻어진 Visual odometery는 관측값으로 사용하게

 되면서  Nonlinear least square method 방식을 통해 관측값과 예측값을 iterate하여서 에러값을 미니멀하게 만들어 글로벌 맵을 최적화 한다.

에러 펑션 Minimize하는 방식은

e(x-x^) M e(x-x^)^t

에러 펑션과 인포메이션 매트릭스(corvariance matrix)를 통해 최적화를 한다.

방식에는 Gaussian Newton과 Levenberg-Marquardt(LM)
