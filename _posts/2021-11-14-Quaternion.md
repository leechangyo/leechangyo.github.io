---
layout: post
title: 각 벡터 Quaternion 각도 구하는 법, quaternion times its conjugate transpose meaning?
category: Machine Vision
tag: Machine Vision
---

### 각 벡터 Quaternion 각도 구하는 법

https://stackoverflow.com/questions/1171849/finding-quaternion-representing-the-rotation-from-one-vector-to-another

https://math.stackexchange.com/questions/3174308/norm-of-quaternion

https://stackoverflow.com/questions/69936813/what-is-a-meaning-of-q-q-adjoint-mean\

SVD를 구하는 이유는, 두 eigen vector에 대한 multiply는 Orthogonal Matrix 이다. 즉, A,B라는 두 포인트가 있다면, 두 매트릭스간에 Rotation Matrix를 구하는 것이다.

### quaternion times its conjugate transpose meaning

포인트(Vector)의 quaternion, rotation matrix를 구할 수 있다.

quaternion times its conjugate transpose -> Eigen Decommission -> U(eigen vector) * sigma(eigen value) * V(eigen vector) -> U * V = Orthogonal matrix(Mapping Rotation)

이런 식으로 Rotation Matrix를 구할 수 있다.

참고 : 유투브에 "how to quaternions produce 3d rotation", "quaternion and 3d rotation, explained interactively"
