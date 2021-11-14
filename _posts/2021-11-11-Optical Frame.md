---
layout: post
title: Optical Coordinate
category: Machine Vision
tag: Machine Vision
---

카메라에서 바라보는 Coordinate는 Optical Coordinate이다.


<a href="https://postimg.cc/18JTm8hG"><img src="https://i.postimg.cc/PqjkMDSR/Kakao-Talk-Image-2021-11-14-09-58-54.jpg" width="700px" title="source: imgur.com" /><a>

우리의 object는 로봇이기 때문에 카메라로 우리가 보고 있는 Pointcloud들은 로봇 Coordinate로 변환을 시켜줘야 한다.

변환 하는 방법은 아래와 같다.

```
R = 0  0  1
   -1  0  0
    0 -1 0

q = 0.5, -0.5, 0.5, -0.5
```

하나하나 대입을 하면 Coordinate 변환을 알 수 있다

Robot Base link를 고려하기 않고 camera link로만 어떤 작업을 할 때 많이 사용 한다.
(로봇 베이스를 통해 세계를 바라 보고 싶다면, extrict parameter만 고려해주면 된다.)

관련 테스트는

https://www.andre-gaschler.com/rotationconverter/

으로 로테이션 확인을 할 수 있다.
