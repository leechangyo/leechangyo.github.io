---
layout: post
title: Opitcal Flow 혹은 Direct Method로 구하는 Mappoints 및 Pose estimation
category: Visual SLAM
tag: Visual SLAM
---

### Optical Flow
1. Feature Point를 찾는다.
2. 디스크립터를 사용하지 않고
3. 얻어진 Feature Point를 Optical Flow를 통해서 이미지 시퀀스간에 특징점 모션을 트래킹 한다.
4. 즉 디스크립터를 사용하는 대신에 Optical Flow를 사용한 것이다.
5. 이떄 3차원 공간에 특징점은 불변한다라는 과정하에
6. 이미지 플래인에 투영을 하여서 실제 특징점과 3D 공간상으로 투영된 특징점의 오차를 최소화 한다.
7. 이미지 NonConvexity 이유로 최적화 하는데 어려움이 있는데 이미지 피라미드를 이용하여서, 이미지 스케일 문제를 해결하고 최적화를 실행한다.
8. 즉, 예를 들어 이미지에 20개의 특징점이 있는 픽셀에 모션이 생긴다면, 0.5로 이미지를 압축을 하여서 줄어든 픽셀 모션 5개를 통하여 최적화한다.
9. Optical Flow를 통해 얻어진 것들은 Epiploar contraint, Triangularation, PnP알고리즘을 통해 맵포인트나, 자세를 추정한다.

### Direct Method
1. 특징점 없이, 픽셀로만 이용을 하여서 얻어지는 특징점(?)으로 Epipolar contraint, Triangularation, PnP알고리즘을 통해 자세와 맵포인트들을 추정하는 방법이다.
2. Direct Method 같은 경우에는 이미지 플래인에 랜덤으로 픽셀을 고른다.
3. 이떄 첫번째 이미지 플래인에서 랜덤으로 선택된 픽셀 p1의 위치와 값을 2번째 이미지 플래인에서 intensity오차를 통해 p2 위치를 찾는다.
4. 이때 이미지 픽셀에 대한 intensity는 불변한다는 가정하에 Itensity 오차를 계산하여서 p2의 위치를 최적화 한다.
5. 얻어진 p1, p2에 대한 오차 최적화에 대한 관계식으로 카메라 자세 T를 얻게 된다.
6. 이를 통해 카메라좌표계와 T를 고려한 두번째 카메라좌표계에서 q를 구하고, 얻어진 q는 내부 카메라 파라미터로 부터 이미지 플래인에서의 카메라 자세를 얻게 된다.
7. 카메라 좌표계 간의 변환을 얻는 걷은 구글이나, 유투브에 잘 나와있다(자코비안을 통해서 Hessian Matrix와 bias를 통해 정해진 iteration수만큼 리에대수를 통하여서 자세를 업데이트 추정한다.
8. 이미지 NonConvexity 이유로 최적화 하는데 어려움이 있는데 이미지 피라미드를 이용하여서, 이미지 스케일 문제를 해결하고 최적화를 실행한다.
9. 즉, 예를 들어 이미지에 20개의 특징점이 있는 픽셀에 모션이 생긴다면, 0.5로 이미지를 압축을 하여서 줄어든 픽셀 모션 5개를 통하여 최적화한다.
10. 이를 통해 6,7 방법을 통해 자세를 추정한다.


### Direct Method의 장단점
1. 특징점과 디스크립터 계산하는데 사용되는 시간을 줄였다.
2. 특징점이 없는 공간에서 사용 가능. inderect method는 특징점 이외에 장면들을 무시하지만, direct method는 모든 이미지 픽셀을 활용할 수 있음.
3. density 한 map구축 가능
4. Direct Method는 Gradient Descent에 의지를 하기 때문에 이미지는 NonConvexity의 특징을 가지고 있어, 오직 작은 모션 일때만 적용이 가능하다.
5. 비슷한 픽셀의 intensity가 많기 떄문에 복잡성이 증가한다.
6. 픽셀에 대한 Itensity가 불변한다는 가정하에 하는 작업이므로, 빛에 대해 약하다.
