---
layout: post
title: Bag of word를 통한 loop closure
category: Visual SLAM
tag: Visual SLAM
---

1. 이미지에서 특징점들과 디스크립터를 얻게 되는데 얻어지는 디스크립터에 대해 word를 만듬(디스크립터 벡터의 성향을 통해 word를 만듬)
2. 만들어진 word는 Bag of word에 담게 됨
3. 이미지당 words(vocabulary)를 가지고 있음.
4. unorder_map 함수를 통해 각 Bag of word에서의 word id를 가지고 있는 이미지 이름들을 저장을 함.
5. 비교를 할 새로운 이미지가 있다고 할 때 vocabulary를 만들고 Bag of words에 word id에서 가장 많이 share하고 있는 이미지 이름을 불러옴
6. bag들간에 비교를 통해 퍼센테이지가 일정 이상 만족을 하면, 이전에 보았던 씬으로 간주하고 loop closure시작
4. loop closure은 Full Bundle Adjustment를 통해 카메라 포즈(각 카메라)에 보이는 landmark들을 투영하여서 error 최소화
