---
layout: post
title: Bag of word를 통한 loop closure
category: Visual SLAM
tag: Visual SLAM
---

1. 이미지에서 얻어지는 디스크립터를 Bag of word에 add하여 word들을 생성함.
2. 이미지 순으로 bag(?)처럼 워드를 담음
3. 그리고 하나하나 bag들간에 비교를 통해 퍼센테이지가 일정 이상 만족을 하면, 이전에 보았던 씬으로 간주하고 loop closure시작
4. loop closure은 Full Bundle Adjustment를 통해 카메라 포즈(각 카메라)에 보이는 landmark들을 투영하여서 error 최소화
