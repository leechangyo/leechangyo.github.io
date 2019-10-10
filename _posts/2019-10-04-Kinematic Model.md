---
layout: post
title: 4. Kinematic Model(Direct Transformation)
category: ROBOTICS
tag: ROBOTICS
---

# Kinematic Model(Direct Transformation)
<a href="https://postimg.cc/BX2yrmDt"><img src="https://i.postimg.cc/28tDBJSd/20191010141819.jpg" width="700px" title="source: imgur.com" /><a>
- Composition of frames translation and rotation
- **Direct Transformation is a Homogeneous Matrix**
<a href="https://postimg.cc/kDQv9z1r"><img src="https://i.postimg.cc/SQ5TLpcK/20191010142017.jpg" width="700px" title="source: imgur.com" /><a>

## 1. Forward Kinematic Steps
<a href="https://postimg.cc/jDCrGG6K"><img src="https://i.postimg.cc/8zmPrGjF/20191010142242.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/Z9kDVgVw"><img src="https://i.postimg.cc/3wW5JQpQ/20191010142523.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/K44r3f5j"><img src="https://i.postimg.cc/XNk2hDGf/20191010142656.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/PPjJnrNH"><img src="https://i.postimg.cc/vH1V94s4/20191010142858.jpg" width="700px" title="source: imgur.com" /><a>

## 2. Base Frame and Toll
<a href="https://postimg.cc/3y4YDPW9"><img src="https://i.postimg.cc/d0n1gFFM/20191010143029.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/yDJ8FJbF"><img src="https://i.postimg.cc/4xPHRVvF/20191010143130.jpg" width="700px" title="source: imgur.com" /><a>

## 3. Adding a tool
<a href="https://postimg.cc/343hbWnf"><img src="https://i.postimg.cc/TwDP1Dmx/20191010143255.jpg" width="700px" title="source: imgur.com" /><a>

## 4. Mechanical Coupling
- some joint axes are mechanically **"Coupled"**
- Ex) there are two J1, J2 and if they are not coupled when J1 moved, J2 is staying joint value
- Input to Forward Kinematics is position of Real Joints
- Motor Encoder position must be adjusted

<a href="https://postimg.cc/dDzHqgKw"><img src="https://i.postimg.cc/Wb4Q7PWF/20191010143540.jpg" width="700px" title="source: imgur.com" /><a>
