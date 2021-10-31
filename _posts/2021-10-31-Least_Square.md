---
layout: post
title: Least Square 간단 이해
category: calculus
tag: calculus
---

super clears
https://www.youtube.com/watch?v=mY3G_hjrt7A&list=PLubUquiqNQdP_H6uUmU-9f0y_LheA3Hil&index=2

https://darkpgmr.tistory.com/56

https://hellbell.tistory.com/entry/Iteratively-Reweighted-Least-Square-IRLS

http://www.cv-learn.com/20210313-ba/

---

센서는 모두 에러를 가지고 있기 때문에 infomration matrix 필요

error * information matrix * error

Eq{1} [e1,e2,e3][][e1,e2,e3]^T <- e1,e2,e3 are sensors

skew-matrix on information matrix meant relationship between each sensors.

if we assume each sensor doesn't impact on other, then, we can define skew matrix's elements to "0"

diagonal elements are 1/sigma^2 which is simga is standard distribution of sensor.

from {1} we got, e(x) = (e1^2)/(sigma)^2 + (e1^2)/(sigma)^2 + (e1^2)/(sigma)^2 with that, we only care about biggest value.

to find armin(x)

1) Talyor expension of e
2) Linearization -> independent of delta x + linear dependent + quadratic dependency
3) iteration(delta x differentiation) -> H * delta x = -b
4) delta x = -H^-1 & b
5) x = x + delta x

this process is gaussian newton

**case
if H is m x n matrix, we can not inverse it

using trick of linear algebra

H^t * H *  dletax = H^t(-b) // pseudo inverse(when inverse matrix not possible)

delta x = H*b

**case very expensive inverse matrix method that's why

many way to matrix decomposition is

- cholesky(matrix size small)
- conjugate gradient(matrix size big)

probabilistic notation : minimizing the error squared term is the equivalent to maximising the log-likelihood that our state vector x is on the measured point in our map.

Least Square는 즉 미지 상수 abc 를 구하는 것이다
