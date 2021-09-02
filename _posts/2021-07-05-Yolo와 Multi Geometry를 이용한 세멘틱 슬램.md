---
layout: post
title: Yolo와 Multi Geometry를 이용한 세멘틱 슬램
category: SLAM
tag: SLAM
---

easy, there are two typical methods

1) ignore feature points inside of dynamic object label generated from yolo.

2) using Epipolar constraint, estimate distance feature point inside of boundary box between two frames
and if it is over than certain threshold, then define it as dynamic point and ignore it.
