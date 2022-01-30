---
layout: post
title: orthophoto
category: Machine Vision
tag: Machine Vision
---


orthophoto는 카메라가 바라보는 이미지 플래인을 평행(parallel)하게 하게 바라보면서 아래와 같이 볼 수 있게 해주는 것이다.(orthogonal parallel projection)

<a href="https://postimg.cc/G9D3LLdF"><img src="https://i.postimg.cc/rynD9sHP/Screen-Shot-2021-04-08-at-9-58-13-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/7Jqqtzdq"><img src="https://i.postimg.cc/7h2b6MNJ/Screen-Shot-2021-04-08-at-9-59-57-PM.png" width="700px" title="source: imgur.com" /><a>

그리고 Bundle Adjustment를 이용하여서 이미지들을 합성하여 구글맵처럼 크게 연결을 해둔다.

orthophoto를 하려면 아래와 같은 조건이 필요하다.

<a href="https://postimg.cc/zbYN3QCd"><img src="https://i.postimg.cc/NM5jCq5w/Screen-Shot-2021-04-08-at-10-03-16-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/NKb261J7"><img src="https://i.postimg.cc/TPvjX0nS/Screen-Shot-2021-04-08-at-10-04-29-PM.png" width="700px" title="source: imgur.com" /><a>

DTM의 문제는 고층빌딩과 빌딩들을 제대로 orthophoto 표현해주지 못한다.

DSM은 레이저 스캐닝을 통해 z값을 얻을 수 있으므로 고층빌딩에 대한 orthophoto 표현을 할수 있다.

<a href="https://postimg.cc/6T5ZQbM6"><img src="https://i.postimg.cc/dQGj4zdC/Screen-Shot-2021-04-08-at-10-06-56-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/8FSJgZFS"><img src="https://i.postimg.cc/TPyrmSfp/Screen-Shot-2021-04-08-at-10-07-38-PM.png" width="700px" title="source: imgur.com" /><a>

orthophoto를 구하는 방법은 다음과 같다.

<a href="https://postimg.cc/v4cFHyV7"><img src="https://i.postimg.cc/P5Mtyrf7/Screen-Shot-2021-04-08-at-10-08-42-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/tYnNXmgZ"><img src="https://i.postimg.cc/qMjbxPVL/Screen-Shot-2021-04-08-at-10-10-44-PM.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/3yGwNmvZ"><img src="https://i.postimg.cc/43LH814j/Screen-Shot-2021-04-08-at-10-11-34-PM.png" width="700px" title="source: imgur.com" /><a>

orthophoto plane을 따로 만들고, 특정 포인트를 찍어서 orthophoto 그리고 실제 지형, 그리고 camera image plane, 카메라 파라미터를 이용을 하여서 orthogonal image에 표현을 하며, camera image plane에서 보이는 intensity값을 그대로 적용한다.
