---
layout: post
title: why sensor fusion need | sensors pros & con | Kalman Filter
category: Sensor fusion
tag: Sensor fusion
---

## Introduction to sensor fusion

### 1.	Sensor fusion
Uncertainty
Sensor has their own pros & con
So need to sensor fusion
Even sensor fusion it is difficult to figure out world

### 2.	Sensors
In the world, there are a lot of sensor existing like camera lidar radar gps imu, even biologic sensor

### 3.	Lidar
Active sensor
Laser sensor
FOV 360 degree
Vertical field of view(how high, how low)
Point cloud(intensity, it used at ML/DL for detecting object)

### 4.	Camera
Passive sensor
Understanding night vision
High resolution
Image only can be read by camera
Camera can be worked as active sensor by preprocessing algorisms (intensity, feature based)

### 5.	Radar
Active sensor
Signal process
Understand potion and velocity
Propagate out and get back frequency from unit
Low resolution
Just can figure where is object like this
Cost effective

### 6.	Fusion
Merge sensors
Figure out environment using Mathematical
Prediction and update cycle(update predict by actual observation data)
Sensor often continuous form
But we can change discrete form
Choose correct sensor

### 7.	Kalman Filters
Linear form

But real world is non-linear (curve or unexpected behavior)

So need to use extended Kalman filter(Taylor 1st series, not long term driving good, because their error is accumulated) or unscented Kalman filter(sampling and correct(like particle filter, long drive advance)
