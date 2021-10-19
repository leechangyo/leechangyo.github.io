---
layout: post
title: Factor Grpah
category: Optimization method
tag: Optimization method
---

https://gisbi-kim.github.io/blog/2021/03/04/slambackend-1.html

http://www.cs.cmu.edu/~kaess/pub/Dellaert17fnt.pdf

https://opensam.org/opensam.org/2020/05/26/factor-graphs.html

http://www.cv-learn.com/20210405-visloc-my-thoughts/

https://gtsam.org/tutorials/intro.html


Factor graphs are a family of probabilistic graphical models, other examples of which are Bayesian networks and Markov random fields, which are well known from the statistical modeling and machine learning literature

In SLAM we want to characterize our knowledge about the un-knowns X, in this case robot poses and the unknown landmark posi-tions, when given a set of observed measurements Z. Using the language of Bayesian probability, this is simply the conditional density

p(X|Z), (1.2)

and obtaining a description like this is called probabilistic inference


Generative Modeling :models that predict the next word in a sequence are typically generative models

a Bayesian network [163] or Bayes net is a directed graphical model where the nodes represent variables θj.

Per the general definition of Bayes nets, the joint density p(X,Z) = p(x1,x2,x3,l1,l2,z1,z2,z3,z4) is obtained as a
product of conditional densities:


Note that the graph structure makes an explicit statement about data association, i.e., for every measurement zk we know which landmark it is a measurement of. While it is possible to model unknown data association in a graphical model context, in this text we assume that data association is given to us as the result of a pre-processing step.

This could be derived from odometry measurements, in which case we would proceed exactly as described above.
Alternatively, such a motion model could arise from known control inputs ut. In practice, we often use a conditional Gaussian assumption,
p(xt+1|xt,ut) = 1√|2πQ|exp{−12 ‖g(xt,ut) −xt+1‖2Q}


factorization(因素分解）

For example, steps 1 and 2 above can be switched without consequence. Also, we can generate the pose measurement z1 at any time after x1 is generated, etc


n robotics we are typically interested in the unknown state variables X, such as poses and/or landmarks, given the measurements Z. The most often used estimator for these unknown state variables X is the maximum a posteriori or MAP estimate, so named because it maximizes the posterior density p(X|Z) of the states X given the measurements Z


e see that the odometry factors form a prominent, chain-like backbone, whereas off to the sides binary likelihood factors are connected to the 20 or so landmarks

http://computing.or.kr/14693/joint-probability-distribution%EA%B2%B0%ED%95%A9-%ED%99%95%EB%A5%A0-%EB%B6%84%ED%8F%AC/

https://www.youtube.com/playlist?list=PLOJ3GF0x2_eWtGXfZ5Ne1Jul5L-6Q76Sz


joint probabilty는 동시에 일어날 확률을 말합니다.

간단히 이해로는 Node(로봇 포즈, 카메라나 라이더 센서로 얻어진 랜드마크들 저장)와 Node끼리 연결되어있는 Factor, 이 케이스 같은 경우 Odometery, Node와 landmark 사이에 있는 놈들은 measurement들을 잘 가지고 노는 것이다. 다만, Probabiltic을 사용함으로써 breif propogation이 기본 틀이기 떄문에, 이전 상태를 가지고 현재 pose를 확률적으로 추정을 하거나, measurement값들을 추정을하여서 만들어진 기법으로 알고 있으나, 확실한 공부가 더 필요하다.
