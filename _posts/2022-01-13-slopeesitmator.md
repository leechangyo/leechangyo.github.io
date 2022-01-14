---
layout: post
title: slope estimation(곡선 길이 및 곡률 계산)
category: Machine Vision
tag: Machine Vision
---

with curavture, we are able to define an angle of a slope plane. the way to do is using plane parameter can get curvature.

the other one to detemine a slope plane,

set one vertical line and dot product normal vector, can define a angle of plane.


Consider a car driving along a curvy road. The tighter the curve, the more difficult the driving is. In math we have a number, the curvature, that describes this "tightness". If the curvature is zero then the curve looks like a line near this point. While if the curvature is a large number, then the curve has a sharp bend.


Normal vector마다 곡률을 구할 수 있느데, 한번 해보자

#### 2d case


<a href="https://postimg.cc/jDr1gv0C"><img src="https://i.postimg.cc/JzyWhdp3/Kakao-Talk-Image-2022-01-13-18-29-10.jpg" width="700px" title="source: imgur.com" /><a>

Fomular

```
k(t) = (x'y'' - x''y') / (x' * x' + y' * y')^(3/2)
```
