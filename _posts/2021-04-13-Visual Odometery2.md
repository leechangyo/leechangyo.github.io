---
layout: post
title: Visual Odometery & Pose Estimation(using Direct Method(intensity))
category: Visual SLAM
tag: Visual SLAM
---

1. feature extract feature point 방법의 단점은 키포인트들과 Descriptor(매칭점을 찾는데 사용됨) 계산이 많이 소모 된다

2. feature extract feature point를 사용시 특정점 이외에 나머지 이미지 정보를 무시한다.

3. blur 크거나 특정점이 없는 공간에서의 키포인트들과 디스크립터를 구하기 쉽지 않아 매칭포인트를 구하고 Visual Odometery를 구하기 쉽지 않다.

위와 같은 문제를 Optical Flow을 이용하여서 image pixel intensity를 통해 키포인트를 추출하고 FLANN같은 방법으로 이미지 매칭 포인트를 찾는 다음 카메라 포즈를 찾기 위해 매칭 포인트를 이용한 epipolar Contraint를 통해 8point algorithm을 통한  Fundamental Matrix를 구한다.

이때, 질좋은 8 매칭포인트를 구하기 위해서 RANSAC을 사용하여서 Fundamental Matrix를 진행한다.

이후 Camera 내부 파라미터를 통해 Essential Matrix를 만들고, 행렬 분해를 통해 Relative Pose를 구한다.

구해진 Relative Pose와 매칭포인트를 통해 3d Triangulation으로 맵포인트들을 만든다.

이후 다음 이미지에 이미지 추출과 매칭을 통해 매칭된 2d와 3d mappoints들은 solve pnp를 통하여 다음 카메라 포즈를 추정을 하고, 다음 2d끼리 매칭된 포인트에 대해서 3d triangulation을 한다.

만약 스테레오나 뎁스 카메라일 경우 위에 절차를 생략을 하고 바로 매칭 포인트에 대해서 3d mappoints을 만들고 solve pnp를 통해 카메라 포즈와 3d mappoints들을 만든다.

즉, Discriptor를 이용한 매칭포인트는, Optical FLow(Lucas-Kandade Optical Flow방법)로 대체를 하고 Epipolar Contraint(모노일경우)과 PnP, ICP방법을 통해 카메라 포즈를 추정한다.

기존에 feature extract 알고리즘을 통해 키포인트와 디스크립터를 찾고, 이전이미지와 현재 이미지의 매칭점을 찾고, 좋은 매칭점만으로 Fundamental Matrix를 구하고 싶으니, Ransac을 통해서 매칭이 좋은 8points를 추출하여 Epipolar Contraint을 통해 Fundamental Matrix, 그리고 카메라 파라미터를 고려하여 Essential Matrix 값을 구하고, 동시에 triangulation 을 통하여 3D Mappoints 찾은 다음, 찾은 3D Mappoints를 다시 현재 이미지 플래인에 Reprojection을 하여서 현재 관찰된 3D Mappoints에 관한 매칭점과 Re projection된 2d 포인트를 Least Square를 통해 Visual Odometery 값을 최적화하여 구하였다.(이도 마찬가지로 stereo 카메라나 뎁스 카메라일경우 바로 3d mappoints 생성과 solvepnp를 통해 카메라 포즈와 맵포인트들을 만든다.)

그러나 Direct Method는 이런 절차와 다르게 Photometric Error를 이용하여 최적화를 한다.

### Direct Method의 약점은

- 빛에 약하다. 일정하지 못한 환경에서 취약하다.

- 비슷한 픽셀을 너무 많아 계산할때 복잡하다.

- 로테이션에 매우 취약하다.
