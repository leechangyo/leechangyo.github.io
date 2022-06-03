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

- HTOP 에 보이는 것은 코어에 들어있는 Thread들 이다.

- 거대한 배터리로 부터 여러 디바이스를 달때 전압이나 전하들을 고려해야된다.
  - 예를 들어 30V 배터리가 있고, 컴퓨터 12V, LTE 5V, 센서 9V.
    - 각 디바이스 마다 정격 입력(동자학가 위해서 필요한 전압과 전류)가 있다.
      - Volt는 댐과 같기 때문에 전압을 맞춰야만 전류가 흐르게 된다.
  - 위에 조건을 맞춰주기 위해서 switch power supply를 통해서 배터리로 부터 Postivie electrode, Negative electrode로 되어있는 연결 부분으로 부터 연결을 하고, 나머지 switch power supply（12V,5V,9V)의 전선들드 그곳에다 연결을 하게 된다.
  - 전압에 맞는 siwtch power supply에다가 나머지 디바이스를 연결해주면 끝이다.

- 로봇 모터 제어에는 토크제어(전류 제어)와 속도제어(위치제어)가 있다.
- DHCP는 랜으로 연결되는 어떤 것이든 자동으로 연결이 되는 것이다.
  - 인트라넷을 사용할 경우, 아이피를 뚤어줘야 한다.
    - 할당할 local computer에서 인식할 ip주소를 입력해주고, 인터넷으로 치면 나오는 디폴트 값임 라우터와 게이트웨이를 입력한다
    - 마지막으로 lan으로 부터 오는 인트라넷 ip를 설정해주면 끝~

- 개인적으로 프로그래밍을 할 때 중요하다고 생각하는 개념 및 습관
  - Binary로 읽고, 쓰고 할 수 있는 능력
  - share_ptr, 포인터로 차원을 넓혀서 programming 할 수 있는 능력.(최대한 heap에다 올리기)
  - typedef과 이태까지 사용했던 함수들을 잘 정리하는 utils
  - 하나의 programming template을 가지고 개발하기
  - 기반 클래스, 파생 클래스에 같은 이름의 함수를 쓴다면, 기반 클래스에 virtual을 넣어주고 파생 클래스에는 override를 쎃어준다.
    - Note: virutal 기반 클래스에 소멸자 꼭 넣어줘야한다.
      - 기반클래스에 파생클래스 햇갈리지 않도록 pure viretual function도 사용하면 좋다.
  - class내에 this를 쓰는건, 클래스안에 함수나 변수들을 쓰는데 헷갈리지 않게 하려고 하는 것이다.

- Jacobian 성질은 각 변수에 대한 벡터장을 구하여 변수간의 관계를 알게 해주는 목적에 있따
- Hessian은 자코비안에대한 편미분 및 자코비안 제곱으로 인하여 변수값을 구할때 사용이 된다.(최적화 할때 많이 사용된다)
- IMU에 대한 xyz axis는 z는 중력장, x,y는 linear acceration이다.
- 로보틱스에서 intel을 많이 쓰는 이유
  - Intel processors are more powerful and speedier than ARM processors. ARM chips, on the other hand, are more mobile-friendly than Intel processors (in most cases). People who were adamant about one side or the other have been upset over the last few years.
  - kernel(비트) 체제가 다를 수 있기 때문에, 호환되는 프로그램이 arm이 적음
  - [운영체제 비트 차이에 대한 설명](https://www.studytonight.com/post/x86-vs-x64-what-is-the-difference-between-x86-and-x64-architecture)
