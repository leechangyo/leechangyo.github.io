---
layout: post
title: 3. Fusing a GPS and IMU to Estimate Pose
category: Sensor fusion
tag: Sensor fusion
---

<a href="https://postimg.cc/Fdm4XvbF"><img src="https://i.postimg.cc/G2ThYhHD/Capture.jpg" width="700px" title="source: imgur.com" /><a>

last section we combined the sensor in an IMU to estimate an object's orientation
in this section we add a GPS sensor, GPS can measure position and velocity and so in this way we can extend fusion algorithm to estimate them as well and

# Hot to get position ? GPS!

<a href="https://postimg.cc/RWzGR0v0"><img src="https://i.postimg.cc/3xk63y62/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/Tpvr8HF7"><img src="https://i.postimg.cc/Nf5bH34c/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# exercise

<a href="https://postimg.cc/D4Pm354H"><img src="https://i.postimg.cc/NFWXTzss/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- GPS만 사용했을떄 Orientation에 대한 값은 엉망이지만 Postion 에 대한 값은 Ground Truth와 비슷하다.

<a href="https://postimg.cc/LJX5Gt9Q"><img src="https://i.postimg.cc/dVRCk6zF/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 오른쪽에 보이는 것은 X,Y,Z에 대한 값인데 무슨 값이냐 하면, Ground Truth와 현재 실험하는 데이터 값에 대한 정확도에 대한 표준 편차이다.
- 오른쪽 맨위는 QUaternion 값으로 Ground Truth와 비교하는 값이다.

- 만약 속도를 올리면 GPS만 사용할 경우에도 position 값이 정확하지 않다.

<a href="https://postimg.cc/s1jMpCFR"><img src="https://i.postimg.cc/FFc0XhD1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/0b62f8ch"><img src="https://i.postimg.cc/Qxm9jC5N/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- IMU가 데드레콘으로 업데이트가 되면서 GPS하고 퓨전이 된다. 여기서 쓰인 방법으로는 EKF 방법이다.

<a href="https://postimg.cc/WdwRmCLQ"><img src="https://i.postimg.cc/YCJp7BXq/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/ft0BCSSJ"><img src="https://i.postimg.cc/sXTqVpV9/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- Estimating sensor bias is important

<a href="https://postimg.cc/Jy3bz3X3"><img src="https://i.postimg.cc/FK8GGDs6/Capture.jpg" width="700px" title="source: imgur.com" /><a>

  - bias가 점점 벗어나게 된다면 잘못된 값을 얻게 되기 떄문이다.
  - Calibration을 하더라도 Draft가 되지 않게 주의해야한다.
  - initial bias estimate가 중요하다, 그러기 위해서는 시간을 주어서(Coverage) intilaize를 하는 것이 좋다.
  - bias draft가 생겼을때 보정할 수 있는 필터가 필요하다.

- Initialize

<a href="https://postimg.cc/R3mHy3YW"><img src="https://i.postimg.cc/DzJPWLdP/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Real system ( we don't know ground truth)

<a href="https://postimg.cc/vDCDCggG"><img src="https://i.postimg.cc/qMvnh2p6/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Kalman Filter
filter inilizied we can start with kalman filter, every common filter consists predict and correction

<a href="https://postimg.cc/1fZsppVV"><img src="https://i.postimg.cc/3JkNQ1K1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/T56FWJNR"><img src="https://i.postimg.cc/x8J2hsKb/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 만약 GPS만 쓴다면 Prediction Step은 1초에 Velocity가 변화지 않는다고 가정하고 since there were no update to correct that assumption the estimate would drastically run away from truth
- however with the IMU, the filter is updating a hundred times a second and looking at the accelerometer in seeing that the veolocity is in fact changing. so in this way the filter can react to a changing state faster with the quick updates of IMU, then it can with the slower updates of the GPS
- once the filter converges and it has a good estimate of sensor biases then that will give us an overall better prediction and therefore a better overall state estimation. that is power of sensor fusion.


<a href="https://postimg.cc/T56FWJNR"><img src="https://i.postimg.cc/x8J2hsKb/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- play around with this! we can check

<a href="https://postimg.cc/WDcHynj0"><img src="https://i.postimg.cc/d0hcLHxN/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Reference
Matlab Sensor Fusion.
