---
layout: post
title: RANSAC 간단 요약
category: Machine Vision
tag: Machine Vision
---

RANSAC을 2D면 최소 2개의 포인트, 3D면 최소 3개의 포인트를 Random으로 잡아서 normal vector를 만들고 모든 포인트들을 normal vector plane에 프로젝션을 하여서 일정 distance threshold and curvature에 만족하는 inliers수를 저장을 하고 N 번만큼은 이 작업을 한다.

작업 이후 제일 많은 inliers을 가지고 있는 normal vector plane에 대한 모델 파라미터를 primary plane으로 정하는, 모델 파라미터를 찾는 작업이다!


NDT RANSAC에서는 이미 모든 포인트들에 대해 Normal distribution(Mean, covariance Matrix) 혹은 Cross product을 통한 normal vector에 대한 classified(EigenValue크기에 따라 plane, edge, point를 구분할 수 있다)를 하고 오직 Plane에 때한 벡터를 NDT CELL(Noraml surface)라고 하는데, 랜덤으로 NDT CELL를 픽을 하고, NDT CELL에 포함된 포인트들을 Plane에 프로젝션을 시켜서 정한 특정 distance threshold, curvature threshold가 만족하는 포인트들을 inilier로 구분하여서 N 번만큼 Iteration을 하여서 가장 많은 inlier를 가지고 있는 NDT CELL을 primary PLANE(Plane Ceofficient 모델)으로 잡는데 사용 한다.

그 이후는 primary plane에 존재하는 포인트들을 가지고 least square를 통해 plane fitting을 사용한다.
