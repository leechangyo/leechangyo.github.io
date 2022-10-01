---
layout: post
title: EightPoints Algorithm(To find Essential Matrix and fundamental Matrix)
category: Visual SLAM
tag: Visual SLAM
---

the EightPoint Algorithm is an algorithm used in computer vision to estimate the Essential matrix or the fundamental matrix related to a stereo camera from a set of corresponding image point.

<a href="https://postimg.cc/Lq5ffnJy"><img src="https://i.postimg.cc/jjQhpftr/Screen-Shot-2022-03-23-at-11-13-35-AM.png" width="700px" title="source: imgur.com" /><a>

use Sequence Quadratic Program(Gradient Newton, LM ...) to solve unknown variable(Essential Matrix's element) iterately.

eightpoints is selected by RANSAC

**STEP**
1. image feature extraction
2. image feature matching(KNN)
3. use ransac outlier image feature matching's result
4. use ransac to find essential matrix and fundamental matrix with eight point algorithm.
5. now with eesstianl matrix and intrinsic parameter to make fundamental matrix.
6. and with fundamental matrix, using SVD to get Relative rotation Matrix and Traslation.(initialize pose)
7. now we can generate 3d triangulation point by them
8. and do PnP solve(3d - 2d) next camera pose estimation and literately generate 3d triangulation point(creating map) until camera frame no longer exist

## Reference

https://www.quora.com/Computer-Vision-What-is-the-difference-between-the-essential-matrix-and-the-fundamental-matrix

https://www.baidu.com/from=844b/s?word=essential+matrix+c%2B%2B&sa=tb&ts=9943862&t_kt=0&ie=utf-8&rsv_t=c6b9XlTmpQPfwQGN%252BwmWZENzj0Tikfj9D2s6z%252BCpYp13qJVswX7UAlr6NA&ms=1&rsv_pq=12478331738706715542&ss=&rqlang=zh&oq=essential%2Bmatrix%E6%B1%82%E8%A7%A3
