---
layout: post
title: Orthogonal procrustes problem(to find Rotation and Translation/3D RIGID TRANSFROMATION)
category: Machine Vision
tag: Machine Vision
---

## Fiducial Marker나 객체에 대한 pose를 찾는 방법 중 하나이다.

 one is given two matrices A {\displaystyle A} A and B {\displaystyle B} B and asked to find an orthogonal matrix Ω {\displaystyle \Omega } \Omega which most closely maps A {\displaystyle A} A to B {\displaystyle B} B. [2]

 즉, A와 B를 매핑하는 Orthgonal matrix(Rotation Matrix)를 찾는 법인데, 두 매트릭스에서의 엘레멘트 평균값 - 매트릭스 에 대해 dot product을 해서 H 행렬을 만들고, SVD를 통해 Rotation matrix를 구한다.

 최소 4개의 점을 가지고 있는 것이 좋다.

 ```python
# Integral 코드에서 쓰이는 Procrustes analysis
def rigid_transform_3D(A, B):
	centroid_A = np.mean(A, axis = 0) // all point mean value of x y z
	centroid_B = np.mean(B, axis = 0) // all point mean value of x y z
 	H = np.dot(np.transpose(A - centroid_A), B - centroid_B)  // all points of raw x y z value - mean value, dot product A,B number of points become 3x3
 	U, s, V = np.linalg.svd(H)
 	R = np.dot(np.transpose(V), np.transpose(U))
  # 만약 Rotation Matrix에 대한 Determinant가 -이면
  # 우리가 바라보고 있는 공간상에서의 flip이 되어 있는 상태이므로
  # z축을 기반으로 -를 해주면 flip이 되서 우리가 바라보고 있는 공간상의 rotation matrix를 얻게 된다.
 	if np.linalg.det(R) < 0:
 		V[2] = -V[2]
 		R = np.dot(np.transpose(V), np.transpose(U))
 	t = -np.dot(R, np.transpose(centroid_A)) + np.transpose(centroid_B)
 	return R, t
def rigid_align(A, B):
	R, t = rigid_transform_3D(A, B)
	A2 = np.transpose(np.dot(R, np.transpose(A))) + t
	return A2

```

이것에 대한 설명 개굿

https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/04/06/pcasvdlsa/

https://math.stackexchange.com/questions/947867/how-to-prove-invariance-of-dot-product-to-rotation-of-coordinate-system

https://redstarhong.tistory.com/55 [

https://redstarhong.tistory.com/55

https://mathworld.wolfram.com/FrobeniusNorm.html

https://simonensemble.github.io/2018-10/orthogonal-procrustes.html

https://en.wikipedia.org/wiki/Orthogonal_Procrustes_problem
