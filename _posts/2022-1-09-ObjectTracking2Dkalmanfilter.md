---
layout: post
title: 2-D Object Tracking using Kalman Filter(single sensor)_GPS원리
category: Sensor fusion
tag: Sensor fusion
---

설명 잘 되어 있음.

https://machinelearningspace.com/2d-object-tracking-using-kalman-filter/


Environment에 대한 Motion Model만 잘 정의한다면 센서 하나를 가지고도 Object tracking을 할 수 있다.

여기서는 초기 motion에 대한 모델 정의화 초기값이 추어지고, image에서 취득되는 측정값(x,y)값을 반영하여서 update를 통한 모델 속도를 추정함과 동시에 자세를 추정을 한다.
