---
layout: post
title: Normal surface, normal vector 관한 이해
category: Machine Vision
tag: Machine Vision
---

Normal vector, Normal Surface를 만드는 것 자체가 Region 별로 그룹화를 해서 Mean, Covariance matrix를 통한 plane인지 edge인지 point인지 classified하고 혹은 3 set points을 cross product으로 통해 normal vector를 구할 수 있다.(이때 첫번째 point를 기준으로 normal vector를 만들게 된다.)

이렇게 되면 결국 데이터 index가 줄어 들게 된다.

예로

```
pcd::Pointcloud ptr = [ 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ,0 ]
```

이런 데이터가 있다고 한다면, Normal Vector, Normal Surface로 만든 이후에는 Normal Distribution 혹은 cross product으로 만들었다고 했을때, 아래와 같이 데이턱 될 것이다.

```
pcd::pointcloud nv = [  X ,X ,X ,X ,X ,X ,X ,X ,X ,X ,X,
                        X ,X ,X ,X ,X ,X ,X ,X ,X ,X ,X,
                        X ,X ,X ,X ,X ,X ,X ,X ,X ,X ,X,
                        1, 1 ,1 ,1 ,1 ,1 ,1 ,1 ,X ,X ,X,
                        1, 1 ,1 ,1 ,1 ,1 ,1 ,1 ,X ,X ,X,
                        1, 1 ,1 ,1 ,1 ,1 ,1 ,1 ,X ,X ,X,
                        2, 2 ,2 ,2 ,2 ,2 ,0 ,2 ,X ,X ,X,
                        2, 2 ,2 ,2 ,2 ,2 ,0 ,2 ,X ,X ,X,
                        2, 2 ,2 ,2 ,2 ,2 ,0 ,2 ,X ,X ,X,
                        2, 2 ,2 ,2 ,2 ,2 ,0 ,2 ,X ,X ,X,
                        0, 0 ,0 ,0 ,0 ,0 ,0 ,0 ,X ,X ,X,
                        1, 1 ,1 ,1 ,1 ,1 ,1 ,1 ,X ,X ,X,
                        1, 1 ,1 ,1 ,1 ,1 ,1 ,1 ,X ,X ,X ]

```

Normal vector가 1이면 plane, 0이면 edge, 2이면 points로 나눠지면서 데이터의 수가 적어 적어진다.(X는 데이터 어드레스에 데이터가 존재하지 않는다는 뜻)

+) 이런 식으로 Normal vector를 만들고 raw데이터를 normal vector로 부터 얻어지는 plane fomular model을 통해 distance를 구하고 egienvalues들을 통하여 plane의 curvature 만족하는 포인트들을 segment classified를 하는데 많이 쓰인다.

+) 만약 특정 Region(Ellipse, Rectangle)을 통한 means Covariance Matrix를 구하고 SVD를 통해 normal plane model을 구하고 이를 raw 포인트를 넣어서 distance(similarity)를 구하거나 eigenvalue로 인하여 curvature를 구할 수 있다.

+) 만약 2D라고 한다면 이는 covariance matrix나 cross product으로 얻어지는 line fomular이다.
