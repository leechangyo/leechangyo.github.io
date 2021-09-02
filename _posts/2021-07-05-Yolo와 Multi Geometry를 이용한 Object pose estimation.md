---
layout: post
title: Yolo와 Multi Geometry를 이용한 Object pose estimation
category: SLAM
tag: SLAM
---
1)
Image -> Feature extract -> 3D matching
      -> Yolo -> boundary box

2)
ignore feature point outside of boundary box, only remain feature points inside of box.

3)
get mean or median by 3dpoints inside of boundary to estimate object pose
