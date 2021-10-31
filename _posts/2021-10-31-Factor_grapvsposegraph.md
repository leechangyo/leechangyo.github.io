---
layout: post
title: Factor Graph vs Pose Graph and Bundle adjustment
category: SLAM
tag: SLAM
---
Factor graph optimization is an optimization of any generic factor graph with nodes (states) and edges (constraints), for example you can have IMU preintegration constraints between two poses that you wish to minimize the error of based on the covariance matrix of the measurements.

Bundle adjustment is a special case of factor graph optimization where the only states are camera poses and landmark position, and the only constraints are the reprojection constraints from the landmarks to the cameras.

I'm not sure about g2o, but you can definitely use ceres to do either, as its just the case of defining the correct cost functions

https://arxiv.org/pdf/1704.05959.pdf


Bundle adjustment
http://jinyongjeong.github.io/2020/03/01/Jacobian_of_BA/
