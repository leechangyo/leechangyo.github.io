---
layout: post
title: Running 중 생기는 hyperopt error
category: Programming
tag: Programming
---

만약 이런 문제가 발생한다면

```
TypeError: __init__() got an unexpected keyword argument 'n_iter'
```

설치된 버전이 올드버전이므로 sklearn이 올드버전이 필요하다 그러므로 아래 것으로 설치하면 문제 해결=

```
pip install git+https://github.com/hyperopt/hyperopt-sklearn.git
```
