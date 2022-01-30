---
layout: post
title: Epipolar Contraint으로 얻어지는 Essential Matrix, Fundamental Matrix, Homography Matrix의 미지수들을 어떻게 구할까??
category: Visual SLAM
tag: Visual SLAM
---

두 이미지 시퀀스에서 특징점과 디스크립터를 FLANN알고리즘을 통해 매칭점을 찾습니다. 찾아진 매칭점들은 두 이미지 시퀀스간의 관계를 나타내는 Epipolar Contraint을 통해서 Essential Matrix와 Fundamental Matrix를 구하여서, Triangulation을 통한 맵포인트 생성과 PnP알고리즘을 통해 카메라 포즈를 구할 수 있습니다.

#### Essential Matrix

Essential Matrix는 Symmetric하고 Positive-definte하기 때문에 행렬분해를 통해서 Rotation과 Tranlation을 구할 수 있다.

그렇다면 Epipolar Contraint을 통해 Essential Matrix안에 있는 미지수를 구해야하는데 어떻게 구할까? 거기에 대한 해답은 **Eight point algorithm**이다.

Normal Plane과 8개 특징점에 대한 Epipolar Contraint을 통해서 선형화 된 식을 얻고 이를통해 Essential Matrix를 만든다. 자세한 식은 구글이나 유투브에 잘 나와있다.

얻어진 Essential Matrix는 행렬분해를 통해 Rotation과 Translation 값을 구할 수 있다.

#### Homography

두 이미지 시퀀스에 대한 epipolar Contraint에서 많이 나오는 것이 Homography이다. Homography는 두 이미지 플래인간에 투영관계를 나타내고 있다. 만약 특정점이 두 이미지 플래인에 나타나고 있다면, 이 Homography를 통해 모션에 대한 예측을 할 수 있다.
즉 두 이미지플래인에 매쳐있는 공동의 특징점들을 Homography를 통해 모션에 대한 예측을 한다. 이 Homography Matrix를 구하는것은 Fundamental Matrix를 구하는 것과 비슷하다.
이때 4개의 매칭점을 이용을 하여서 rank가 8인 Homography Matrix를 구하게 된다. 이를 Direct Linear Transform이다.(카메라 켈리브래이션할때도 많이 사용된다.) 자세한 식은 구글이나 유트브에 잘 나와있다.

얻어진 Homography Matrix는 Essential Matrix와 마찬가지로 행렬분해를 통해 Rotation과 Translation을 구할 수 있다.

Homography Matrix가 슬램에서 중요한 이유는, 특징점 및 카메라 자세에 대한 로테이션이 생겼을 경우, Fundamental Matrix의 자유도는 낮아지게 된다. 이는 Degerate 문제가 생긴다. 이때 많은 노이즈를 포함하게 되는데, 만약 Eight Point algorithm으로 Fundamental Matrix문제를 풀게 된다면, Degerate로 생기는 노이즈에 따라 정확한 값을 얻을 수 없다.
이 문제를 해결하기 위하여, Fundamental Matrix와 Homography Matrix를 동시에 계산하며, 재투영을 하였을때 최소의 오차를 가지고 있는 매칭점을 구하게 된다. 이때 모든 Point를 고려할 수 없으므로, Ransac을 통해 가치있는 포인트들을 추리고 행렬분해를 통해 Rotation과 Translation을 구한다.

#### 슬램에서 문제들

1. Homography Matrix는 순수 로테이션(Translation 이 없는)일때 사용이 된다. Essential Matrix같은 경우 Translation이 0 일 경우 Essential Matrix의 값또한 0이다. 이에 Homography Matrix를 써서 회전에 대한 값을 구한다. 그러나 모노카메라일경우 Translation이 없을 경우 Triangulation을 통한 맵포인트를 구할 수 없기 떄문에, initilaization을 할때 일정에 정해진 이동이 꼭 필요하여 초기화를 해야된다. 또한 초기화 당시 Translation이 작다면 카메라 포즈나 맵포인트들에 대한 정확도가 낮아진다. 초기화가 잘 되었다면 3D-2D 모션 추정 알고리즘을 통해 카메라 자세를 추정할 수 있다.

2. 오차가 큰 매칭점들은 RANSAC에 의해 제거를 하여서 정확한 Essential Matrix를 구한다.

3. Essential Matrix 혹은 Homography Matrix를 통해 Rotation과 Tranlation을 구하게 되었다면, 두 이미지 시퀀스의 카메라 자세, Transformation과 두 이미지간의 매칭포인트들을 통해 Mappoints를 생성 할 수있다.

4. 이렇게 생성된 카메라 자세와 Mappoints들은 PnP 알고리즘을 통해 카메라 자세를 구할 수 있다.(주로 스테레오카메라, 뎁스카메라.)(1,2,3 방법은 모노카메라 방법)

5. PnP는 주로 Direct Linear Transformation, Bundle Adjustment로 두개가 있다.(3D-2D 방법), (3D-3D는 ICP 방법)
