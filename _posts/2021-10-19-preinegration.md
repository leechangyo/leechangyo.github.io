---
layout: post
title: pre integration imu
category: Sensor fusion
tag: Sensor fusion
---

http://rpg.ifi.uzh.ch/docs/teaching/2016/13_visual_inertial_fusion.pdf

IMU(Inertial Measurement Unit)
- Angular Velocity
- Linear Accelerations


Why IMU?
- Monocular Vision is scale ambiguous
- Pur vision is not robust enough
  - Low texture(feature)
  - High dynamic range
  - High speed motion

- Robustness is a critial issue : Tesla Accident
"the autopilot sensors on the model S failed to distinguish a white tractor-trailer crossing the highway against a bright sky"

Pure IMU integration will lead to large drift (especially cheap IMUs)

- Intuition
  - Integration of angular velocity to get orientation: error proportional to t
  - Double integration of acceleration to get position: if there is a bias in acceleration, the error of position is proportional to t2
  - Worse, the actually position error also depends on the error of orientation.


In navigation, dead reckoning is the process of calculating current position of some moving object by using a previously determined position, or fix, and then incorporating estimations of speed, heading direction, and course over elapsed time.

Pre Integration IMU는 즉 IMU를 Odometery로, 카메라 포즈로 쓰고, 카메라는 Map constructing 혹은 keyframe추출 loop closure 같은 것으로 쓰는 것이다. 이렇게 되면, PnP알고리즘을 통해 camera pose를 구하는 CPU Computation을 줄일 수 있다.
