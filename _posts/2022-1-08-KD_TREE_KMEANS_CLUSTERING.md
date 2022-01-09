---
layout: post
title: KD-TREE and K-Means clustering Implementation
category: Machine Learning
tag: Machine Learning
---

### KD-Tree

바이너리 형태로 트리를 작성을 하며 Search에 대한 time Complexity(klog2)로 가져가게 된다.

다차원 공간에서 Search를 하는데 많이 사용되며, 이때 Dependancy로 Distance Threshold와 Tree maximum leaf

들이 있다.

Depth level에 의해서 Binaray Tree가 형성이 되는데, 즉 2차원 x,y를 생각 할 때 Root는 Depth 0으로 x데이터에 대해서 Parent leaf가 형성이 되고, 다음 데이터가 들어왔을떄 Depth 1 Level로 y에 대한 데이터를 Parent y값과 비교해서 tree를 형성을 하면서 구간 구간을 만들게 된다.

<a href="https://postimg.cc/cvLJQg5n"><img src="https://i.postimg.cc/4xzhRp6B/2.png" width="700px" title="source: imgur.com" /><a>

<a href="https://postimg.cc/hzLgFc5n"><img src="https://i.postimg.cc/fLFb9b7V/clusterkdtree.png" width="700px" title="source: imgur.com" /><a>


실제 포인트클라우드에서 주로 Segmentation 이후에 나눠어진 Ground와 Unknown Classification에 대한 데이터가 구분이 되어 있을 때, Unknown에 대한 데이터를 kd-tree로 Tree를 만든 다음에 point를 하나 하나 방문하면서 주변 neihbor 포인트가 특정 Distance Threshold안에 들어 오게 되면 같은 Class로 define하여서 clustering을 하게 된다.(정의가 된 포인트에 대해서는 neihbor를 설치할때 마크가 되어있어서 스킵하게 된다.)


<a href="https://postimg.cc/WqSv3SVf"><img src="https://i.postimg.cc/SjbNgtvq/Depiction-of-nearest-neighbor-search-udacity-in-a-kd-Tree-The-kd-Tree-architecture.png" width="700px" title="source: imgur.com" /><a>

그러나 KD트리도 단점이 있는데, 만약 차원의 수가 계속 증가하게 되면, 서치에 대한 정확도가 떨어질 수도 있다.

<a href="https://postimg.cc/xkS0T6bn"><img src="https://i.postimg.cc/d0hD6Xsk/Kakao-Talk-Image-2022-01-09-11-35-59.jpg" width="700px" title="source: imgur.com" /><a>


### K-Means clustering

Machine Learning 쪽에서 많이 쓰인다. 특히 Bag of Word나 Object Detection Detector에서 주어진 데이터셋과 Label 데이터를 통해서 Learning을 통해 Classification Model을 만들거나(SVM, Bayes Naive 등).

주어진 unlabel된 데이터를 k라는 수 만큼 clustering을 하는데 사용이 된다.

<a href="https://postimg.cc/jWXrqtb4"><img src="https://i.postimg.cc/50c932KT/2314284-A5372-C25-D28.png" width="700px" title="source: imgur.com" /><a>

엄청 쉽다.

무수한 포인트들이 있다면, 그 중에 k 수만큼 랜덤으로 포인트를 잡는다(초기 k-means를 잡는다). 이후 포인트들 하나씩 k수 만큼 선정된 클러스터링 아이디를 중심으로 방문하면서 포인트들이 k 포인트들의 특정 distance Threshold에 들게 되면 해당하는 포인트들은 특정 k포인트로 그룹이 되고 그룹의 means 값을 만든다. 그룹에 대한 means값을 가지고 다시한번 포인트들을 k-menas값들에 방문을 하면서 특정 threshold에 포함이 되면 다시 groupping을 하고 다시 means값을 만드는 작업을 특정 수만큼 iteration을 하여서 k-means를 이용한 클러스터링을 하게 된다.

문제는 Dependancy가 K값이기 때문에 clustering할 객체에 수가 항상 랜덤할 경우 좋지 못하다.

또한 timecomplexity가 O(n^2)이므로 데이터셋이 커지면 커질수록 비효율적이다.


### Code Reference

https://github.com/Isacarthuso/KDtree

https://zhuanlan.zhihu.com/p/64180937

https://github.com/jacquesmats/Lidar_Obstacle_Detection
