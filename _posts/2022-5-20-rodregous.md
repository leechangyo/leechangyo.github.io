---
layout: post
title: Rodrigues formula rotation matrix and lie group relationship
category: calculus
tag: calculus
---

- Simple Notion
  - Lie Algebra :
    - Tangent Space, Local Coordinate (+,-), easy to calculate pose and orientation
    - Hirbert space(kind of)
    - Multi Geometry and Computer Vision Use a lot this lie Algebra
    - it is compose of vector
    - use logarithm (sum of multiply operation change to sum of +,- operation)
  - Lie Group :
    - Manifold Space, Global Coordinate (+,-,x,divived), it used in multi Geometry to show where land marks is and camera pose is in .
    - it is deep correlation with lie Algebra
    - it is compose of element in matrix we called Pose Vector and Roataion matrix
    - use exponential (change from vector in lie Algebra to matrix in lie group)(which means local coordinate to global coordinate)
  - Rodrigues Rotation formula
    - it also deeply related lie algebra and lie group.
    - rotation matrix has 3 DoF, but has 9 element, that's why it is not idial to use it in optmiazation method(which is many times has to calculate and even can meet gilmber lock or singularity because of more element number than 3 DoF)
    - therefore Rodrigues Roataion Formular change to 3 DoF and 3 Rotation Vector even within after normalization.
    - 3 rotation vector (Axis Angle)

- Process :
  - Lie Group - > Rodrigues Roation Formular(exponential map to logarithm map) -> Lie Algebra -> Optimization or calculate all the functiosn all the element -> Rodrigues Roation Formular(logarithm map to exponential map) ->Lie Group

In the theory of three-dimensional rotation, Rodrigues' rotation formula, named after Olinde Rodrigues, is an efficient algorithm for rotating a vector in space, given an axis and angle of rotation. By extension, this can be used to transform all three basis vectors to compute a rotation matrix in SO(3), the group of all rotation matrices, from an axis–angle representation. In other words, the Rodrigues' formula provides an algorithm to compute the exponential map from so(3), the Lie algebra of SO(3), to SO(3) without actually computing the full matrix exponential.


### Example
https://stackoverflow.com/questions/66279458/rodrigues-formula-to-convert-rotation-vector-to-rotation-matrix
