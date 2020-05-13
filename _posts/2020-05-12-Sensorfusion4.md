---
layout: post
title: 4. Tracking a Single Object with an IMM Filter
category: Robotics
tag: Robotics
---

<a href="https://postimg.cc/B8h2FSht"><img src="https://i.postimg.cc/zBN793Kw/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/7b1Ytskn"><img src="https://i.postimg.cc/Y2ZG9ZDZ/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/MvW1K3b9"><img src="https://i.postimg.cc/Qx7mmRBZ/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- Interacting multiple model iflter is showing much better

# why IMM is more efficient?

<a href="https://postimg.cc/SX0sjJbm"><img src="https://i.postimg.cc/TP1W7Dk5/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- but it has data assosiation problem
  - if many object is moving , it is distributed

- predict next trajectory is important

<a href="https://postimg.cc/BjFZW4Lk"><img src="https://i.postimg.cc/Gm74ypcL/Capture.jpg" width="700px" title="source: imgur.com" /><a>
<a href="https://postimg.cc/HVqH9RZ2"><img src="https://i.postimg.cc/RZFF4zwj/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# Motion comes from

<a href="https://postimg.cc/gLLQwkWP"><img src="https://i.postimg.cc/d35YxLND/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# how do we account for control inputs?

<a href="https://postimg.cc/WFCfPLJY"><img src="https://i.postimg.cc/Kj8XBmcb/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# if control input we don't know?

it is uncooperative

<a href="https://postimg.cc/TLLC07HV"><img src="https://i.postimg.cc/sXKqVbYN/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/McXDZ3wC"><img src="https://i.postimg.cc/8CmnHx0z/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/RqwT0HrZ"><img src="https://i.postimg.cc/ryPZQ1Qx/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 에어플레인의 스피드와 턴을 모르니 우리는 accounted for with process noise를 증가시켜야 한다.

<a href="https://postimg.cc/qzMQZvYB"><img src="https://i.postimg.cc/CLbt5Zs8/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/T5PmQpPZ"><img src="https://i.postimg.cc/PJW4XDnr/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 아까와 같이 터닝하는 포인트에 low process noise같은 경우 unit의 양을 가늠할 수 없을 정도로 커지는데 high process noise 같은 경우 터닝하는 포인트의 증가하는 표준편차값이 줄어들었다.

# the problem can be solved by Multiple Model Filter

<a href="https://postimg.cc/JDSmYF7M"><img src="https://i.postimg.cc/Zn5Rnk1B/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- 하지만 state estimation의 previsou와 current의 상관관계를 업데이트하기 위하여 state covariance를 필터에 업그데이트 시킨다.

<a href="https://postimg.cc/8F5SpGjT"><img src="https://i.postimg.cc/zfwXkXNH/Capture.jpg" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/bs9c1rnz"><img src="https://i.postimg.cc/gkfzF6k6/Capture.jpg" width="700px" title="source: imgur.com" /><a>

# why not run an IMM with a million models?

<a href="https://postimg.cc/QVsrCcB6"><img src="https://i.postimg.cc/7PJPBVqy/Capture.jpg" width="700px" title="source: imgur.com" /><a>

- usually model used 3 or four
