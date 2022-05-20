---
layout: post
title: PointcloudXYZNormal && Correspondence(ICP에 사용)
category: SLAM
tag: SLAM
---

PointcloudXYZNormal:

주로 Correspondence를 찾을때 쓴다.(Matching method)

projection pointcloud to Normal plane(considering distance between camera pose and point poise), and find Correspondence another frame's points(other also PointcloudXYZNormal);


A point structure representing Euclidean xyz coordinates, and the RGB color, together with normal coordinates and the surface curvature estimate.

Due to historical reasons (PCL was first developed as a ROS package), the RGB information is packed into an integer and casted to a float. This is something we wish to remove in the near future, but in the meantime, the following code snippet should help you pack and unpack RGB colors in your PointXYZRGB structure:


### REFERENCE

[Pointcloud Correspondence](https://blog.csdn.net/qq_36501182/article/details/79173161)

[Pointcloud Correspondence discuss](https://www.pclcn.org/bbs/forum.php?mod=viewthread&tid=1091&page=1)

[ICP - Correspondence](https://blog.csdn.net/keineahnung2345/article/details/120683309)

[Normal Coordinates](https://www.youtube.com/watch?v=KD7W4Ko-F_o)

[Normal Plane](http://mathonline.wikidot.com/normal-rectifying-and-osculating-planes)
