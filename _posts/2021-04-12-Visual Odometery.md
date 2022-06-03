---
layout: post
title: Visual Odometery & Pose Estimation
category: Visual SLAM
tag: Visual SLAM
---

### 2D - 2D Pose Estimation(Monocular, Stero를 이용한 방법)

1. 특징점 추출을 이용하여서 특징점 추출
 - ORB, SIFT, etc..

2. 이전 프레임과 현재 프레임(두 프레임)에 추출된 특징점을 매칭
 - KD-Treess, Hamming stance(Discriptor가 있을 경우 (ORB)), FLANN 을 사용하여서 매칭
 - 너무 지저분하게 매칭이 생기면 RANSAC을 통해 inlier 매칭점만 찾음

3. 찾은 매칭점(points)은 Epipolar Contraint를 통해 Fundamental Matrix와 Essential Matrix구함
 - 두 이미지에 매칭되는 점은 epipolar line(pixel 좌표와 베이스라인으로 두 다른 이미지가 마주보는 점의 라인), epipole(베이스라인으로 두이지미가 마주보는 이미지 프레인간의 점)을 통해서 호모지니어스 좌표계에서의 매칭 포인트를 찾고(epipolar line을 통해 서칭해서 찾는다)
 - 이를 통해 epipolar plane이 형성이 된다.
 - 이를 통해 Fundamental Matrix, Essential Matrix를 구할 수 있다.
   - Essential Matrix는 3x3 매트릭스이나, 요소는 랜덤 변수로 되어 있다.
   - 랜덤 변수를 구하기 위해서는 최소 5개의 점(매칭점)이 필요하지만, 잘알려진 방법 Eight-point을 사용한다.(Eight Point Algorithm, Direct Linear Transform)
   - 찾아야하는 9개의 요소를 벡터로 표현하고(e), 8개의 점 + 1(호모지니어스)식으로 8개의 row 벡터를 새워 (9x8) 행렬을 만든다음(A)
   - epipolar contraint 특징 A*e = 0 을 이용하여서 문제를 푸는데, 이때 SVD를 사용하여서 Essential Matrix를 구한다.
 - 구해진 Essential Matrix는 Homogenous Matrix를 표현을 하여 이를 통해 Rotation Matrix와 Translation을 찾을 수 있다.

4. 위와 같은 방법으로 Pose Estimation을 할 수 있다.
 - 필요한 조건으로는 카메라 파라미터(focal lenght, principal point), 두 이미지
 - 순서 :
    1. feature matching(image1, image2) // generate 2 keypoints set about two image
    2. Fundamental Matrix(keypoint1,keypoint2)
    3. essential matrix(keypoint1,keypoint2,focal lenght, prinicpal point, RANSAC);
    4. homography matrix(keypoint1,keypoint2,RANSAC,3,noArray(),2000,0.99);
    5. RecoverPose(essential matrix, keypoint1, keypoint2, R, t, focal_lengh, principal point);

5. 이를 통해서도 3D Mappoints를 구함.(triangulation)
  - 구해진 3D Mappoints들은 다시 이미지 프레임으로 RE-projection하여서 관측된 2D 매칭점과 Re projection된 2D 포인트를 least Square 방법을 통해 최적화 하여서 Visual Odometery를 추정하는 방법이 많이 쓰인다.

6. Solve Pnp를 통해 (3D-2D) 카메라 포즈를 찾는다.(3D-3D일 경우 ICP)

**NOTE: 모노 카메라일경우 위에 같은 방식으로 VO를 구함, 다만 Stereo나 RBGD같은 경우 바로 3D Mappoints를 만들고 solve pnp를 통해 카메라 포즈를 구한다.**

### 3D - 2D Pose Estimation

1. 특징점 추출 이용하여서 특징점 추출
2. 특징점 매칭
3. Fundamental Matrix 및 Essential Matrix 구하기
3. 찾은 매칭점(뎁스이미지 일 경우 특징점(1,2,3번 제외))에 Triangulation을 통해(Depth 카메라일경우 제외) 3D Mappoints 생성
4. 이때도 solvePnP 및 DLT 사용하여서 Rotation, Translation 값을 구함(Pose Estimation)

### 3D - 3D pose estimation(RGB-D or Lidar or raders)(ICP Method)

- RGB-D Sensor를 제외한 lidar 나 radars는 preprossess가 필요없으므로 바로 3D Mappoints생성 가능
- ICP 방법을 써서 pose Estimation(이전 스캔과 현재 스캔의 맵포인트 매칭을 통한 Pose Estimation)

### Bundle Adjustment(번외)

- 3D Mappoint가 이미지 프레임에서 특징점과 이미 공유되고 있는 3D Mappoint일 경우, 현재 3D Mappoint를 공유하고 있는 이미지들 플래인에 투영을 하여서 이전에 투영된 포인트화 현재 투영된 포인트를 Least Square를 통해 에러를 최소화 하면서 Camera Pose와 Mappoints들을 교정한다.
