---
layout: post
title: Glaph SLAM Bundle Adjustment Tutorial
category: SLAM
tag: SLAM
---

로봇에 대한 Transformation Matrix는 Homogeneous Coordinate(homogeneous coordinates A coordinate system that algebraically treats all points in the projective plane (both Euclidean and ideal) equally. For example, the standard homogeneous coordinates [p1,p2,p3] of a point P in the projective plane are of the form [x,y,1] if P is a point in the Euclidean plane z=1 whose Cartesian coordinates are (x,y,1), or are of the form [a,b,0] if P is the ideal point – the point at infinity – associated to all lines in the Euclidean plane z=1 with direction numbers a,b,0. Homogeneous coordinates are so called because they treat Euclidean and ideal points in the same way.)이다.

node와 edge간에 관계를 이용하여서 Pose Graph 최적화를 이루어 낼때, Transformation matrix(R,T)을 가지고 계산하기에는 어렵다. 그 이유는 Rotation（+，- 로 계산 어려움, 곱셈으로 함）에 대한 Constraint이 있기 때문에 Gradient Decent를 가지고 global Minimal을 찾는게 쉽지 않다. 이에  Lie Aligebra(Targent space(로테이션에 대한 선형화), log)로 변환을 하여서 Rotation Matrix들을 Vector화를하면 “+,-“에 Non Constraint가 되기 때문에, 최적화를 쉽게 할 수 있다. 이에 최적화 작업이후에 대한 결과값들을 다시  Lie Group(Manifold space, exp)로 변환을 하여서 노드들에 대한 Transformation Matrix를 반영(업데이트))한다.



## Reference

[Graph SLAM CODE](https://edward0im.github.io/engineering/2020/09/08/pose-graph-optimization/)

[Graph SLAM Bundle Adjustment 방법 및 코드 설명](https://kusemanohar.wordpress.com/2017/04/29/howto-pose-graph-bundle-adjustment/)

[Graph SLAM 설명 2](http://jinyongjeong.github.io/2017/02/26/lec13_Least_square_SLAM/)

[Graph SLAM CODE2](https://github.com/UditSinghParihar/g2o_tutorial)
