---
layout: post
title: Zhang's Method Calibration & Direct Linear Transformation
category: Machine Vision
tag: Machine Vision
---

Zhang's method는 체크보드와 Direct Linear Transformation을 통해 캘리브레이션 하는 방법을 소개한다.

먼저 히어리어스 코너 디텍션으로 통해 체크보드의 꼭지점을 구하고, Direct Linear Transformation을 통해서

카메라 파라미터를 구한다.

Direct Linear Transformation은 이런 파라미터를 구하는 방법 뿐만 아니라 이미지의 왜곡을 잡아주는데도 사용하게 된다(affine)
