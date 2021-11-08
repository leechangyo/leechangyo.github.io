---
layout: post
title: Plane Segmentation RANSAC
category: Machine Vision
tag: Machine Vision
---

### PCL DOC :

https://pcl.readthedocs.io/projects/tutorials/en/latest/random_sample_consensus.html?highlight=console#the-explanation

good sample !

### Segmentation :

https://blog.csdn.net/cindy9608/article/details/119886698

Segmentation하는 법은 Normal Vector를 구하고 ax + by + cz + d = 0 (Plane coefficient)을 만족을 하면 Plane으로 인지

RANSAC Segmentation RANSAC

### well explained :

https://medium.com/@ajithraj_gangadharan/3d-ransac-algorithm-for-lidar-pcd-segmentation-315d2a51351


3차원에서의 RANSAC 방법

https://towardsdatascience.com/detecting-the-fault-line-using-k-mean-clustering-and-ransac-9a74cb61bb96

2차원에서의 ransac 방법

https://stats.stackexchange.com/questions/267267/what-does-scaling-the-normal-vector-of-a-plane-hyperplane-mean

### Summary :

RANSAC Plane Segmentation를 사용하는 이유는 모든 포인트들을 Ellipse 혹은 rectangle화 하여서 Normal vector(normal surface를)를 구하거나 모든 포인트의 네이버 3개를 이용하여서 normal vector(normal surface)를 구해 얻어진 plane coefficient를 raw point들을 대입하여 distance "d"를 구하여서 Threshold안에 들면 plane에 존재하는 inlier을 존재하거나 normal surface curvature를 통해 plane인지 object인지를 구분하는데 많은 calculation cost와 time cost가 생긴다.

그렇기 때문에 RANSAC을 이용한 Plane Segmentation이 효율적이고 많이 쓰이게 된다.

RANSAC은 머신러닝에서 혹은 컴퓨터 비전에 inlier 와 outlier를 나누는데 많이 쓰인다.

위에 쎃은 링크대로 램덤하게 포인트를 잡고 gradient(3차원일 경우 normal surface(plane), curvature를 구할 수 있다, xyz도 또한)를 구하여서 주변 포인트들이 특정 gradient와의 distance가 특정 Threshold안에 든다면, 그것인 inlier로 간주를 한다.

이를 iteration을 하여서 inlier를 구한다.

RANSAC PLANE Segmentation은 랜덤으로 3개의 포인트를 잡고 외적을 통해 normal surface plane을 구한다.

얻어진 plane coefficient와 raw point들을 통해 distance를 구하고 특정 Threshold 안에 든다면 plane으로 간주를 한다.

정해진 inlier 양만큼 구하고, iteration 반복 횟수 만큼 normal surface를 구하고 distance를 구한다.

+) 로 eigen value에 따라 엣지인지 플레인인지 닷인지 구분을 하고, xyz coordinate를 지정을 하여서 그라운드에 있는 플래인만 딸 수 도 있다.
