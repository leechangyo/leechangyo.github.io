---
layout: post
title: EightPoints Algorithm(To find Essential Matrix way)
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
5. now we can generate triangulation point and to do PnP solve camera pose estimatation
