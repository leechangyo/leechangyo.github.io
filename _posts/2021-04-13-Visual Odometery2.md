---
layout: post
title: Visual Odometery & Pose Estimation(using Direct Method(intensity))
category: Visual SLAM
tag: Visual SLAM
---

1. feature extract feature point 방법의 단점은 키포인트들과 Descriptor(매칭점을 찾는데 사용됨) 계산이 많이 소모 된다

2. feature extract feature point를 사용시 특정점 이외에 나머지 이미지 정보를 무시한다.

3. blur 크거나 특정점이 없는 공간에서의 키포인트들과 디스크립터를 구하기 쉽지 않아 매칭포인트를 구하고 Visual Odometery를 구하기 쉽지 않다.

위와 같은 문제를 Optical Flow을 이용하여서 키포인트만 계산, 남기고 디스크립터와 매칭포인트는 계산하지 않는다.

혹은 Direct Method 방법으로 픽셀 intensity를 통해 Optical Flow로 추출된 키포인트만 이용하여 디스크립터 없이 바로 카메라 포즈를 계산하다.(Direct Method는 보다 간단하다)

즉, Discriptor를 이용한 매칭포인트는, Optical FLow(Lucas-Kandade Optical Flow방법)로 대체를 하고 Epipolar Contraint과 PnP, ICP방법을 통해 카메라 포즈를 추정한다.

기존에 feature extract 알고리즘을 통해 키포인트와 디스크립터를 찾고, 이전이미지와 현재 이미지의 매칭점을 찾아 Epipolar Contraint을 통해 Fundamental Matrix, 그리고 카메라 파라미터를 고려하여 Essential Matrix 값을 구하고, 동시에 triangulation 을 통하여 3D Mappoints 찾은 다음, 찾은 3D Mappoints를 다시 현재 이미지 플래인에 Reprojection을 하여서 현재 관찰된 3D Mappoints에 관한 매칭점과 Re projection된 2d 포인트를 Least Square를 통해 Visual Odometery 값을 최적화하여 구하였다.

그러나 Direct Method는 이런 절차와 다르게 Photometric Error를 이용하여 최적화를 한다.

### Direct Method의 약점은

- 빛에 약하다. 일정하지 못한 환경에서 취약하다.

- 비슷한 픽셀을 너무 많아 계산할때 복잡하다.

- 로테이션에 매우 취약하다.
