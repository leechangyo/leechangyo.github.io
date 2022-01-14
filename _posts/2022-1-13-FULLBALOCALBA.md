---
layout: post
title: 간단 FULL BA and LOCAL BA
category: SLAM
tag: SLAM
---

### BA소개

Structure from Motion을 수행할 때, 다수의 프레임에 존재하는 Visual keypoint 들의 위치 (i.e. 2D pixel location)를 기반으로 추정할 수 있는 3D landmark position과 카메라 프레임간의 3D relative motion을 동시에 최적화하는 과정

SLAM에서는 다음과 같은 방법으로 사용한다.

1. Sliding window optimisation
  - 실시간으로 SLAM 프로그램을 돌리면서, 마지막 N개의 keyframe 정보를 기반으로 Bundle adjustment를 수행하여 실시간으로 local map + pose를 보정하는 작업

2. Loop closure
  - 실시간으로 SLAM을 돌리면서, Loop closure detection이 일어났을 때 Loop closure optimisation을 다른 쓰레드에서 비실시간으로 수행하여 loop에 대한 pose + global map을 보정하는 작업

3. Global optimisation
  - 실시간으로 SLAM을 돌리면서, 종종 다른 쓰레드에서 비실시간으로 모든 map + pose를 보정하는 작업. (잘 안쓰임)

4. Global optimisation as 후처리
  - SLAM이 끝나고나서, 모든 map + pose를 비실시간으로 보정하는 작업.


### 간단 메카니즘

<a href="https://postimg.cc/kVtZ6Qpy"><img src="https://i.postimg.cc/vmPdKX7C/ba.png" width="700px" title="source: imgur.com" /><a>

- 보통 closed-form 방식으로 initial guess 정보가 있음.
  - Initialization 단계에서 Homography/Fundamental matrix를 이용하여 얻어낸 relative motion 정보 + 이후 PnP로 추가해내간 relative motion 정보.

  - Initialization 단계에서 초기 R|t로 얻어낸 3D landmark position 정보 + 이후 triangulation으로 추가해내간 3D landmark position 정보.

  - 하지만 이러한 정보들은 어쩔 수 없이 error를 내포하고 있음.

- 위의 error를 보정하기 위해 최적화를 진행함.
  - 3D landmark에서 카메라 coordinate의 원점까지의 직선을 ‘ray’로 표현할 수 있음.
  - 이러한 ray가 다수 있을 경우 ‘bundle of ray’라고 표현함.
  - 이 ‘bundle of ray’를 보정 (i.e. adjust) 한다는 의미로 bundle adjustment라는 이름을 사용함.

- **슬라이딩을 하면서 이전에 카메라 프레임정보(points,pose,extrinic,instrinic parameters)(Local일 경우 i_th번째까지의 카메라 프레임, Full일 경우 전체 프레임 정보)와 현재 프레임정보를 cost function을 통해서 Iteration K만큼 포인트들과 포즈 파라미터들을 최적화 하는 작업**

### Cost Function(Reprojection error)

<a href="https://postimg.cc/p5DRfNcq"><img src="https://i.postimg.cc/tJcJHp3Q/Screen-Shot-2022-01-14-at-5-12-07-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/8jMxzBxB"><img src="https://i.postimg.cc/15jSMvHT/Screen-Shot-2022-01-14-at-5-17-37-PM.png" width="700px" title="source: imgur.com" /><a>


#### Image Projection 방법

<a href="https://postimg.cc/RWz1vczt"><img src="https://i.postimg.cc/sgMT9c6T/Screen-Shot-2022-01-14-at-5-30-43-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/1Vt6YHwx"><img src="https://i.postimg.cc/6qV0qbQQ/Screen-Shot-2022-01-14-at-5-24-14-PM.png" width="700px" title="source: imgur.com" /><a>

#### Parameter 수

<a href="https://postimg.cc/TLhgt0nz"><img src="https://i.postimg.cc/76n9YQhZ/Screen-Shot-2022-01-14-at-5-28-19-PM.png" width="700px" title="source: imgur.com" /><a

<a href="https://postimg.cc/SnvyWGN2"><img src="https://i.postimg.cc/SKyJbZBr/params.png" width="700px" title="source: imgur.com" /><a>

#### Sparse한 매트릭스 성질을 이용하여서 symmetric matrix 성질을 이용한 QR한다.

<a href="https://postimg.cc/BLY90SL8"><img src="https://i.postimg.cc/wMH6D39k/Screen-Shot-2022-01-14-at-5-37-08-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/rzxC3hBT"><img src="https://i.postimg.cc/NGPJySbK/Screen-Shot-2022-01-14-at-5-41-17-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/rKY1tvWS"><img src="https://i.postimg.cc/8zTtxGzn/Screen-Shot-2022-01-14-at-5-43-01-PM.png" width="700px" title="source: imgur.com" /><a>
