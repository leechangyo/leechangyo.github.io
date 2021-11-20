---
layout: post
title: RANSAC 간단 요약[NDT-RANSAC BASED Segmentation]
category: Machine Vision
tag: Machine Vision
---

RANSAC을 2D면 최소 2개의 포인트, 3D면 최소 3개의 포인트를 Random으로 잡아서 [2D이면 line fomular]/[3D면 plane formular]를 통해 모델을 만들고 모든 포인트들을 모델에 적용하여 하여서 일정 distance threshold  inliers수를 저장을 하고 N 번만큼은 이 작업을 한다.

작업 이후 제일 많은 inliers을 가지고 있는 plane formular를 primary plane로 사용하여서 Segmentation을 한다.

RANSAC 은 주로 파라미터 모델을 찾는데 사용이 되긴하는데, 다양한 용도에서도 사용이 된다.

### NDT-RANSAC BASED Segmentation

NDT RANSAC에서는 이미 모든 포인트들에 대해 Normal distribution(Mean, covariance Matrix) 혹은 Cross product을 통한 normal vector에 대한 classified(EigenValue크기에 따라 plane, edge, point를 구분할 수 있다)를 하고 오직 Plane에 때한 벡터를 NDT CELL(Noraml surface)로 구분을 하는데, 랜덤으로 NDT CELL의 Plane Parameter(plane formular)를 픽을 하고,모든 포인트들을 해당 formular에 프로젝션을 시켜서 정한 특정 distance threshold, curvature threshold가 만족하는 포인트들을 inilier 저장하여,N 번만큼 Iteration을 랜덤으로 또 NDT CELL(Plan Formular)를 픽하고 그에 대한 inliers 수를 저장을 하여 가장 많이 inliers을 가지고 있는 NDT CELL(Plane formular)를, primary PLANE(Plane formular 모델)로 잡는데 사용 한다.

그 이후는 primary plane에 존재하는 포인트들을 가지고 least square를 통해 plane fitting을 한다.

쉽죠잉?
