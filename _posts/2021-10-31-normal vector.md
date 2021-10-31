---
layout: post
title: normal vector와 normal plane에서의 margin의미
category: calculus
tag: calculus
---

3차원을 가정한다면 법선벡터는 normal surface를 만든다고 생각하면 된다. 이때 최소 3개 이상의 벡터들을 covraincae matrix나 혹은 외적을 이용해서 법선 벡터를 만들 수 있는데, 이는 plane의 수직 방향 벡터에 대한 방향과 curvature, plane 각도 들을 알 수 있게 된다. 이와 같은 정보를 통해서 평면과 혹은 바닥을 검출해 낼 수 있다.

간단 소개
https://adioshun.gitbooks.io/pcl/content/Tutorial/Feature/Normal-Estimation.html

공부를 하다 보면 plane margin이라는 말이 나오는데 이는 perpendicular distance 이다. 즉 주변 포인터를 통해 normal surface 형성을 하고, 주 변포인터와 normal surface간에 distance로 보면 된다.  

https://www.slideshare.net/nsimmons/11-x1-t05-05-perpendicular-distance


이 것이 왜 중요하냐 하면

plane segmentation을 할때 perpendicular distance를 통해 다른 plane과 merge를 할지 혹은 split을 할지 결정을 한다.
