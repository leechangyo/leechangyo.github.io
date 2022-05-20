---
layout: post
title: PCA and SDV Difference
category: calculus
tag: calculus
---


Principal Component Analysis, or PCA, is a well-known and widely used technique applicable to a wide variety of applications such as dimensionality reduction, data compression, feature extraction, and visualization. The basic idea is to project a dataset from many correlated coordinates onto fewer uncorrelated coordinates called principal components while still retaining most of the variability present in the data.

Singular Value Decomposition, or SVD, is a computational method often employed to calculate principal components for a dataset. Using SVD to perform PCA is efficient and numerically robust. Moreover, the intimate relationship between them can guide our intuition about what PCA actually does and help us gain additional insights into this technique.

[한국어 설명](https://ratsgo.github.io/from%20frequency%20to%20semantics/2017/04/06/pcasvdlsa/)

[Well Known about between PCA and SDV Difference](https://intoli.com/blog/pca-and-svd/)

위에 말을 따르면
SVD는 PCA의 Application일종이다.

즉, 목적은 covariance Matrix에 대해 Matrix Decomposition을하여 EigenVector와 EigenValue를 구하는 것이고 PCA는 말 그대로 egienvalue가 높은 eigenvector들을 가지고 주성분(기저)로 normal surface, plane or line parameter, rotation matrix 등 다양한 분야에서 사용할수 있다.
