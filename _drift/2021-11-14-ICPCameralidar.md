---
layout: post
title: Lidar와 Camera icp 할때 initial pose
category: Machine Vision
tag: Machine Vision
---

```c++
// lidar and camera link extric pararmter
Value& quaternion = calParam[0]["quat"];
Value& translation = calParam[0]["trans"];

Eigen::Quaternionf qvec;
Eigen::Vector3f tvec;

qvec.x() = quaternion[0].GetDouble();
qvec.y() = quaternion[1].GetDouble();
qvec.z() = quaternion[2].GetDouble();
qvec.w() = quaternion[3].GetDouble();

tvec.x() = translation[0].GetDouble();  
tvec.y() = translation[1].GetDouble();   
tvec.z() = translation[2].GetDouble();  

Eigen::Matrix4f init_guess = Eigen::Matrix4f::Identity();

// put that in matrix.
init_guess.block<3, 3>(0, 0) = qvec.toRotationMatrix();
init_guess.block<3, 1>(0, 3) = tvec;

//
initial_pose_list_[0] = init_guess.inverse();    

// initial_pose_list_[1] << 1, 0, 0, 0,
//                          0, 1, 0, 0,
//                          0, 0, 1, 0;
```

이유는 그냥 단순, camera link에서 얻어지는 pointcloud 데이터들을 lidar coordinate로 project를 하고 싶기 때문에 inverse를 해준 것이다.

READ ICP

https://www.researchgate.net/figure/Affine-transformation-matrix-and-its-inverse_fig1_242299747
