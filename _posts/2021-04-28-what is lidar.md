---
layout: post
title: what is lidar sensor
category: Sensor fusion
tag: Sensor fusion
---

## what is lidar Sensor

Lidar is a sensor. Lidar stands for light detection and ranging. Like analog to radar. It sends out beams of light, and it measures how long it takes for the beam of light to come back.  So it determines the distance to an object. A lidar contains a laser. A lidar is just a contraption basically consisting of a laser, a method of deflection basically scanning that laser beam across the field of view, and photo diode or a photo detector that detects the incoming photons that are reflected from the object. So the laser sends out a very short pulse, few nanoseconds, and then measures the time it takes for the pulse to go to the object, and back to the lidar where it's detected. This time gives us then an indication, or gives us the exact distance to that object.
 You scan this laser beams across the field of view, which means you get for each point in the field of view, a distance to an object or no distance if there is no object there whatsoever. From this, it tells you exactly what objects there are,
and how far away they are. We use lidar as one sensor in our overall sensor suite.  We have several different kinds of sensors. We have lidar, we have radar, and we have various cameras, and ultrasound. All of those sensors work together to detect, and track objects in the environment.


## Lidar Sensor

Lidar sensing gives us high resolution data by sending out thousands of laser signals. These lasers bounce off objects, returning to the sensor where we can then determine how far away objects are by timing how long it takes for the signal to return. Also we can tell a little bit about the object that was hit by measuring the intensity of the returned signal. Each laser ray is in the infrared spectrum, and is sent out at many different angles, usually in a 360 degree range. While lidar sensors gives us very high accurate models for the world around us in 3D, they are currently very expensive, upwards of $60,000 for a standard unit.

•	The Lidar sends thousands of laser rays at different angles.
•	Laser gets emitted, reflected off of obstacles, and then detected using a receiver.
•	Based on the time difference between the laser being emitted and received, distance is calculated.
•	Laser intensity value is also received and can be used to evaluate material properties of the object the laser reflects off of.

<a href="https://postimg.cc/K1bxTF2f"><img src="https://i.postimg.cc/4x7drN2D/Picture5.png" width="700px" title="source: imgur.com" /><a>

Here are the specs for a HDL 64 lidar. The lidar has 64 layers, where each layer is sent out at a different angle from the z axis, so different inclines. Each layer covers a 360 degree view and has an angular resolution of 0.08 degrees. On average the lidar scans ten times a second. The lidar can pick out objects up to 120M for cars and foliage, and can sense pavement up to 50M.

<a href="https://postimg.cc/w1D5WXnH"><img src="https://i.postimg.cc/Qdy6dSbB/Screen-Shot-2021-04-27-at-7-45-27-PM.png" width="700px" title="source: imgur.com" /><a>
