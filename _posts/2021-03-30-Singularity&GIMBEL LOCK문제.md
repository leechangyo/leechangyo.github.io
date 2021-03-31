---
layout: post
title: Singularity & Gimbar lock 문제
category: ROBOTICS
tag: ROBOTICS
---

Singularity는 Rotation Matrix혹은 Jacobian Matrix의 Determinant값이 "0" 되는 현상이다. 즉 싱귤러리티라고 한다면 더 이상 동작을 못하고 멈춘다라고 하면 된다.

Rotation Matrix같은 경우 싱귤러리티가 발생하면 정말 로봇의 조인트나 아니면 모바일 로봇의 회전 한계일수도 있지만, 않일 경우에도 이런 현상이 일어나기 때문에 문제가 된다. 그렇기 때문에 Quternion을 통해 계산량을 빠르게 하고, 또 싱귤러리티를 피하여서 사용된다.

Jacobian Matrix같은 경우에도 싱귤러리티가 발생하는데, 이때 미지수의 랜덤값을 구해야 되는 경우, SVD(Signgular Value Decomposition)을 통해 변수의 미지수 값을 구할 수 있다.

짐벌락 같은 경우 Rotation Matrix을 사용하다 보면 생기는 문제이다. 이 같은 경우에는 만약 Rotation Matrix는 3 rank로 되어 있는 행렬일탠데 오일러 회전과 같이 회전의 연산을 각 축의 성분으로 나누어 계산하기 때문이다. 이 또한 Quternion으로 해결이 가능하다.
