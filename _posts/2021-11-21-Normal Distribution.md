---
layout: post
title: The Normal Distribution Transform lidar slam(理论，实战)
category: SLAM
tag: SLAM
---

### Main Concept

正态分布变换算法是一个配准算法，他应用于三维点的统计模型，使用标准最优化技术来确定联合和点云间的最优的匹配，因为其在配准过程中不利用对应点的特征计算和匹配，所以时间比其他方法快。即类似与图像匹配一样的三维点云匹配。

NDT算法使用牛顿法进行参数优化

当使用NDT配准时，目标是找到当前扫描的姿态

### STEP

```
1. 将空间（reference scan）划分成各个格子cell

2.将点云投票到各个格子

3.计算格子的正态分布PDF参数

4.将第二幅scan的每个点按转移矩阵T的变换

5.第二幅scan的点落于reference的哪个 格子，计算响应的概率分布函数

6.求所有点的最优值，目标函数为
```

즉, 그리드 셀을 정하고, epsilon, stepsize를 정한다.

```c++
// Setting scale dependent NDT parameters
// Setting minimum transformation difference for termination condition.
ndt.setTransformationEpsilon (0.01);
// Setting maximum step size for More-Thuente line search.
ndt.setStepSize (0.1);
//Setting Resolution of NDT grid structure (VoxelGridCovariance).
ndt.setResolution (1.0);

// how many update
ndt.setMaximumIterations (35);
```

그 다음 first scan 데이터를 가지고 셀에 포함된 포인트들의 mean(q) 과 covariance matrix를 만든다.

+) 초기 intial transformation matrix값은 odometry 값이나 zero로 한다.

만든 이후, second scan 데이터(x)를 지정한 셀에 투입을 하여서 PDF를 구하는데, 우리는 이에 대한 PDF 합을 score처리를 할 것이다. "score(p)=sum of exp((-(x-q)^t * Covariance matrix * (x-q))/2)"

여기서 p는 vector of parameters = (tx, ty, theta) 이다.

우리의 목적은 이제 score를 높임(optimal parameter)를 얻음으로써 모든 포인트를 업데이트를 해주고 R, T를 구하는 것이다.

이에 사용하는 방법은 최적화 알고리즘인 Newton algorithm을 사용한다.

```

//Newton algorithm
H * delta_p = -g;

// updating parameter p.
p <- p + delta_p

```

Newton Algorithm으로 p파라미터를 최적화를 할때, 위에 set한 stepsize와 epsilon을 이용하여서 iterations 수 만큼, Converge 될때까지 한다.

얻어진 p의 데이터 값은, R,t 값으로 second scan 데이터를 업데이트 시켜주고, 로봇의 포즈를 얻는다.

이 작업을 반복적으로 한다. incrementaly 하게 반복적으로 한다.




### 3D

https://blog.csdn.net/qq_40216084/article/details/107618766

https://zhuanlan.zhihu.com/p/94993060

### 2D

https://www.cnblogs.com/chenlinchong/p/12023978.html


### 综合

https://www.cnblogs.com/yhlx125/p/5749770.html


### CODE

https://gitee.com/xiaojake/xchu_slam/tree/master/xchu_slam

https://github.com/search?q=ndt+slam

https://github.com/XiaoJake/xchu_slam/blob/master/xchu_slam/src/xchu_slam.cpp
