---
layout: post
title: Affine transformation 특징 및 Eigen Expression
category: Machine Vision
tag: Machine Vision
---

Affine transformation is a linear mapping method that preserves points, straight lines, and planes. Sets of parallel lines remain parallel after an affine transformation. The affine transformation technique is typically used to correct for geometric distortions or deformations that occur with non-ideal camera angles

도움이 되는 자료들

https://articulatedrobotics.xyz/5-transformation_matrices/

https://www.researchgate.net/figure/Process-of-coarse-alignment-a-The-reference-face-and-input-face-b-3D-affine_fig3_266681108

https://www.youtube.com/watch?v=E3Phj6J287o

https://towardsdatascience.com/image-geometric-transformation-in-numpy-and-opencv-936f5cd1d315

https://blog.csdn.net/qq_37174526/article/details/100137483

https://www.brainvoyager.com/bv/doc/UsersGuide/CoordsAndTransforms/SpatialTransformationMatrices.html

https://stackoverflow.com/questions/45637472/opencv-transformationmatrix-affine-vs-perspective-warping

https://fivedots.coe.psu.ac.th/~ad/jg/ch139/index.html

https://en.wikipedia.org/wiki/3D_projection


#### Eigen Expression

```c++
// g++ eigen_test.cpp -o eigen_test -I /usr/include/eigen3/

// dynamic block expression.
Eigen::Quaternionf qvec_final(final_transform.block<3, 3>(0, 0)); // array size, position

// read it
Eigen::Vector3f tvec_final = final_transform.block<3, 1>(0, 3); // array size, position

```

easy test to do very good.
