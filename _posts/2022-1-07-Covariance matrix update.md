---
layout: post
title: Covariance Matrix Propagation and Information Matrix Propagation relationship with Gaussian(Normal) Distribution, Optimization
category: Optimization method
tag: Optimization method
---

<a href="https://postimg.cc/3WN1p6rr"><img src="https://i.postimg.cc/ZYFQkt1N/Kakao-Talk-Image-2022-01-09-22-16-27.jpg" width="700px" title="source: imgur.com" /><a>


<a href="https://postimg.cc/ZB2d6wj0"><img src="https://i.postimg.cc/tJCNQfxd/Kakao-Talk-Image-2022-01-09-22-17-42.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/T52PsS0t"><img src="https://i.postimg.cc/cCw8vGnG/Kakao-Talk-Image-2022-01-09-22-20-28.jpg" width="700px" title="source: imgur.com" /><a>


최적화 방법은 즉 error function에 2차 편미분을 하여서(Jacobian Matrix, Hessian Matrix) Gaussian Newton(Decent) 방식으로 변환을 하여서, delta X에 대한 값(pose and landmark or just pose(landmark))를 x에 업데이트를 Iteration K만큼 해주면서 업데이트에 적용되는 delta x에 대해 최소값을 만드는 것이다.

### Reference

https://present5.com/siddharth-choudhary-bundle-adjustment-a-tutorial/

http://www.cv-learn.com/20210313-ba/
