---
layout: post
title: Essential Matrix, Fundamental Matrix
category: Machine Vision
tag: Machine Vision
---

스테레오 카메라를 이용해서 3D mappoints를 구하는데 있어 필요한 essential Matrix와 Fundamental matrix가 있다.

이는 epipolar contraint을 통해서 Essential matrix나 Fundamental matrix를 구할 수 있는데

두 이미지에 매칭하는 피처포인트에 대해서 frame과 frame 간의 연결 할때 생기는 epipole 그리고 3차원 공간에서서 교차해서 만나는 포인트를 찾기 위해

epipole과 interest 특징점을 연결을 한 선이 epipoline으로, 프래임간 에피폴라라인으로 서칭을 통해 매칭점에 대한 3차원 위치를 찾는다.

이 찾게 된 3D 상에 매칭점은 epipoline과 baseline, epipole 등으로 통해 Fundamental Matrix를 구할 수 있으며

Essential Matrix는 E = K^t* F * K인 카메라 파라미터를 통해 구할 수 있다.

구해진 Essential Matrix는 essential matrix triangulation을 통해서 3D Mappoints가 구해지게 된다.

번외로, 구해진 3D Mappoints들은 카메라 포즈를 최적화 할때도 사용하게 되는데. 현재 프레임에서 추출된 이미지 플래인에서의 특징점과 3D Mappoints에 이미 가지고 있는 특

징점을 리프레젝션을 하여서 Bundle Adjustment(Least Square)를 통해 iterate를 하여서 최적화를 시킨다.

e(x-x^)M e^t(x-x^) = 0

위와 같은 에러 펑션과 Information matrix(Correspond matrix)를 이용한 알고리즘(BA)이다
