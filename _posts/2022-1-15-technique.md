---
layout: post
title: Vision Algorithm Engineer로 자주 사용되는 Technique(Keep Update)
category: daily
tag: daily
---

### 자주 사용되는 테크닉들

Feature matching : feature extractor, KNN search matching.

Triangulation : Epipolar contraint to matching features

Motion Esitmation : Fundemantal matrix and essential matrix(Direct Method, using Epipolar contraint)


Elevation Mapping :
- Sensor Measurement Variance Update : 참고(Elevation Mapping), Sensor Jacobian * Covariance matrix * Sensor Jacobian^T

- 2D Vehicle에서의 kinematic Model Jacobian Matrix(Motion Update) :
  - Yaw 축을 고정한 Pith raw에 대한 Kinematic Model에 대한 Jacobian Matrix를 만들고 이를 Pose에 관한 Covariance Matrix로 Yaw축이 고정된 Covairance Matrix를 만듬.
  - Tangent pitch로 2D affirmation matrix에서의 x,y에 대한 값들을 고려할 수 있다.(z축 고정)
  - Yaw축 기준의 이전 covariance matrix와 현재 Covariance Matrix와 이전 robot pose과 현재 robot pose를 고려하여서 모션에 대한 Varaince를 구하여서 활용할 수 있다.

```c++
  // 오일러 각은 강체가 놓인 방향을 3차원 공간에 표시하기 위해 레온하르트 오일러가 도입한 세 개의 각도이다. 즉, 3차원 회전군 SO의 한 좌표계다. 3차원 공간에 놓인 강체의 방향은 오일러 각도를 사용하여 세 번의 회전을 통해 얻을 수 있다.
  kindr::EulerAnglesZyxPD eulerAngles(robotPose.getRotation()); // get eulerangle(오일러각)
  double tanOfPitch = tan(eulerAngles.pitch());
  // (A.5)
  Eigen::Matrix<double, 1, 3> yawJacobian(cos(eulerAngles.yaw()) * tanOfPitch, sin(eulerAngles.yaw()) * tanOfPitch, 1.0);
  Eigen::Matrix<double, 4, 6> jacobian;
  jacobian.setZero();
  jacobian.topLeftCorner(3, 3).setIdentity();
  jacobian.bottomRightCorner(1, 3) = yawJacobian;
```

[Rigid Body - Kinematic Model - EulerAngle Releationship](https://ece.montana.edu/seniordesign/archive/SP14/UnderwaterNavigation/Euler%20Angles.html)


- Tagnet vector Curvature를 이용한 Feature Extraction(Lidar), Slope Estimation

- Normal Vector 구하는 법 : cross product 혹은 Covariance Matrix

- Ground Surface 구하는 법 :  number of normal vector who satisfied threshold of difference between normal vector D and similirity threshold(innerproduct between normal vectors) over threshold of of connected normal

- Tracking : Kalman Filter, Extended Kalman Filter, UKF, PSO, Particle Filter.

- camera Pose optimization : Bundle Adjustment.

- Outlier remove : RANSAC

- Loop closure detection :
  - PGO(certain pose node is in range of past pose node, then loop closure) - node matching
  - BoW, scan context - descriptor matching

- Least Square Problem(Convex optimization(cost function, object function)) : Gaussian Newton, LM Method etc.

- 3D Matching Method : PnP, KD -tree
  - Pose and orientation Optimization :(NDT, ICP 방법이 있음)

- NDT는 Probabilistic Density를 통해서 가장 높은 유사도를 가지고 있는 포인트들만 cost function으로 최적화.

- Factor Graph과 Pose Graph의 차이점은 Factor Graph은 PDF를 사용하는 것이고, Pose Graph는 특정 range에 되었을때 Contraint들이 closed가 되어서 minimization을 하는 것이다.

- Clustering : KD tree, K-means(it is used in machine learning a lot of times.)

- 3D Pointcloud space complexity를 잘 화용하는 법 Octree map.

- G2O : Graph based optimization (node - edge)
- Ceres : least square 문제 해결

- TTL : Collision rate(using Boundary box(Maximum-minum x,y,z estimate TTL))

- SVM : affine image(calibration multi camera and image stitching)

- DownSampling : Voxelization(means and covariance)

- Line Detection : edge extraction + binary operation in certain roi

- Exploration : Computer vision(edge Extraction + Binary Operation) or KD-TREE(Search cluster of unknown region)

- Global Path Planning : RRT

- Local Path Planning : (DWA + EB) or TEB

- Object Detector = Yolo(backbenc(Resnet) + dataset(coco) + regression boundary

- Semantic SLAM : moving consitenct check(epipolar contraint) + label. remove labeled dynamic and store it some specific labeled semantic information(extracted by feature extractor such as orb) in keyframe and store it in map database.

- Multi Geometry View : TODO

- Camera Lidar Calibration ：ICP Calibration, Checkboard calibration
  - 주의할 것 : Calibration을 할때 Pointcloud들을 base_link 좌표에서 표현이 될수 있도록 PointCloud에 센서와 베이스링크의 Transformation을 대략적으로 구해야 된다.
  - 손으로 대략 구해진 Transformation Matrix를 inverse 해서 pointcloud에 적용한다(base link로 프로젝션)
  - 두 작업이 끝났으면, ICP Matching방법을 써서 카메라와 라이다간의 Tranformation Matrix를 구한다.
  
