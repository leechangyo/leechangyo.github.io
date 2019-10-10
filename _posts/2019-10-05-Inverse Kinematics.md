---
layout: post
title: 5. Inverse Kinematics
category: ROBOTICS
tag: ROBOTICS
---

# Inverse Kinematics
<a href="https://postimg.cc/CnFj1CJC"><img src="https://i.postimg.cc/GtDzXqyf/20191010143752.jpg" width="700px" title="source: imgur.com" /><a>

## 1. General Derivation(起源)
- Not possible for serial Kinematics
- Must be derived(获得) with geometrical intuition
- Not always possible to find in closed-form
- Numerical solutions sometimes required

## 2. Non-Linear Porblem
- Example: TCP moves linearly along axis z
- Joints movements are highly non-linear
<a href="https://postimg.cc/62HQrdtf"><img src="https://i.postimg.cc/T1xWX9sF/20191010144047.jpg" width="700px" title="source: imgur.com" /><a>

## 2.1 Non-unique solution
- Same TCP pose can be reached with different joints configurations
  - to solve this, we give singularities
- infinite joints solution exist for one single TCP pose to reduce
- Dependent linear value


## 3. Inverse Kinematics Steps
1. Decouple wrist from arm
<a href="https://postimg.cc/GH6xPDpR"><img src="https://i.postimg.cc/Mpp3Sb2n/Capture.png" width="700px" title="source: imgur.com" /><a>
- find out that the position of the wrist point only depends on the first three joint axes of the robot
- moving the fourth, fifth, sixth joint will not modify the position of the wrist point
- so we can split into two part (wrist, arm) it called **Decoupling**
<a href="https://postimg.cc/dhvLKQHd"><img src="https://i.postimg.cc/pL58zpK0/20191010144710.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/HrhKGn3s"><img src="https://i.postimg.cc/VNfcCbCM/20191010145903.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/4YxdLLFR"><img src="https://i.postimg.cc/7Zg2Lj3h/20191010150135.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/Xrkh2WDQ"><img src="https://i.postimg.cc/Cx3gxLCT/20191010150337.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/qNcRjtTS"><img src="https://i.postimg.cc/CxXqMD6h/20191010150805.jpg" width="700px" title="source: imgur.com" /><a>

## 3. Base Frame and Tool
<a href="https://postimg.cc/V5HDk62x"><img src="https://i.postimg.cc/pXP7k5CL/20191010151247.jpg" width="700px" title="source: imgur.com" /><a>

## 4. Adding a tool
<a href="https://postimg.cc/m1bfZrqz"><img src="https://i.postimg.cc/nrmHkjGk/1.jpg" width="700px" title="source: imgur.com" /><a>

## 5. Couple
- output of inverse kinematics is position of real joints
<a href="https://postimg.cc/s114qz1w"><img src="https://i.postimg.cc/DfPpWyhk/20191010151405.jpg" width="700px" title="source: imgur.com" /><a>
- They must be adjusted to calculate correct motor on target positions
<a href="https://postimg.cc/qzyyHvrX"><img src="https://i.postimg.cc/CL6NDR6t/20191010151543.jpg" width="700px" title="source: imgur.com" /><a>
