---
layout: post
title: Singularity and embedding vector, orthgonal meaning
category: calculus
tag: calculus
---

### Singularity

Singularity란 무엇인가. Matrix에서의 Linear dependency이다.

즉 A라는 행렬이 있다면, 행렬의 역수 A^-1이 0이라는 점이고, 이는 최적화 할때 Linear Problem이나 Non Linear Problem문제를 풀때 Least Square를 많이 사용하는데, 미지수에 대한 값을 구할 수 없다는 것이다.

Singularity Matrix를 푸는 방법은 없다.


### embedding vector

embedding vector : Feature Vector(like input image)

즉 특징을 가진 벡터로, classificiation을 할 때 사용이 되거나, 특징 적인 control 제어할때 사용이 된다.


### ORthogonal Matrix Represent Rotation Matrix

A * A^-1 = I , A * B = I <- ORthogonal Matrix이다.

Rigid motion camera rotation을 구할때 많이 사용된다.


### Reference

[ROTATION MATRIX BY SVD(ORthogonal)](https://www.google.com/url?sa=i&url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F15940663%2Fcorrect-way-to-extract-translation-from-essential-matrix-through-svd&psig=AOvVaw3Ht5aWtri0DRZJ9-ov96TL&ust=1644734882795000&source=images&cd=vfe&ved=0CAsQjRxqFwoTCLj86oXJ-fUCFQAAAAAdAAAAABAe)

[orthogonal matrix reprenst rotation matrix](https://www.quora.com/What-is-the-difference-between-an-orthogonal-matrix-and-a-rotation-matrix)
