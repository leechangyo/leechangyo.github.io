---
layout: post
title: 2. Optimization application
category: Optimization method
tag: Optimization method
---

# Optimization in Signal and Image processing

## Model arising in compressive sensing and imaging sciences

- Separable(可分开的) Structures of problems

<a href="https://postimg.cc/zL2TZB2S"><img src="https://i.postimg.cc/vBbhnxWj/Capture.png" width="700px" title="source: imgur.com" /><a>

- Convexity and easy subproblems(子问题).
- Often involving large, nonsmooth convex functions with separable structure.
- [First-order method](https://math.stackexchange.com/questions/2201384/what-is-the-definition-of-a-first-order-method) can be very slow for producing high accuracy solutions, but also share many advantages:
  - Non-differentiability
  - Use minimal information, e.g.,(f;∇f)
  - Often lead to very simple and "cheap" iterative schemes
  - **Complexity/iteration mildly dependent (e.g., linear) in problem’s dimension**
  - Suitable when high accuracy is not crucial [in many large scale applications, the data is anyway corrupted or known only roughly

## Example: compressive sensing(Kernel 같은 개념)
- Goal: Recover sparse signal from very few linear measurements.
<a href="https://postimg.cc/gX55JGj3"><img src="https://i.postimg.cc/WzbP53PY/Capture.png" width="700px" title="source: imgur.com" /><a>
- Basis pursuit model:
<a href="https://postimg.cc/gLFDQjrt"><img src="https://i.postimg.cc/3NWcyDhh/Capture.png" width="700px" title="source: imgur.com" /><a>

## Compressive Sensing
- Find the sparest solution
  - Given n=256, m=128.
    - n is original signal
    - m is output value
  - A = randn(m,n); u = sprandn(n, 1, 0.1); b = A*u;
<a href="https://postimg.cc/pmT6D9jK"><img src="https://i.postimg.cc/3xCQQmJf/Capture.png" width="700px" title="source: imgur.com" /><a>

## Sparse((흔히 넓은 지역에 분포된 정도가) 드문, (밀도가) 희박한) recovery models
<a href="https://postimg.cc/YhjPFGYY"><img src="https://i.postimg.cc/wTkYSDBf/Capture.png" width="700px" title="source: imgur.com" /><a>
- Basis pursuit model : nonunique solution
- Ax = b = {$x^*$+N(A)} => ∀ v ∈ N(A), Av = 0
  - N(A) is non-space
- μ is given parameter(constant variable)

## Sparsity((흔히 넓은 지역에 분포된 정도가) 드문, (밀도가) 희박한) under a basis
- Given a reprenting basis or dictionary, we want to recover a signal which is sparse under this basis.
- Two types of models:

<a href="https://postimg.cc/1gGk58N6"><img src="https://i.postimg.cc/HLRdz5YS/Capture.png" width="700px" title="source: imgur.com" /><a>

- Commonly used dictionaries include both analytic and trained ones.
  - analytic bases: Id, DCT, wavelets, curvelets, gabor, etc., also their combinations; they have analytic properties, often easy to compute (for example, multiplying a vector takes O(n log n) instead of O($n^2$))
  - data driven: can also be numerically learned from training data or partial signal, patches (offline or online learning).
  - they can be orthogonal, frame, normalized or general.
  - **If Φ is orthogonal (or just invertible), the analyse based model is equivalent to synthesis based one by changing the variable. When it is not, the model is harder to solve.**

## Parallel MRI reconstruction
<a href="https://postimg.cc/GHvC73c4"><img src="https://i.postimg.cc/VLVkN5pW/Capture.png" width="700px" title="source: imgur.com" /><a>

## Nonconvex sparsity models
- min f (x) + h(x)
- where either f or h is nonconvex or both are nonconvex. Some popular models:
  - Nonlinear least square for nonlinear inverse problems f (x) = ‖F(x) − y‖^2.
  - h(x) = ‖x‖_p, where 0 <= p < 1 or rank constraint.
  - Alternating minimization methods in many applications, such as blind deconvolution, matrix factorization, or dictionary learning etc.

# Optimization of Matrices

## The Netflix problem
<a href="https://postimg.cc/cKDmg7My"><img src="https://i.postimg.cc/TwxHsCKw/Capture.png" width="700px" title="source: imgur.com" /><a>

## Matrix Rank Minimization
<a href="https://postimg.cc/tsdXGjLJ"><img src="https://i.postimg.cc/K8w4LcfP/Capture.png" width="700px" title="source: imgur.com" /><a>

## Video separation
<a href="https://postimg.cc/k2f6xKf7"><img src="https://i.postimg.cc/N0w6vxX2/Capture.png" width="700px" title="source: imgur.com" /><a>

## Sparse and low-rank matrix separation
- Given a matrix M, we want to find a low rank matrix W and a sparse matrix E, so that W + E = M.
- Convex approximation:

<a href="https://postimg.cc/xXmkZ8Gz"><img src="https://i.postimg.cc/y8QFkSkv/Capture.png" width="700px" title="source: imgur.com" /><a>

- Robust PCA (reduce dimension)

## Extension
<a href="https://postimg.cc/CZ644zwf"><img src="https://i.postimg.cc/QtjnhTPS/Capture.png" width="700px" title="source: imgur.com" /><a>

## Correlation Matrices
- A correlation(相互关系) matrix satisfies:
  - X = X^T , X_ii = 1, i = 1, . . . , n, X >= 0.
- Example: (low-rank) nearest correlation matrix estimation

<a href="https://postimg.cc/fVvZ1GLM"><img src="https://i.postimg.cc/t4KTnyGn/Capture.png" width="700px" title="source: imgur.com" /><a>

# Optimization in Machine learning

## Why Optimization in Machine Learning?
<a href="https://postimg.cc/PpB35qxY"><img src="https://i.postimg.cc/x8dWgJVx/Capture.png" width="700px" title="source: imgur.com" /><a>

## Loss functions in neural network
<a href="https://postimg.cc/3kZnMTvr"><img src="https://i.postimg.cc/Z5GgdYYN/Capture.png" width="700px" title="source: imgur.com" /><a>

## convolution operator
<a href="https://postimg.cc/Wth5KC8g"><img src="https://i.postimg.cc/QNqvBrW4/Capture.png" width="700px" title="source: imgur.com" /><a>

## Optimization algorithms in Deep learning: stochastic gradient methods
- pytorch/caffe2: adadelta, adagrad, adam, nesterov, rmsprop, YellowFin https://github.com/pytorch/pytorch/tree/master/caffe2/sgd
- pytorch/torch: sgd, asgd, adagrad, rmsprop, adadelta, adam, adamax https://github.com/pytorch/pytorch/tree/master/torch/optim
- tensorflow: Adadelta, AdagradDA, Adagrad, ProximalAdagrad, Ftrl, Momentum, adam, Momentum, CenteredRMSProp https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/kernels/training_ops.cc

## Generative adversarial network (GAN)
<a href="https://postimg.cc/SYjd0TMZ"><img src="https://i.postimg.cc/T1J4DBYX/Capture.png" width="700px" title="source: imgur.com" /><a>

## Reinforcement Learning
<a href="https://postimg.cc/0r3FkDy6"><img src="https://i.postimg.cc/Wbbv77R7/Capture.png" width="700px" title="source: imgur.com" /><a>

# Optimization in Finance

## Portfolio optimization
<a href="https://postimg.cc/mtPwr55R"><img src="https://i.postimg.cc/hPr32F8J/Capture.png" width="700px" title="source: imgur.com" /><a>

## Review of Risk Measures
<a href="https://postimg.cc/QVH5gWkJ"><img src="https://i.postimg.cc/8kbHSR79/Capture.png" width="700px" title="source: imgur.com" /><a>

# Reference

[ Optimization method - Standford University ](https://web.stanford.edu/class/ee364a/lectures.html)
