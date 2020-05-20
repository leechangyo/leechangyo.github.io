---
layout: post
title: 5. how to track multiple object at once
category: Sensor fusion
tag: Sensor fusion
---

<a href="https://postimg.cc/ZCBjhMTX"><img src="https://i.postimg.cc/ZKjQpGXY/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Elements of a multi-object tracking systems

<a href="https://postimg.cc/Z0gFjxrH"><img src="https://i.postimg.cc/656jWHqk/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- vague 모호한

# What makes multi-object Tracking difficult?

<a href="https://postimg.cc/w19tk1bF"><img src="https://i.postimg.cc/9Q7GCTP3/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/0z5QwLbB"><img src="https://i.postimg.cc/2ShBPYmr/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- prediction과 measure 겹치는 부분을 tracking 해서 멀티 옵젝트 tracking을 한다.

- 하지만 만약 아래와 같은 문제가 생긴다면 어떻게 할 것인가?

<a href="https://postimg.cc/BtG1v7cd"><img src="https://i.postimg.cc/y6DhvwJY/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- this is data association problem

# the number of objects being tracked is not fixed
- sometimes tracks need to be created or removed.

<a href="https://postimg.cc/sBd3JqXz"><img src="https://i.postimg.cc/SKjjmpbn/Capture.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/jLBTsmyf"><img src="https://i.postimg.cc/L888VMXx/Capture.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/kDZz88D3"><img src="https://i.postimg.cc/BQQ0rB06/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 만약 prediction과 measurement가 멀다면 에어플레인의 track을 delete한다.
- 하지만 false positive나 missed detection이 된다면 큰 오류가 발생한다.
- this is track maintenance problem

# when tracking multiple objects what are some ways to approach
- data association
- track maintenance

## obesrvation step

<a href="https://postimg.cc/xXdm2ky8"><img src="https://i.postimg.cc/GmFxvG4F/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/Z9dFGB43"><img src="https://i.postimg.cc/438LhV08/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/kRbym2xL"><img src="https://i.postimg.cc/gjMt3Rrc/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- all those thing is handling in Observation steps

## assignment step

- Matching an obesrvation to a track

<a href="https://postimg.cc/34MRqWxk"><img src="https://i.postimg.cc/nzrDCD8G/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- Eclidian distance로 assignement를 하는것이 아는 probability distribution으로 assignement를 한다.

<a href="https://postimg.cc/JtYnwT0w"><img src="https://i.postimg.cc/5NbFCTMt/Capture.jpg" width="700px" title="source: imgur.com" /><a>

## track maintenance

- deleting and creating tracks

<a href="https://postimg.cc/cvm2jz1w"><img src="https://i.postimg.cc/q7Vrtf9b/Capture.jpg" width="700px" title="source: imgur.com" /><a>

## estimation filtering
- we may choose to ignore obseravtion outside of a defined region for each track

## gating
- it's a screening mechanism that determines which detections are valid candidate to look at for assignment and which should just be flat-out ignored for


<a href="https://postimg.cc/hzFL0DFr"><img src="https://i.postimg.cc/qRq14qj9/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Exercise

<a href="https://postimg.cc/3drcGBFw"><img src="https://i.postimg.cc/MK12h3c1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/mzRwsLdr"><img src="https://i.postimg.cc/ZRvD60p6/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- GNN(Global nearest neighbor) for Data association
- IMM for estimation filter
<a href="https://postimg.cc/625vcyjC"><img src="https://i.postimg.cc/Sscfy9c1/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- speed up
<a href="https://postimg.cc/Kkw8TzdG"><img src="https://i.postimg.cc/d3tkzZ6y/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- JPDA for Data association
