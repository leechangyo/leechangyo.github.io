---
layout: post
title: Gaussian Distribution에서 Maximum LikelyHood(Log LikelyHood)를 구하고, 그의 용도
category: Machine Learning
tag: Machine Learning
---

<a href="https://postimg.cc/fJBMMPgs"><img src="https://i.postimg.cc/XY0qkSfr/maxresdefault.jpg" width="700px" title="source: imgur.com" /><a>

Maximum LikelyHood는 Modeling이나 classificiation, 최적의 Model을 선택하는데 많이 사용이 되고, 또한 Least Square를 통해(Gaussian Distribution에 Means값이 최대 likelihood가 되도록 계속적으로 최적화, "홀쭉해 지게 하기") 미지수에 대한 최적의 해를 구하는데도 사용하게 되는데, 최적화 문제를 푸는데 있어서 Maximum LikelyHood보다 log-likleyhood를 사용하여서 더 쉽게 풀 수 있다.

likleyhood의 크기가 커지면 커질수록 optimization 프로세스에 불리하다, 그러므로 log를 이용하여서 수를 줄이고 이를 통해 optimization을 한다.


The log-likelihood is usually easier to optimize than the likelihood function.

Log likelihood is used in statistics and machine learning because of calculus. It's easier to take the derivative of a log of a product than it is of the product itself. The likelihood is a big product, often of exponential functions, such as this one when you're working with normal distributions.

즉, likelihood를 통해서 최적화를 통해 미지수를 구할 수 있을 뿐만 아니라, 주어진 데이터와 Gaussian Modeling된 Model에 대해 classificiation을 할 수 있다.

[Super Well Maximum LikelyHood Explain](https://towardsdatascience.com/probability-concepts-explained-maximum-likelihood-estimation-c7b4342fdbb1)
