---
layout: post
title: why square root and hessian matrix and error state kalman filter
category: calculus
tag: calculus
---

선형 : 닫혀있는 공간, 스판, subspace 해가 있는 시스템

lie group <-> lie algebra :
Manifold space <-> vector space
easy calculation, created jacobian matrix
constrained conditon <-> unconstrained condition
최적화에서 많이 쓰임

역행렬이 존재하지 않는다면, singularity's 해가 존재하지 않는다(블랙홀),
무수한 해를 가질 수 있다.

직교벡터와 정규직교벡터의 집합은 항상 선형독립이다.

벡터->평면 투영, span(u) 유닛 벡터를 알면 할 수 있다.

자명해(Any solution in which at least one variable has a nonzero value is called a nontrivial solution.）

why call square root
https://www.quora.com/Why-do-we-call-square-root-a-square-root


## SVD Explain Easly
https://stackoverflow.com/questions/62285218/how-to-find-the-rotation-matrix-from-svd

정방행렬 A 가 Postivie Definite인 경우 A 의 고유값은 항상 모두 양수이다.

일반적으로 기계학습에서는 대칭이고 Positive Definite인 행렬을 다룬다. 예를 들면, 인 행렬이 있고 각 열은 사람을 의미하고 각 행은 Feature를 의미한다고 가정했을 때, 는 각 사람들 간 유사도 를 의미하고 는 각 Feature들의 상관관계를 의미한다. 이 때, 는 주성분분석(Principal Component Analysis)에서 Covariance Matrix를 구할 때 사용된다.

Matrix분해를 해서 선형시스템을 푸는 문제에 분해를(QA, LL)를 사용하는 이유는, 계산을 빠르게 할 수 있고, 고유값을 이용해서 계산하기 때문에 안정적이다. 또한 singularity문제를 해결을 할 수 있다.


## Hessian
https://en.wikipedia.org/wiki/Hessian_matrix

이후 중력 벡터가 추정되고 중력에 수평한 평면이 예측되면 모든 상태와 궤적(trajectory)를 중력에 맞게 방향을 재조절할 수 있다 (Lupton and Sukkarieh, 2009).  그리고 일반적으로 사용하는 g = (0,0,-9.81 xx) 와 같이 설정하고 xx 는 독자의 실험의 정확도에 맞게 설정할 수 있다.


Differential equations deal with continuous system, while the difference equations are meant for discrete process. Generally, a difference equation is obtained in an attempt to solve an ordinary differential equation by finite difference method.


## error state kalman filter

에러를 dx를 만드는 작업과

https://www.reddit.com/r/ControlTheory/comments/lkan24/errorstate_kalman_filter_vs_extended_kalman_filter/

https://notanymike.github.io/Error-State-Extended-Kalman-Filter/

https://alida.tistory.com/63?category=1061564

error state kalman filter example code
https://matthewhampsey.github.io/blog/2020/07/18/mekf

https://taeyoung96.github.io/research/IESKF/

https://www.researchgate.net/publication/269254094_Extended_Kalman_Filter_vs_Error_State_Kalman_Filter_for_Aircraft_Attitude_Estimation

https://blog.csdn.net/weixin_39061796/article/details/120027784
