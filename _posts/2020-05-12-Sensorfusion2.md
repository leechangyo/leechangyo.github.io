---
layout: post
title: 2. Sensor Fusion for Localization and Multi-object Tracking
category: Sensor fusion
tag: Sensor fusion
---

<a href="https://postimg.cc/5XBhbGss"><img src="https://i.postimg.cc/rw3MC2x2/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Sensor Fusion to estimate orientation
  - Attitude and Heading reference system(AHRM)

<a href="https://postimg.cc/CBXPjHvZ"><img src="https://i.postimg.cc/WzbBc5g7/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 자력계 [magnetometer, 磁力計]

<a href="https://postimg.cc/4mzL9cpd"><img src="https://i.postimg.cc/MKLhgbTy/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/4mzL9cpd"><img src="https://i.postimg.cc/MKLhgbTy/Capture.jpg" width="700px" title="source: imgur.com" /><a>


- we can find orientation using Magnetometer, Accelerometer, Gyro

<a href="https://postimg.cc/Mv8tqGg8"><img src="https://i.postimg.cc/FsY6bdZ1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- Accleromter : 중력 방향을 이용해서 orientation을 찾는다 (accleration은 오직 위아래 Gravity만 얻는다.)
- Magnetometer : 자기력을 이용하여 북쪽을 결정한다. (기준점을 이용한 orientation구한다, 만약 Magnetic 물체가 있으면 방해를 받는다.)

<a href="https://postimg.cc/Mv8tqGg8"><img src="https://i.postimg.cc/FsY6bdZ1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 하지만 남반구인지 북반구인지에 따라 북쪽을 가르키는 방향이 다르다.

<a href="https://postimg.cc/5HH8wcD3"><img src="https://i.postimg.cc/KYfQhxNd/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 정확한 북쪽을 가르키기 위해서는 몇가지 외적이 필요하다.
<a href="https://postimg.cc/t7GmCDVf"><img src="https://i.postimg.cc/WpdR8Hp2/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 아래쪽은 가속도 벡터의 반대 방향( Measure accleration)
- East는 아래쪽과 Magnetic field의 외적이고
- 북쪽은 동쪽과 아래쪽의 외적이다.
- 따라서 본체의 방향은 본체 프레임과 NED(North, East, Down) 프레임 사이의 회전이다.
- 방금 계산한 NED 벡터에서 Direction Consine Matrix directly하게 구할 수 있다.

# let's check real device

<a href="https://postimg.cc/phR839p0"><img src="https://i.postimg.cc/9MqpRZVj/Capture.jpg" width="700px" title="source: imgur.com" /><a>
- there is a physical IMU and Accelerometer.

<a href="https://postimg.cc/ftztB6rp"><img src="https://i.postimg.cc/50vBNJq4/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- first connect to the Arduino and IMU
- and using a Matlab viewer to visualize the orientation
- update the view each time
- from for 1 = 1:1000 to Q
  - is a basically reading the sensors performing the cross products and building The DCM

<a href="https://postimg.cc/ftztB6rp"><img src="https://i.postimg.cc/50vBNJq4/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- but the systme is linear, so if moving alot it's estimate accurate down
- Accleration 센서는 오직 위아래 중력 값만 판단한다.
- Magnetic 물체가 있으면 orientation 값을 찾는데 방해를 받는다.
- 예를 들어
<a href="https://postimg.cc/m1Sm0fL9"><img src="https://i.postimg.cc/BbrRP4Rm/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 문제를 풀기 위해 Calibration 필요

<a href="https://postimg.cc/VJ229wds"><img src="https://i.postimg.cc/FKKm4svS/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/SJsPwR9N"><img src="https://i.postimg.cc/ncYt9Qc7/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/wygfFjHz"><img src="https://i.postimg.cc/W3JBNdHd/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/NLNJ7tnh"><img src="https://i.postimg.cc/GhRWRhJp/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 한가지 더 문제인 Corrupting linear accelrations

<a href="https://postimg.cc/6287kdBf"><img src="https://i.postimg.cc/76VgTVcd/Capture.jpg" width="700px" title="source: imgur.com" /><a>

  - take the commands that are sent to the actuators and play it through a model of the system to estimate the expected linear acceration and then subtract that value from the measurement
  - this is something that is possible if say our system is drone and mine is flying it around by commanding the four propellers.
  - if we can't predict the linear acceleration or the external disturbances are too high, another option is to ignore accelerometer reading that are outside of some threshold from a 1g measurement. if magnitude of the reading is not close to the magnitude of gravity, then, clearly the sensor is picking up on other movement and it can't be trusted.
  - this keeps corrupted measuremens from getting into our fusion algorithm, but it is not a great solution because we stopped estimating orientation during these times and we lose track of the state of the system again
  - it is not really problem if we trying to estimate orientation for a static object.

# Solving to get orientation value problem
- add gyro !

<a href="https://postimg.cc/xkwRFT28"><img src="https://i.postimg.cc/MTTFHv37/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/5Y8JQ0gM"><img src="https://i.postimg.cc/sXNg8GZ1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- there are two method to find orientation
  - accel + mag
  - Gyro
- accel + mag 장단점과 Gyro의 장단점을 합하여 문제점 해결

<a href="https://postimg.cc/Ny64Svfx"><img src="https://i.postimg.cc/XvhzZnRt/Capture.jpg" width="700px" title="source: imgur.com" /><a>

  - Complementary(상호 보완) Filter
  - Kalman Filter
  - Madgwick
  - mahony

- 센서 퓨전을 하면서 어떤 센서를 더 믿을 것인지 결정하는 것이 중요하다.

<a href="https://postimg.cc/23hztH4S"><img src="https://i.postimg.cc/c15rr2Cw/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Reference
Matlab Sensor Fusion.
