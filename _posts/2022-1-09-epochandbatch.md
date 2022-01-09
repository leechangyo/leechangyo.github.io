---
layout: post
title: epoch, batch size, iteration의 의미
category: Machine Learning
tag: Machine Learning
---

정리 잘 해놓은 블로그

https://m.blog.naver.com/qbxlvnf11/221449297033

최적화 방법은 즉 error function에 2차 편미분을 하여서(Jacobian Matrix, Hessian Matrix) Gaussian Newton(Decent) 방식으로 변환을 하여서, delta X에 대한 값(pose and landmark or just pose(landmark))를 x에 업데이트를 Iteration K만큼 해주면서 업데이트에 적용되는 delta x에 대해 최소값을 만드는 것이다.
